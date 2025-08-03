---
sidebar_position: 3
---

# Guia de criação de Pull Requests

Pull Requests são a forma oficial de se enviar novas modificações de código para as branches principais de `dev`, `hmg`, `stage` e `master`. Todo e qualquer código adicionado, modificado, removido ou atualizado deve - obrigatoriamente - passar por revisão de pares em um Pull Request.

:::tip
**Materiais de apoio**

 - Como criar um Pull Request: https://www.atlassian.com/br/git/tutorials/making-a-pull-request
 - Guia para criação de Pull Request: https://www.atlassian.com/blog/git/written-unwritten-guide-pull-requests
 - (Ebook) What to Look for in a Code Review: https://leanpub.com/whattolookforinacodereview
 - (Vídeo) Code Review Best Practices: https://www.youtube.com/watch?v=a9_0UUUNt-Y
:::

E para que esse processo de revisão torne-se o mais seguro e tranquilo possível, existem algumas "regras de etiqueta" que devemos seguir em sua criação, revisão e aprovação.

## Criando um Pull Request

1. Adicione o código do card (tarefa do jira) no título do seu Pull Request

    Exemplo: `CORE-26: cria componente básico de input`

2. Adicione a descrição (descriptions) do que foi adicionado, atualizado ou removido em seu Pull Request, seguindo o template sugerido pelo GitHub:

    ```md
    ## Tarefas Resolvidas

    - [ ] Testei no iPhone SE 1 e no iPhone Pro Max
    - [ ] Testei no Android e no iOS
    - [ ] Preenchi o template do pull request
    - [ ] Garanti 100% de cobertura de testes nos arquivos que criei/modifiquei

    ## Visão geral 

    *Escreva aqui quaisquer informações importantes para os revisores ou testadores. Coloque o porquê de as modificações serem feitas e demais observações importantes.

    ## Modificações 

    *Escreva aqui as reais modificações que foram feitas no projeto. Adição de novas rotas, mudanças de comportamento e modificações no layout das telas ou recursos do projeto. Coloque **detalhes de como o recurso ou correção foi implementado ou possíveis biblioteca adicionadas.***

    *Você pode separar por tópicos e colocar uma descrição detalhada para cada item, se preferir. **Este item deve conter as informações técnicas.***

    ## Resultado obtido (prints, vídeos ou GIFs) 

    *Coloque aqui prints, vídeos ou até GIFs das telas e modificações que foram feitas com o resultado obtido após as modificações.*

    *Adicione também **prints e links do Figma**, para facilitar a validação do design.*

    ## Cobertura de testes

    *Adicione um print do gráfico de cobertura de testes gerado pelo comando `yarn test:coverage`.*

    ## Passos para testar 

    *(Opcional) Coloque aqui um passo a passo (menu, telas e campos navegados) que o usuário deve navegar e modificar para testar as modificações.*

    ## Anexos 

    *(Opcional) Coloque aqui possíveis arquivos que sirvam para realizar o teste do recurso. Faça upload ou coloque o link.*
    ```

3. Confira se o seu Pull Request não possui conflitos. Se existir, faça o `rebase` ou `merge` da sua `branch` com a `branch` de destino antes de finalizar.

4. Aguarde até que todas os **checks** tenham passado e então faça o `merge`.

## Revisando um Pull Request

1. Ao iniciar a revisão de um Pull Request, adicione-se como `Reviewer` no canto superior direito da página, para informar aos colegas que você já está revisando este código.

2. Adicione comentários ao código sempre que tiver dúvidas, sugestões ou correções que devem ser realizadas para que este Pull Request seja aceito (não esqueça de criar uma **Task** quando a correção for obrigatória).

3. Forme uma opinião em relação ao código que está revisando, faça o seguinte exercício: *Este código é bom o suficiente para ir para produção?*

4. Ao estar satisfeito com a qualidade do código revisado, aprove o Pull Request para que o autor possa realizar o **Merge**.
