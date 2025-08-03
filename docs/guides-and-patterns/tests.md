---
sidebar_position: 7
---

# Testes de unidade e de componentes

Após configurar o [`Jest`](../architecture/jest.md) no padrão do app `project`, o projeto está apto a trabalhar com testes de unidade, componentes, snapshot e dentre outros.

## Bibliotecas

- [Jest](https://jestjs.io/) - Utilizado para execução dos testes, geração de cobertura, etc.
- [React Test Render](https://reactjs.org/docs/test-renderer.html) - Utilitário que possibilita renderizar componentes React em objetos JSON.
- [React Native Testing Library](https://callstack.github.io/react-native-testing-library/) - Utilitário que possibilita testar componentes no React Native com mais facilidade.
- [React Hooks Testing Library](https://react-hooks-testing-library.com/) - Utilitário que possibilita testar hooks com mais facilidade.
- [Axios Mock Adapter](https://github.com/ctimmerm/axios-mock-adapter) - Utilizado para criar mocks para as chamadas de API.

> Importante que você estude o básico da documentação de cada uma dessas bibliotecas, para te ajudar a tirar o melhor de cada uma durante os testes.

## Indicadores

Apesar da **cobertura de testes** não ser o indicador ideal para medir a qualidade do teste de software, ele ainda é um dos melhores indicadores que apontam se estamos no caminho certo.

O objetivo principal é que o app `project` caminhe sempre com **100% de cobertura nos testes**, ou o mais próximo possível.

## Teste de componentes

Os testes de componentes são realizados com o auxílio das bibliotecas `React Test Render` e `React Native Testing Library`, e devem seguir alguns critérios.

- No mínimo menos um teste de snapshot (se possível, um por variação de estado)
- No mínimo um teste para cada evento do seu componente, como `onPress`, `onChange`, `onFocus`, `onBlur`, etc.
- Alcançar o mínimo de cobertura definido nos **indicadores de teste** do app `project`. 

:::info IMPORTANTE
Sempre que um `bug` for corrigido em um componente, deve-se criar um teste para impedir que esse bug ocorra novamente no futuro.
:::

## Teste de API

Todas as chamadas para APIs `HTTP` do projeto são realizadas utilizando `axios`, por isso, nos testes devemos utilizar o pacote `Axios Mock Adapter` para possibilitar a criação dos mocks necessários.

:::tip
Veja a [documentação oficial](https://github.com/ctimmerm/axios-mock-adapter) do `Axios Mock Adapter` para entender como utilizá-lo.
:::

Assim como no testes de componentes, os testes de `API` também devem seguir o mínimo de cobertura definido nos **indicadores de teste** do app `project`. 
