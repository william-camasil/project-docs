---
sidebar_position: 4
---

# Guia do desenvolvedor

- [Variáveis e funções](#1-nomenclaturas-de-variáveis-e-funções)
- [Padrões de projeto](#2-padrões-de-projeto)
- [React e React Native](#3-react-e-react-native)
- [Testes](#4-testes)

Estas são as diretrizes a serem seguidas no desenvolvimento dos projetos.

## 1. Nomenclaturas de variáveis e funções

### Deve
- 1.1. Utilizar o idioma inglês;
- 1.2. Utilizar o padrão camelCase;
- 1.3. Utilizar a regra [SID](https://github.com/kettanaito/naming-cheatsheet#s-i-d) (short, intuitive, descriptive);
- 1.4. Utilizar o padrão [A/HC/LC](https://github.com/kettanaito/naming-cheatsheet#ahclc-pattern).

### Não deve
- 1.5. Utilizar abreviações;
- 1.6. Inverter lógica em condições, se possível. Exemplo: `!isEnabled` ao invés de `isDisabled`.

## 2. Padrões de projeto

### Deve
- 2.1. Priorizar o componente `<Spacer size={} />` sobre margins no estilo;
- 2.2. Priorizar variants dos componentes sobre estilos personalizados;
- 2.3. Priorizar `early return` sobre `else`;
- 2.4. Priorizar dependências do expo sobre dependências da comunidade externa;
- 2.5. Nomear navegações aninhadas com o sufixo "Navigation";
- 2.6. Nomear types de resposta com o sufixo ResponseDataType;
- 2.7. Nomear types de requisição com o sufixo RequestDataType;
- 2.8. Nomear arquivos e pastas referentes a componente com UpperCamelCase. Outros padrões de arquivo podem ser consultados na documentação da [estrutura do projeto](../guides-and-patterns/project-structure.md);
- 2.9. Utilizar [white label](../architecture/white-label.md) para imagens e textos que contenham relações com a Project.

### Não deve

- 2.10. Utilizar `else if`;
- 2.11. Utilizar interfaces fora de classes;
- 2.12. Criar componentes/arquivos muito grandes se estes podem ser separados em componentes/arquivos menores;
- 2.13. Criar bordas, cores, opacidade, sombras, espaçamentos e estilos de texto com valores fixos, sem utilizar tema;
- 2.14. Importar itens de outros repositórios project que não são exportados explicitamente em seu index. Ex: `import X from 'omin-repo/lib/utils'`. Usar `import { X } from 'project-repo'`;
- 2.15. Importar itens com nome diferente do nome exportado. Ex: arquivo com `export default WHITE_LABEL;` importado com `import whiteLabel from './whiteLabel'`. Importar como `WHITE_LABEL`.
- 2.16. Importar [imagens](../guides-and-patterns/images.md) diretamente ao invés de importar pelo index do asset. Ex: `import Logo from '../.../../static/img/images/Logo'`. Importar com `import { Images } from './assets';` e utilizar com `<Images.Logo />`.

## 3. React e React Native

### Deve
- 3.1. Utilizar [useCallback](#usecallback) em funções passadas para componentes filhos;
- 3.2. Utilizar useCallback em funções presentes em outros hooks;
- 3.3. Utilizar [useMemo](https://pt-br.react.dev/reference/react/useMemo) para criar objetos (object/array) passados por propriedade para componentes filhos;
- 3.4. Utilizar useMemo em valores presentes em outros hooks;
- 3.5. Utilizar useMemo quando a computação do resultado do valor é custoso, independente do tipo;
- 3.6. Criar estilos estáticos com `StyleSheet.create` fora da função do componente;
- 3.7. Utilizar useViewStyles para estilos dinâmicos de views;
- 3.8. Utilizar useTextStyles para estilos dinâmicos de texto;
- 3.9. Utilizar useImageStyles para estilos dinâmicos de imagem;
- 3.10. Utilizar useAsync para chamadas assíncronas somente quando necessitar do uso do loading. Caso contrário, utilizar useCallback;
- 3.11. Utilizar useDidMount para callbacks que devem ser executados somente ao montar o componente;
- 3.12. Utilizar useDidMountAndUpdate para callbacks que devem ser executados ao montar o componente e ao atualizar seu array de dependências;
- [3.13](#usedidmountandupdate-vs-useeffect). Utilizar [useEffect](https://pt-br.react.dev/reference/react/useEffect) caso queira garantir que todas as variáveis utilizadas estejam presentes no array de dependências;
- 3.14. Remover inscrições (_event listeners_ e _timers_) na função de retorno do useEffect, useDidMount, ou useDidMountAndUpdate caso estes possuam uma.

### Não deve
- 3.15. Utilizar funções que retornam funções no jsx, de modo a contornar a exigência do `useCallback` pelo ESLint;
- 3.16. Utilizar **index** como [key de listas](https://pt-br.reactjs.org/docs/lists-and-keys.html#keys) de componentes, se possível.

### Com moderação,
- 3.17. Declarar [funções puras](#funções-puras) fora do componente.

## 4. Testes

### Deve
- 4.1. Respeitar a prioridade das [queries](https://callstack.github.io/react-native-testing-library/docs/guides/how-to-query);
- 4.2. Utilizar [getBy](https://callstack.github.io/react-native-testing-library/docs/api/queries#getby-queries-) ao invés de [queryBy](https://callstack.github.io/react-native-testing-library/docs/api/queries#queryby-queries) quando o elemento deve existir;
- 4.3. Aguardar o componente ser renderizado caso este use um provider;
- 4.4. Aguardar chamada assíncrona ao montar (caso exista) através de elementos da tela;
- 4.5. Criar um arquivo de test-utils somente no caso deste ser reutilizado por outros arquivos de teste ou para não sobrecarregar o arquivo
de teste;
- 4.6. Nomear o describe pai com o nome do componente sendo testado;
- 4.7. Priorizar um expect por it, se possível. Separar vários its ajuda a documentar os comportamentos que devem ocorrer na tela;
- 4.8. Utilizar [jest.mock()](https://jestjs.io/pt-BR/docs/jest-object#jestmockmodulename-factory-options) somente para mocks que podem ser utilizados em todos os testes, sem dependência de ordem de execução;
- 4.9. Utilizar [jest.spyOn()](https://jestjs.io/pt-BR/docs/jest-object#jestspyonobject-methodname) dentro do it caso precise realizar um mock específico para este bloco de teste;
- 4.10. Utilizar [UNSAFE_getByType](https://callstack.github.io/react-native-testing-library/docs/api/queries#unsafe_bytype) APENAS em componentes que serão utilizados **uma** vez na tela;
- 4.11. Utilizar [within](https://callstack.github.io/react-native-testing-library/docs/api/misc/other#within-getqueriesforelement) em casos onde se deve garantir que o resultado da query esteja dentro do componente desejado (como botões em modais);
- 4.12. Utilizar [waitFor](https://callstack.github.io/react-native-testing-library/docs/api/misc/async#waitfor) para fazer a asserção quando se deseja esperar que a condição seja atendida, mas esta condição não se utiliza do `getBy`;
- 4.13. Utilizar [findBy](https://callstack.github.io/react-native-testing-library/docs/api/misc/async#findby-queries) caso precise utilizar `waitFor` + `getBy`;
- 4.14. Criar mocks globais no repositório project-tools-mf quando estes forem padrões e apresentarem repetição entre testes.

### Não deve
- 4.15. Utilizar a função delay. Priorizar `find` ou `waitFor`;
- 4.16. Criar uma função de renderização da tela separada do escopo do teste caso a renderização seja simples ou não contenha repetição de código;
- 4.17. Nomear testes com frases que não complementam o "it". Exemplos: "it _request_ x successfully", "it _test_ home". Priorizar iniciar com "should";
- [4.18](#erros-de-teste). Construir testes que emitem erros ou warnings no console;
- 4.19. Utilizar `jest.spyOn()` para um bloco de teste e deixar que este mock afete outros testes (com [vazamento de escopo](#vazamento-de-escopo-em-testes)).

---

## Observações

### Funções puras

Funções puras (sem dependência externa e sem side effect) podem ser declaradas fora da função do componente, porém estas
permanecem na RAM mesmo após o componente não estar mais sendo utilizado.

### [useCallback](https://pt-br.react.dev/reference/react/useCallback)

useCallback mantém a referência da função argumento entre re-renderizações, e somente retorna outra referência caso alguma dependência mude.

Funções somente são iguais se estas apontam para o mesmo objeto/referência, e não se possuem o mesmo resultado. 

```javascript
const firstFunction = function() {
    return 1 + 2;
}
const secondFunction = function() {
    return 1 + 2;
}
firstFunction === secondFunction; // false
firstFunction === firstFunction; // true
```

useCallback deve ser utilizado quando esta função é passada para um componente filho (como um callback), e este filho
depende de uma verificação de igualdade referencial para evitar re-renderizações desnecessárias quando suas props mudam.
Seu outro uso é quando esta função é utilizada por outro hook, para evitar que este hook seja chamado sempre que a tela renderizar
(já que sempre que a tela é renderizada, uma nova função é criada).

### useDidMountAndUpdate vs useEffect

O ESLint obriga que todas as variáveis utilizadas pelo useEffect estejam declaradas no seu array de dependências. Já o useDidMountAndUpdate
não possui essa configuração. Caso queira que o callback seja executado somente quando parte das variáveis utilizadas atualizam, utilize
useDidMountAndUpdate.

### Erros de teste

Alguns erros e warnings são intermitentes e só ocorrem ao ter um processo lento de execução de testes ou quando todos os testes do
projeto são executados (`yarn test`). Garanta que, ao rodar todos os testes do projeto, nenhum erro seja emitido do seu teste. A _pipeline_ do
Pull Request também pode ser verificada para garantir que nenhum erro foi emitido.

Boa parte dos erros são de [act()](https://callstack.github.io/react-native-testing-library/docs/advanced/understanding-act) e ocorrem ao não esperar um comportamento se completar antes de disparar outro.
Por exemplo, em telas que implementam a DigitalSignatureModal, quando o input de senha é preenchido, o loading precisa ser completado antes que outro evento seja disparado, ou que o teste seja finalizado.

Procure seguir o fluxo da tela usando waitFor ou findBy para que o passo a passo seja executado.


### Vazamento de escopo em testes

Alguns métodos do jest setam mocks de maneira global. Utilizar métodos do `spyOn` que não terminam em `Once`, por exemplo, vão fazer este mock valer para toda chamada feita no método mockado e não somente na próxima chamada feita, não
garantindo que as chamadas estão mockadas somente no bloco **it** em que foi inicializada.

Caso necessite que todas as chamadas de um bloco **it** utilizem o mock, métodos que não terminam em `Once` podem ser utilizados, porém, os mocks devem ser limpos ou resetados ao finalizar o **it**.
Métodos para evitar vazamento de escopo ao final do **it** são: [mockRestore](https://jestjs.io/docs/mock-function-api#mockfnmockrestore) ou [mockClear](https://jestjs.io/docs/mock-function-api#mockfnmockclear) (a depender da implementação).
Também é possível se utilizar de [clearAllMocks](https://jestjs.io/docs/jest-object#jestclearallmocks) em um [beforeEach](https://jestjs.io/docs/api#beforeeachfn-timeout) fora das suits de teste.

Caso o mock deva ser utilizado por todos os testes, utilizar `jest.mock()` fora das suits de teste para afetar todo o arquivo de teste.
