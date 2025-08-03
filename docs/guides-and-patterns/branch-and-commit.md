---
sidebar_position: 2
---
# Branches e Commits

## Padrões para nomes de Branches

Você deve saber que `Project` é utilizado um processo para gestão de mudança dos projetos (conhecido como GMUD), esse processo é o responsável por ditar algumas regras que devem ser seguidas durante o desenvolvimento, para que seu código seja aceito e enviado para produção.

Uma das etapas mais importantes da GMUD é o padrão de nomeação das Branches, seja no ambiente de `dev`, `hmg`, `stage` ou `prod`.

Obrigatoriamente, a `branch` deve deixar claro qual é o CARD do Jira que ela está vinculada, bem como o ambiente (`dev`, `hmg`, `stage` ou `prod`) para qual ela é destinada, seguindo o seguinte esquema: `AMBIENTE-CARD-123`

Exemplos:

- `DEV-OPA-1142`: Branch do card `OPA-1142` para o ambiente de `DEV`.
- `HMG-CORE-123`: Branch do card `CORE-123` para o ambiente de `HMG`.
- `STAGE-CORE-123`: Branch do card `CORE-123` para o ambiente de `STAGE`.
- `FPC-1142`: Branch do card `FPC-1142` para o ambiente de `PROD`. Perceba que no ambiente de `PROD` você deve omitir o nome do ambiente no nome da branch.

## GMUD e criação de branch

As branches base são:

- `dev`: ambiente de desenvolvimento
- `hmg`: ambiente de homologação
- `stage`: ambiente de produção*
- `master`: ambiente de produção

Em um projeto tradicional, as branches de trabalho sempre são criadas a partir da `master`, e então são nomeadas de acordo com o ambiente para o qual serão destinadas.

No super app, devido a característica de trabalhar com repositórios distribuídos e estes repositórios serem instalados como dependência direto do `git`, existem alguns detalhes importantes que precisam ser levados em consideração no momento de manutenção das branches.

Por isso, sempre devemos criar a branch de trabalho a partir da branch de destino, por exemplo, tendo o card `CORE-123` para ser entregue:

- Criar a branch `DEV-CORE-123` a partir da `dev`
- Criar a branch `HMG-CORE-123` a partir da `hmg`
- Criar a branch `STAGE-CORE-123` a partir da `stage`
- Criar a branch `CORE-123` a partir da `master`

Deste modo, o fluxo recomendado é trabalhar na branch de `DEV-CORE-123` até concluir a implementação e abertura do [Pull Request (PR)](../guides-and-patterns/pull-request.md) para `dev`. Uma vez que o PR da `dev` foi aprovado, fazer `cherry-pick` das alterações da `DEV-CORE-123` para `HMG-CORE-123`, `STAGE-CORE-123` e `CORE-123`, e por fim abrir os PRs para a `hmg`, `stage` e `master`.

_\* A branch `stage` utiliza o ambiente de produção, porém é uma branch para homologação dos casos onde não é possível testar em dev ou hmg, ou em casos onde é possível testar em produção sem impactar o usuário final (verificação de compatibilidade das apis, por exemplo). As versões geradas em stage serão enviadas para o testflight e app tester, já as versões da master irão para a apple store ou google play store._

## Padrões de commits

Toda mensagem de commit é consistida de `cabeçalho`, `corpo` e `rodapé`, sendo obrigatório somente o cabeçalho.

```
<cabecalho>
<LINHA VAZIA>
<corpo>
<LINHA VAZIA>
<rodape>
```

### Cabeçalho

```
<tipo>: <resumo>
  │        │
  │        └─⫸ Descrição resumida no presente, não capitalizado e sem ponto final
  │
  └─⫸ Tipo do commit: build|ci|docs|feat|fix|perf|refactor|test
```

O `tipo` deve ser um dos seguintes:

- `build`: Alterações que afetam o sistema de build ou dependências externas
- `ci`: Alterações ao arquivo de configuração de Continuous Integration (CI) ou scripts
- `docs`: Alterações exclusivas de documentação
- `feat`: Adição de novas features
- `fix`: Correções de bugs
- `perf`: Alterações que melhoram desempenho
- `refactor`: Alterações que não corrigem bugs nem adicionam features
- `revert`: Commit de reversão
- `style`: Alterações de estilos apenas
- `test`: Alterações para adicionar testes novos ou corrigir existentes

O `resumo` deve prover uma descrição sucinta das alterações contidas no commit. Deve-se utilizar o verbo
no presente do indicativo utilizando a terceira pessoa do singular: Refatora, adiciona, atualiza, remove, etc.
Além disso, o resumo deve ser escrito em letras minúsculas e sem ponto final.

> feat: adiciona tela de login

### Corpo (opcional)

O corpo da mensagem permite que o commit seja explicado de forma mais extensa. Tem por objetivo explicar o porquê de tais
mudanças. Deve-se utilizar o tempo verbal no presente.

Apesar de ser um campo onde é possível descrever o commit em detalhes, este não exclui a necessidade de um bom título do
cabeçalho, pois alguns comandos de histórico mostram apenas o título do commit.
Da mesma forma, há commits que descartam a necessidade de um corpo por serem passíveis de serem descritos somente com o título.

> refactor: remove variável do path da API de cartões
>
> Este commit remove a variável utilizada no path da API de cartões pois esta não é utilizada por nenhum consumidor

Pode ser adicionado utilizando um segundo corpo de mensagem no comando de commit. Exemplo:
```bash
git commit -m "refactor: remove variável do path da API de cartões" -m "Este commit remove a variável utilizada no path da API de cartões pois esta não é utilizada por nenhum consumidor"
```

### Rodapé (opcional)

O rodapé pode conter informações sobre breaking changes ou descontinuações, além de servir para referenciar tickets do Jira
ou outros PRs que este commit está relacionado.

> refactor: remove variável do path da API de cartões
>
> Este commit remove a variável utilizada no path da API de cartões pois esta não é utilizada por nenhum
> consumidor
> 
> Corrige CORE-123

### Commits de reversão

Commits de reversão devem começar com `revert: ` seguidos do cabeçalho do commit que está sendo revertido.
Ex: `revert: refactor: remove variável do path da API de cartões`

O conteúdo do corpo deve conter informações sobre o SHA do commit sendo revertido no formato: `Reverte o commit <SHA>`, bem como
uma clara descrição da razão de estar sendo operado um commit de reversão.
