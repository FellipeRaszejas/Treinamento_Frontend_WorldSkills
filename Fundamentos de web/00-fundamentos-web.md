# Camada 0: Fundamentos de Web

**Foco:** entender o ambiente onde tudo roda. Sem isso, Vite e React viram mágica. A peça central é **ES Modules**.

## 1. Navegador e HTML
- [ ] Critical Rendering Path: bytes → tokens → DOM; CSS → CSSOM; render tree → layout → paint → composite
- [ ] Como o parser de HTML funciona e o que bloqueia a renderização
- [ ] `<script>` normal vs `defer` vs `async` vs `type="module"` (módulos são deferred por padrão)
- [ ] DOM como árvore de objetos; `querySelector`, `createElement`, `textContent` vs `innerHTML`
- [ ] Eventos: captura, alvo, bubbling, `stopPropagation`, `preventDefault`, delegação
- [ ] Semântica HTML (`header`, `main`, `nav`, `section`, `button` vs `div`)
- [ ] Formulários nativos e validação (`required`, `pattern`, `FormData`)
- [ ] `<meta viewport>`, `<link rel="preload|modulepreload|prefetch">`

## 2. CSS
- [ ] Cascata, origem, especificidade, ordem, `!important`, `@layer`
- [ ] Herança e valores computados; box model; `box-sizing`
- [ ] Stacking context e `z-index`; `position`
- [ ] Flexbox e Grid (domínio total, é cobrado em competição)
- [ ] Custom properties (`--var`), `calc()`, `clamp()`
- [ ] Media queries, container queries, mobile-first
- [ ] Unidades: `rem`, `em`, `%`, `vw/vh`, `dvh`
- [ ] Transições, animações, `transform` (e por que não causa layout)
- [ ] Seletores modernos: `:is`, `:where`, `:has`, nesting
- [ ] CSS Modules, PostCSS, pré-processadores (Sass)

## 3. JavaScript (núcleo)
- [ ] Tipos, coerção, `==` vs `===`, truthy/falsy
- [ ] Escopo, hoisting, TDZ, closures, `this`, `call/apply/bind`
- [ ] Protótipos e classes
- [ ] **Event loop:** call stack, Web APIs, fila de microtasks (Promises) vs macrotasks (`setTimeout`)
- [ ] Promises, `async/await`, tratamento de erro, `Promise.all/allSettled/race`
- [ ] Destructuring, spread/rest, optional chaining, `??`
- [ ] `map`, `filter`, `reduce`, `find`, `some`, `every`; imutabilidade
- [ ] Igualdade por referência vs valor (base de por que React compara com `Object.is`)
- [ ] Memória: garbage collection, vazamentos comuns (listeners, timers, closures)

## 4. ES Modules (o conceito mais importante para o Vite)
- [ ] `import`/`export`, default vs named, re-export, `export *`
- [ ] Imports **estáticos** (analisados antes de executar) vs `import()` dinâmico
- [ ] Grafo de módulos, módulos executam uma única vez (singleton), live bindings
- [ ] Imports circulares
- [ ] ESM vs CommonJS (`require`/`module.exports`), UMD, IIFE
- [ ] **Bare import** (`'react'`) vs caminho relativo/absoluto; por que o navegador não resolve bare imports sozinho (import maps existem, mas o Vite reescreve)
- [ ] `import.meta`, `type="module"`, CORS em módulos

## 5. Node.js e ecossistema npm
- [ ] `package.json`: `scripts`, `dependencies`, `devDependencies`, `type`, `main`, `module`, `exports`, `engines`
- [ ] Resolução do `node_modules` (algoritmo de busca)
- [ ] Semver (`^`, `~`), lockfile, `npm ci` vs `npm install`
- [ ] npm vs pnpm vs yarn (como o `node_modules` difere)
- [ ] `npx`, binários em `node_modules/.bin`, versões do Node (nvm)
- [ ] Variáveis de ambiente, `process.env`, `.env`
- [ ] Terminal/Linux básico: `cd`, `ls`, `grep`, `cat`, pipes, permissões

## 6. HTTP e rede
- [ ] Request/response, métodos, status codes (200, 301, 304, 400, 401, 403, 404, 500)
- [ ] Headers importantes: `Content-Type` (MIME), `Cache-Control`, `ETag`, `Authorization`, `Origin`
- [ ] Por que o servidor precisa enviar `text/javascript` para módulos
- [ ] CORS e preflight (`OPTIONS`)
- [ ] Cookies (`HttpOnly`, `Secure`, `SameSite`), `localStorage`
- [ ] WebSocket (usado pelo HMR do Vite)
- [ ] HTTP/1.1 vs HTTP/2 vs HTTP/3 (por que servir muitos módulos ficou viável)
- [ ] DNS, TLS/HTTPS, CDN (noções)

## 7. Web APIs úteis
`fetch` + `AbortController`, History API, `URL/URLSearchParams`, Storage, `IntersectionObserver`, `ResizeObserver`, `requestAnimationFrame`, Web Workers, Service Workers, Clipboard, Drag and Drop.

## Autoavaliação
1. O que acontece, em ordem, entre digitar uma URL e ver a página?
2. Por que `import React from 'react'` falha em um HTML puro?
3. Qual a ordem de execução de `setTimeout`, `Promise.then` e código síncrono?
4. Por que um `<script type="module">` é diferente de um script comum?
5. Por que `{} === {}` é `false` e como isso afeta o React?

## Exercícios
- Montar uma página com 3 módulos ES nativos, sem nenhuma ferramenta
- Tentar importar `react` do `node_modules` direto no navegador e ler o erro
- Servir arquivos com Node puro (`http`) e errar o MIME de propósito para ver o erro
- Implementar delegação de eventos em uma lista dinâmica
