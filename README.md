# Diário de Bordo

Aplicação web de registro de atividades diárias, construída com HTML, CSS e JavaScript puros, sem framework, sem backend e sem banco de dados externo. Os dados ficam no `localStorage` do navegador.

## Funcionalidades

- Criar entradas com título, descrição e data
- Listar entradas, da mais recente para a mais antiga
- Remover entradas, com confirmação
- Persistência no navegador: os dados permanecem após recarregar a página ou fechar o navegador
- Funcionamento offline graças ao service worker
- Instalação na tela inicial (PWA), com o botão exibido quando o navegador permite
- Interface responsiva para celular e desktop

## Estrutura

```
diario-de-bordo/
├── index.html
├── style.css
├── script.js
├── manifest.json
├── service-worker.js
├── icons/
│   ├── icon-192x192.png
│   └── icon-512x512.png
└── README.md
```

## Como executar localmente

O service worker só funciona em `https://` ou em `localhost`. Por isso, não abra o `index.html` com duplo clique. Use um servidor local, por exemplo:

```bash
cd diario-de-bordo
python3 -m http.server 8000
```

Depois acesse `http://localhost:8000`.

## Como publicar no GitHub Pages

1. Crie um repositório público no GitHub, por exemplo `diario-de-bordo`.
2. Envie todos os arquivos desta pasta para a raiz do repositório.
3. Acesse **Settings → Pages**, escolha a branch `main` e a pasta `/ (root)` e salve.
4. Após alguns minutos, o app estará disponível em `https://SEU-USUARIO.github.io/diario-de-bordo/`.

Como todos os caminhos são relativos (`./`), o app funciona tanto na raiz quanto em subpastas.

## Como testar com o Lighthouse

1. Abra o app no Google Chrome.
2. Pressione `F12` (ou `Ctrl+Shift+I`) para abrir o DevTools.
3. Vá até a aba **Lighthouse**, marque **Progressive Web App** e clique em **Analyze page load**.
4. Verifique se a aplicação é reconhecida como PWA e se não há itens reprovados.

Para testar o modo offline, vá até **Application → Service Workers**, marque **Offline** e recarregue a página. As entradas devem continuar aparecendo.

## Atualizando a versão do cache

Ao alterar qualquer arquivo da aplicação, aumente a constante `VERSAO` em `service-worker.js` (por exemplo, de `v1` para `v2`). Assim, os visitantes recebem a versão nova em vez da versão guardada no cache.
