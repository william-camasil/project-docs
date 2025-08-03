---
sidebar_position: 4
---

# Pipelines

https://docs.github.com/pt/actions

Atualmente possuímos pipelines de CI nos repositórios do Super App, tanto para push quando para pull requests.
Eles podem ser acessados em `https://github.com/microfrontend/<repository>/actions`.

Os arquivos que configuram os steps são arquivos yml que ficam na pasta `.github/workflows` de cada repositório. São separados por trigger (`push` ou `pull_request`) e por ambiente (`dev`, `hmg`, `stage` e `master`). Assim que um PR é criado ou um push é feito, o GitHub analisa a seção de `on` para saber se a pipeline referente ao arquivo analisado deve ser executada.

## Pull Request

A pipeline de pull request é executada sempre que um pull request é aberto para as branches de destino (`dev`, `hmg`, `stage` ou `master`) e é obrigatório que seja executada com sucesso para que o pull request possa ser concluído.
Esta pipeline inclui verificações de install, lint, teste e sonar.
O sonar é executado somente em cima dos arquivos modificados, por isso é importante possuir o sonar na pipeline de push para que todos os arquivos sejam analisados e o sonar seja atualizado por completo.

Devido à característica de multi repositórios do super app, é comum nos depararmos com desenvolvimentos que exigem mudanças em mais de um repositório para que uma feature seja concluída. Nesses casos, podemos referenciar as dependências project do package.json para uma branch específica que contenha as alterações necessárias, dessa forma:

`"project-context-mf": "git+ssh://git@github.com:microfrontend/project-context-mf.git#DEV-CORE-123"`.

Você pode enviar seu pull request com essa alteração no package.json, pois a pipeline de push irá corrigir as dependências para a branch de destino após o merge. É importante que o `yarn.lock` esteja atualizado com a branch referenciada no `package.json`. Caso contrário, a pipeline irá fazer um `yarn install` que tentará mudar o `yarn.lock`, o que não é permitido em um CI.
Mais detalhes na seção de pipeline de push abaixo.

## Push

A pipeline de push é executada sempre que um push é feito para as branches de destino (`dev`, `hmg`, `stage` ou `master`), seja por meio de conclusão de pull request ou por pushes diretos (que não são permitidos para os desenvolvedores devido às _branch protection rules_).
Ela contém os steps de install, lint, teste, sonar e ainda adiciona mais um step de correção de dependências entre o install e o lint.

O step de correção de dependências verifica o arquivo package.json e corrige as dependências project que referenciam branches diferentes da branch que foi feito o push. Exemplo: caso seja feito um push para hmg e haja uma dependência project referenciando a branch `HMG-CORE-123`, o script irá substituir `HMG-CORE-123` pela branch `hmg`. Após isso, ele executa o commit e o push para atualizar essas dependências no repositório remoto.

O commit de atualização das dependências é feito pelo usuário admin ProjectGestaoMudancas e possui na sua mensagem a frase `[skip ci]`, que faz com que o ci de push não seja executado novamente para este commit.
Por ser um usuário admin, ele possui as permissões de bypass das _branch protection rules_, incluindo a proteção que impede que um push seja feito diretamente para a branch e a proteção que exige que o CI seja concluído com sucesso.

:::caution Atenção
É importante que a action que executa a pipeline de push seja concluída com sucesso ANTES de iniciar a geração de versão do ambiente no jenkins. Caso contrário, o jenkins fará o checkout do código pra geração de versão ANTES do push de correção da action acontecer, pegando código desatualizado e ocorrendo erro ao fazer o push da atualização do upgrade-app-modules devido a branch local estar desatualizada.

Caso isso ocorra, basta reexecutar a geração de versão no jenkins após a conclusão das actions.
:::

Nesse step de checagem de dependências, o script irá:

- Receber o nome da branch de destino como parâmetro, vindo do comando `yarn fix-dependency-branches ${GITHUB_REF##*/}` no arquivo de pipeline;
- Executar a substituição das branches no package.json;
- Caso não tenha nenhuma alteração necessária, o script se encerra aqui;
- Fazer um `upgrade-app-modules` para atualizar os arquivos `.lock` com a branch correta;
- Caso haja alteração no `yarn.lock`, `package.json` ou `Podfile.lock`, o script irá commitar as alterações;

No step seguinte, é feito o push para o repositório remoto. Caso não tenham alterações, ele irá indicar "_everything up-to-date_".
Devido às _branch protection rules_, o push foi feito separado do script onde o commit é executado, pois a action usada no step de push interage via API com o repositório remoto e pode fazer o bypass caso o usuário que faz o commit tenha essa permissão.

Devido à essa sequência de passos, **é importante que os merges dos PRs sejam feitos em ordem de dependência**, ou seja, se o PR **B** depende do PR **A**, então o PR **A** deve ser mergeado antes do PR **B**.
Isso se deve ao fato de que a alteração do PR **A** deve estar na branch de destino para que o CI de push do PR **B** reconheça as alterações após o upgrade-app-modules.
Não é necessário aguardar as pipelines de push de um PR finalizarem para fazer o merge de outro PR, basta fazer o merge em ordem.

Essa é a lista de ordenações de dependência entre os repositórios atuais:

1. `babel-preset-mf-project`
2. `project-tools-mf`
3. `eslint-config-mf-project`
4. `project-context-mf`
5. `project-style-guide-mf`
6. `project-shared-mf`
7. Todos os repositórios "`app`". Exemplo: `project-login-mf-app` , `project-produtos-mf-app`, etc.
8. `project-core-mf` (que possui todos os anteriores como dependência)

> Em caso de dúvida, basta olhar no arquivo package.json para verificar quais repositórios são dependência do repositório atual.

Caso um PR seja mergeado fora de ordem, dependendo do erro que ocorrer devido a falta da branch correta, será necessário enviar outro PR para corrigir o repositório, atualizando a dependência e seu `yarn.lock`.

Se as dependências não tiveram suas branches alteradas para branches que não são a branch target, então não é necessário fazer o merge em ordem nem aguardar a action para geração de versão.

:::caution Atenção
Para fazer merges de PRs para hmg, stage ou master que possuem alteração de branch de dependência no package.json, é necessário deixar um aviso para o time de GMUD para que seja mergeado em ordem e que a versão somente seja gerada após a conclusão da action de push de todos os repositórios envolvidos.

Exemplo:

"Aguardar as actions do GitHub dos repositórios envolvidos finalizarem com sucesso antes de gerar versão. Podem ser acessadas em `https://github.com/microfrontend/<repository>/actions/workflows/push-build-hmg.yml`"

Substituindo a parte final pelo arquivo correto a depender da branch que está sendo atualizada
:::
