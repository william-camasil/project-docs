
# 🌐 Website da Documentação

Este site foi criado com o [Docusaurus](https://docusaurus.io/), um gerador moderno de sites estáticos ideal para documentação técnica.

## 📦 Instalação

Antes de tudo, instale as dependências com o comando:

```bash
npm run
```

> 💡 Certifique-se de ter o `Node.js` e o `Yarn` instalados na sua máquina.

---

## 💻 Ambiente de Desenvolvimento Local

Para rodar o site localmente e ver as alterações em tempo real:

```bash
npm run start
```

Isso vai iniciar um servidor local e abrir o navegador. Toda alteração feita nos arquivos será atualizada automaticamente sem precisar reiniciar o servidor.

---

## 🛠️ Build (Gerar os Arquivos Estáticos)

Para gerar os arquivos finais do site:

```bash
npm run build
```

Esse comando cria uma versão estática na pasta `build`, pronta para ser publicada em qualquer serviço de hospedagem de sites estáticos (como GitHub Pages, Vercel, Netlify, etc).

---

## 🚀 Deploy (Publicar o Site)

### Usando SSH:

```bash
USE_SSH=true npm run deploy
```

### Sem SSH (usando nome de usuário do GitHub):

```bash
GIT_USER=<SeuNomeDeUsuarioGitHub> npm run deploy
```

Se estiver usando o **GitHub Pages**, esse comando já cuida de tudo: constrói o site e envia para o branch `gh-pages`.
