# Camada 2: Funcionamento Interno do Vite

**Foco:** entender o que o Vite faz entre `npm run dev` e a tela, e entre `npm run build` e a pasta `dist/`. **Dev e produção são dois pipelines diferentes.**

## 1. Filosofia
- [ ] Por que bundlers tradicionais (webpack) ficam lentos em projetos grandes
- [ ] Dev: **módulos ES nativos sob demanda**, sem empacotar na inicialização
- [ ] Produção: bundling otimizado (Rollup/Rolldown, confira sua versão)
- [ ] Papel do `index.html` como ponto de entrada

## 2. Dev server
- [ ] Servidor HTTP baseado em middlewares (Connect-style)
- [ ] Fluxo de uma requisição: navegador pede `/src/App.jsx` → Vite resolve → carrega → **transforma** → entrega como JS
- [ ] Reescrita de imports (bare imports → caminhos servíveis)
- [ ] Cache HTTP: dependências com `?v=hash` e cache forte; código-fonte com 304
- [ ] Como o Vite trata CSS (injeta `<style>` via JS), JSON, imagens, SVG
- [ ] Portas, `host`, `server.proxy`, `server.fs`, HTTPS

## 3. Pré-bundling de dependências
- [ ] Converte CommonJS/UMD em ESM
- [ ] Junta pacotes com centenas de módulos internos em um só (evita cascata de requests)
- [ ] Pasta `node_modules/.vite/deps`, `optimizeDeps.include/exclude`, `--force`
- [ ] Sintoma típico: "nova dependência detectada, recarregando"

## 4. Grafo de módulos e HMR
- [ ] Module graph no servidor: importadores, importados, timestamps
- [ ] WebSocket entre servidor e navegador
- [ ] `import.meta.hot` (`accept`, `dispose`, `invalidate`)
- [ ] HMR boundaries: quem aceita a atualização
- [ ] **React Fast Refresh:** preserva estado; quando reseta (arquivo exporta não-componentes, hooks com mudança de ordem)

## 5. Sistema de plugins
- [ ] Hooks compatíveis com Rollup: `resolveId` → `load` → `transform`
- [ ] Hooks do Vite: `config`, `configResolved`, `configureServer`, `transformIndexHtml`, `handleHotUpdate`
- [ ] `enforce: 'pre' | 'post'` e ordem de execução
- [ ] Virtual modules (prefixo `\0`, `virtual:`)
- [ ] `@vitejs/plugin-react`: o que ele realmente faz (JSX + Fast Refresh)
- [ ] `apply: 'serve' | 'build'`

## 6. Configuração
- [ ] `defineConfig`, `mode`, `command`
- [ ] `resolve.alias`, `base`, `root`, `publicDir`
- [ ] `server.*`, `build.*`, `preview.*`
- [ ] Variáveis de ambiente: `import.meta.env`, prefixo `VITE_`, `.env`, `.env.local`, `.env.[mode]`, `loadEnv`
  - **Tudo que tem `VITE_` vai para o bundle público. Nunca coloque segredo.**
- [ ] `define` (substituição estática de constantes)

## 7. Assets
- [ ] `public/` (copiado como está, URL absoluta) vs `src/assets` (processado, com hash)
- [ ] Sufixos `?url`, `?raw`, `?inline`, `?worker`
- [ ] Inline de assets pequenos (`build.assetsInlineLimit`)
- [ ] `import.meta.glob` e `import.meta.url`

## 8. Build de produção
- [ ] Etapas: resolve → transform → bundle → minify → emit
- [ ] Code splitting: `import()` e `React.lazy` viram chunks
- [ ] `manualChunks`, chunks de vendor, `modulepreload`
- [ ] Hash de nomes (cache busting), `manifest.json`
- [ ] `build.target`, `browserslist`, polyfills
- [ ] `sourcemap`, `minify`, `cssCodeSplit`
- [ ] `vite preview` (serve o `dist/`, não é servidor de produção real)
- [ ] **Por que "funciona em dev e quebra no build"**: ordem de módulos, `process.env`, imports dinâmicos, case-sensitivity de arquivos, dependências CJS, `base` errado

## 9. Tópicos avançados
- [ ] SSR e a API de SSR do Vite; Environment API (confirme na doc da sua versão)
- [ ] Library mode, multi-page apps, Workers
- [ ] Monorepo, aliases e workspaces
- [ ] Comparar com webpack, Parcel, Rspack, Turbopack

## Autoavaliação
1. O que o Vite faz com `import React from 'react'` em dev?
2. Por que existe pré-bundling se o navegador já entende ESM?
3. Qual a diferença entre `public/` e `src/assets`?
4. Por que `process.env.X` pode falhar no Vite?
5. Como um plugin transforma um arquivo `.md` em componente?

## Exercícios
- Ler `localhost:5173/src/main.jsx` no DevTools → Network e comparar com seu código
- Abrir `node_modules/.vite/deps` e inspecionar
- Escrever um plugin que injeta um comentário em cada arquivo e outro com virtual module
- Rodar build, abrir `dist/assets/*.js` e identificar seu código no meio do bundle
- Mini-Vite: servidor Node que serve `.jsx` transformado com Babel e reescreve bare imports
- Usar `rollup-plugin-visualizer` para ver o tamanho de cada dependência
