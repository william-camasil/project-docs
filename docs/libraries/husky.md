# Instalação e configuração do Husky JS

O **Husky JS** é uma ferramenta de linha de comando que permite, através de githooks, a criação de gatilhos que são executados durante o `commit` e `push`.

Para configurar o **Husky JS** no projeto:

1. Garanta que você já instalou o [project-tools-mf](../architecture/tools.md).

1. Instale o Husky seguindo a documentação oficial: https://github.com/typicode/husky

    :::info
    Apesar da documentação do Husky indicar o uso do `prepare`, neste projeto é utilizado o `postinstall` devido ao uso do `yarn` v3 que não possui compatibilidade com o script `prepare`.
    :::

    Modifique o script `postinstall` adicionando o `husky` junto aos demais scripts existentes, exemplo:

    ```json
    "script": {
      ...,
      "postinstall": "yarn pod-install && husky"
    }
    ```

    Se estiver em um submódulo, utilize o pacote [`ignore-dependency-scripts`](https://github.com/douglasjunior/ignore-dependency-scripts) para garantir que o `postinstall` não será executado quando o submódulo estiver instalado como dependência no `core`.

    ```json
    "scripts": {
      ...
      "postinstall": "npx --yes ignore-dependency-scripts \"husky && yarn pod-install\""
    }
    ```

2. (Se Mac/Linux) Como utilizamos o **Node JS** com `nvm`, é preciso adicionar o arquivo `.huskyrc` no diretório do seu usuário (`$HOME` ou `~`) do sistema operacional.

    Crie o arquivo `~/.huskyrc` com o conteúdo:

    ```bash
    export NVM_DIR="$HOME/.nvm"
    [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
    ```

    > Instruções na documentação oficial: https://typicode.github.io/husky/how-to.html#solution

3. Adicione o script `projectdigital-pre-push` para ser executado antes de cada `git push`.

    <details>
        <summary> Se em submódulo </summary>
        <code>
            echo "projectdigital-pre-push" > .husky/pre-push
        </code>
        <br/>
        <code>
            git add .husky/pre-push
        </code>
    </details>

    <details>
        <summary> Se no core </summary>
        <code>
            echo "projectdigital-pre-push --core" > .husky/pre-push
        </code>
        <br/>
        <code>
            git add .husky/pre-push
        </code>
    </details>

    Esse script customizado irá rodar `npm test` e `npm run validate-dependencies`.

4. Adicione o script `projectdigital-pre-commit` para ser executado antes de cada `git commit`.
    ```bash
    echo "projectdigital-pre-commit" > .husky/pre-commit
    git add .husky/pre-commit
    ```

    Esse script customizado irá rodar `npm run lint` e `npm run tsc`.

5. Adicione o script do **Commit Lint** para validar as mensagens de commit:
    ```bash
    echo "commitlint --edit $1" > .husky/commit-msg
    git add .husky/commit-msg
    ```

    Esse script irá validar se as mensagens de commit estão dentro dos [padrões da `project`](../guides-and-patterns/branch-and-commit.md).
