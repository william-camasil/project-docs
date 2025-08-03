---
sidebar_position: 8
---

# Configuração das fontes

O app `project` utiliza fontes customizadas nos componentes, sendo assim, sempre necessário rodar um projeto que utilize o `style-guide`, é preciso realizar o `link` dessas fontes nos projetos nativos.

> Mais detalhes de como as fontes funcionam no React Native, e as diferenças entre Android e iOS: https://mehrankhandev.medium.com/ultimate-guide-to-use-custom-fonts-in-react-native-77fcdf859cf4

## Instalação e link

1. Copiar os arquivos de fonte para dentro do projeto, por exemplo, em `assets/fonts/` na raiz do projeto (fora da `lib` ou `src`).

  :::tip
  Você pode copiar os arquivos do `project-style-guide-mf`.
  :::

1. Criar o arquivo `react-native.config.js` e adicionar o caminho para os `assets` copiados no passo anterior.

    ```js
    module.exports = {
      // outras configs ...
      assets: ['./assets/fonts/'],
    };
    ```

1. No terminal, execute o `react-native-asset` na raiz do projeto (diretório onde fica o `package.json`)

    ```bash
    npx react-native-asset
    ```

    > Confira se os arquivos de fonte foram copiados para dentro da pasta `/android/app/src/main/assets` e se houveram modificações nos arquivos `Info.plist` e `project.pbxproj`.

## Usando as fontes

Após realizado o `link` das fontes, é possível utilizá-las em componentes `Text` dentro do app React Native.

Entretanto, existe uma limitação em relação aos nomes das fontes, que exige formas diferentes de uso para Android e iOS.

No **Android**, deve ser aplicado em `fontFamily` o nome completo da fonte, incluindo a densidade, exatamente como está no arquivo de fonte utilizado. Por exemplo, se o arquivo se chama `AvertaCY_Bold.ttf`, então:

```tsx
<Text
  style={{
    fontFamily: 'AvertaCY_Bold'
  }}
>
  Hello world
</Text>
```

:::info
Note que para o **Android** não deve ser utilizado o `fontWeight`.
:::

Já para o **iOS**, em `fontFamily` não é usado o nome do arquivo, mas sim o nome da fonte. Além disso, deve ser informado em `fontWeight` a densidade desejada, por exemplo:

```tsx
<Text
  style={{
    fontFamily: 'Averta CY',
    fontWeight: 'bold',
  }}
>
  Hello world
</Text>
```

Sendo assim, para que funcione nas duas plataformas de forma transparente, é interessante aplicar a fonte usando o `Platform.select`:

```tsx
<Text
  style={{
    ...Platform.select({
      android: {
        fontFamily: 'AvertaCY_Bold'
      },
      ios: {
        fontFamily: 'Averta CY',
        fontWeight: 'bold',
      }
    }),
    color: '...',
    fontSize: '...',
  }}
>
  Hello world
</Text>
```

:::info
Os temas e estilos gerenciados pelo `project-style-guide-mf` já estão preparados para trabalhar desta forma.
:::

Uma vez que você tenha acesso ao `theme`, é possível aplicar a fonte desejada usando:

```tsx
<Text
  style={{
    ...theme.typography.weights.bold,
    color: '...',
    fontSize: '...',
  }}
>
  Hello world
</Text>
```
