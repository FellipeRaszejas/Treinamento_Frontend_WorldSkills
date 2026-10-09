# Mapa de Estudo: Como Vite + React Funcionam Por Dentro

Objetivo: entender o sistema **da mesma forma que a máquina o entende**, para corrigir erros com precisão e competir na WorldSkills (Web Technologies).

> Este README é o mapa geral. Cada camada tem seu arquivo `.md` com o foco detalhado, checklist, autoavaliação e exercícios.

## Índice das camadas

| # | Arquivo | Tema |
|---|---|---|
| 0 | [00-fundamentos-web.md](00-fundamentos-web.md) | HTML, CSS, JS, ES Modules, Node, HTTP |
| 1 | [01-compilacao.md](01-compilacao.md) | Lexer, parser, AST, JSX, TypeScript |
| 2 | [02-vite-interno.md](02-vite-interno.md) | Dev server, pré-bundling, HMR, plugins, build |
| 3 | [03-react-interno.md](03-react-interno.md) | Render/commit, reconciliação, Fiber, hooks |
| 4 | [04-depuracao.md](04-depuracao.md) | Depurar como a máquina lê |
| 5 | [05-ecossistema-arquitetura.md](05-ecossistema-arquitetura.md) | SSR/SSG, router, estado, estilos, padrões |
| 6 | [06-qualidade-seguranca.md](06-qualidade-seguranca.md) | a11y, segurança, testes, lint, TS, Git |
| 7 | [07-performance-deploy.md](07-performance-deploy.md) | Web Vitals, cache, deploy, Docker |
| 8 | [08-fullstack-integracao.md](08-fullstack-integracao.md) | APIs, auth, back-end, banco de dados |
| 9 | [09-worldskills.md](09-worldskills.md) | Competição, tempo, simulados |

---

## Um esclarecimento que organiza tudo

Vite não é um framework, e React não é Vite. São **três máquinas** trabalhando em sequência:

| Máquina | Papel | Quando age |
|---|---|---|
| **Vite** (+ esbuild/Rollup/Rolldown, Babel/SWC/Oxc) | Ferramenta de build: lê, transforma e entrega o código | Antes e durante o desenvolvimento |
| **React** (+ react-dom) | Biblioteca de runtime: decide o que desenhar e atualizar | No navegador, com o app rodando |
| **Navegador** (parser HTML, CSS engine, V8) | Executa tudo e pinta a tela | No navegador |

Ao corrigir um erro, a primeira pergunta é: **em qual máquina ele aconteceu?**

Analogia com PHP: o pipeline é o mesmo conceito.

```
PHP:   código → lexer (tokens) → parser (AST) → opcodes → Zend VM executa
Vite:  JSX/TS → lexer → parser (AST) → transformação → JS puro → V8 executa
```

O JSX não existe para o navegador. Alguém (Babel, SWC, esbuild ou Oxc) faz o papel de lexer e parser antes.

---

## Resumo por camada

### Camada 0: Fundamentos de web
- **HTML/navegador:** critical rendering path, parser, `defer/async/module`, DOM, eventos, semântica
- **CSS:** cascata, especificidade, box model, stacking context, Flexbox, Grid, custom properties, container queries
- **JavaScript:** escopo, closures, `this`, protótipos, **event loop**, Promises, async/await, imutabilidade, igualdade por referência
- **ES Modules:** import/export estático, grafo de módulos, `import()`, ESM vs CJS, bare imports (conceito central do Vite)
- **Node/npm:** `package.json`, resolução do `node_modules`, semver, lockfile, npm/pnpm/yarn
- **HTTP:** status, headers, MIME, cache, CORS, cookies, WebSocket, HTTP/2

### Camada 1: Compilação (seu lexer/parser)
- Pipeline: código → tokens → AST → transformação → geração + source map
- AST (ESTree): `ImportDeclaration`, `CallExpression`, `JSXElement`...
- **JSX é açúcar sintático:** `<Button a="1">Oi</Button>` → `jsx(Button, { a: "1", children: "Oi" })`; runtime clássico vs automático
- **TypeScript:** o compilador só apaga tipos; checagem é o `tsc`, separado
- Transpilar ≠ empacotar ≠ minificar; tree-shaking depende de ESM; source maps
- Ferramentas: esbuild (Go), Rollup, Babel, SWC, Oxc/Rolldown (Rust). **Confirme na doc qual sua versão do Vite usa.**

### Camada 2: Vite por dentro
- **Dev server:** módulos ES nativos sob demanda, sem bundle inicial; `index.html` como entrada
- **Reescrita de imports:** `react` → `/node_modules/.vite/deps/react.js?v=hash`
- **Pré-bundling:** CJS→ESM e junção de dependências com muitos arquivos
- **HMR:** WebSocket, grafo de módulos, `import.meta.hot`, React Fast Refresh
- **Plugins:** `resolveId`, `load`, `transform`, `configureServer`, `transformIndexHtml`, `handleHotUpdate`, virtual modules, `enforce`
- **Config:** `defineConfig`, `alias`, `base`, `server.proxy`, `import.meta.env`, prefixo `VITE_`, `.env.*`
- **Assets:** `public/` vs `src/assets`, `?url/?raw/?inline`, `import.meta.glob`
- **Build:** code splitting, chunks, minificação, hash, `manifest.json`, `modulepreload`, `vite preview`
- **Dev ≠ prod:** causa de muitos bugs "só quebra no build"

### Camada 3: React por dentro
- Componente = função que retorna **elementos** (`{ type, props, key, ref }`); `UI = f(state)`
- **Trigger → Render → Commit → Effects**; reconciliação, diff, `key`, remount
- **Fiber:** current vs work-in-progress, work loop, lanes, time slicing, Suspense, transitions
- **Hooks:** lista encadeada no fiber (por isso a regra da ordem); snapshots de estado, stale closures, `useEffect` vs `useLayoutEffect`, `useRef`, `useMemo`, `useCallback`, `useReducer`, `useContext`
- **StrictMode:** renderiza e roda efeitos duas vezes em dev
- **react-dom:** `createRoot`, delegação de eventos, hidratação, portals
- **Performance:** `memo`, Profiler, React Compiler
- **React 19:** Actions, `use`, `useActionState`, `useOptimistic`, Server Components

### Camada 4: Depurar como a máquina lê
- Método: ler tudo → classificar → reproduzir → hipótese → confirmar
- Ferramentas: DevTools (Network, Sources, Elements, Application, Performance), React DevTools, astexplorer, `dist/`
- Tabela sintoma → máquina que falhou (completa no arquivo da camada):

| Sintoma | Quem falhou |
|---|---|
| Overlay "Unexpected token" | Parser (esbuild/Babel/SWC) |
| "Failed to resolve import" | Resolução do Vite |
| Tela branca + erro no console | Runtime JS / React |
| "Rendered more hooks than…" | Regra de hooks (fiber) |
| "Hydration mismatch" | react-dom (SSR) |
| Funciona em dev, quebra no build | Diferença dev/prod |
| `import.meta.env.X` undefined | Falta de `VITE_` ou `mode` errado |
| 404/CORS na API | Proxy, base ou HTTP |

### Camada 5: Ecossistema e arquitetura (novo)
- Renderização: CSR, SSR, SSG, ISR, streaming, hidratação; frameworks sobre React
- Roteamento (History API, React Router), estado (local, global, servidor, URL), TanStack Query
- Formulários e validação (React Hook Form, Zod)
- Estilização (CSS Modules, Tailwind, CSS-in-JS), design tokens, temas
- Padrões: composition, custom hooks, compound components, error boundaries
- PWA, i18n, Workers, tempo real

### Camada 6: Qualidade, a11y e segurança (novo)
- **Acessibilidade:** WCAG, semântica, teclado, foco, ARIA, leitores de tela
- **Segurança:** XSS, CSRF, CSP, CORS, tokens, segredos no front, supply chain
- **Testes:** Vitest, Testing Library, Playwright, mocks, MSW
- **Qualidade:** ESLint, Prettier, TypeScript estrito
- **Git:** rebase, bisect, PRs, CI

### Camada 7: Performance e deploy (novo)
- Core Web Vitals (LCP, INP, CLS), Lighthouse, análise de bundle
- Imagens, fontes, lazy loading, code splitting
- Cache HTTP e arquivos com hash
- Deploy: `base`, **SPA fallback**, Nginx, Docker multi-stage, variáveis em build

### Camada 8: Back-end, dados e integração (novo)
- REST, `fetch`, estados de UI, proxy vs CORS
- Auth: sessão, JWT, cookies, hash de senha, RBAC
- PHP/Node, MVC, SQL, modelagem, prepared statements (SQL injection)
- Docker Compose, migrations, `.env`

### Camada 9: WorldSkills
- Technical Description, Test Projects anteriores e marking scheme
- Design implementation, responsividade, a11y, código limpo, bugs em código alheio
- Gestão de tempo, simulados cronometrados, templates próprios (se as regras permitirem)

---

## Ordem de estudo sugerida

1. **Camada 0:** ES Modules + Node + `package.json` (a base do Vite)
2. **Camada 1:** pipeline de compilação; explore a AST e leia o JS transformado no Network
3. **Camada 2:** dev server, pré-bundling, HMR, plugins, build
4. **Camada 3:** render/commit, hooks por dentro, Fiber
5. **Camada 4:** depuração guiada pela tabela
6. **Camadas 5, 6, 8:** projeto completo (React + API + banco) com testes e a11y
7. **Camada 7:** medir, otimizar e fazer deploy
8. **Camada 9:** simulados cronometrados, repetidamente

## Projetos práticos que fixam o conhecimento
- [ ] Tokenizer + parser de uma mini-linguagem (seu lexer)
- [ ] Script Node com a API do Babel transformando JSX
- [ ] Plugin Vite (transform + virtual module)
- [ ] **Mini-React** (`createElement`, `render`, `useState`)
- [ ] **Mini-Vite** (servidor Node que transforma JSX e reescreve bare imports)
- [ ] Repositório com 15 bugs plantados para treinar depuração
- [ ] App full-stack com login, CRUD, testes e deploy em Docker

## Referências
- **Vite:** vite.dev (*Why Vite*, *Features*, *Plugin API*, *Build*, *Env and Mode*)
- **React:** react.dev (*Render and Commit*, *Escape Hatches*, *You Might Not Need an Effect*)
- **Build your own React** (Rodrigo Pombo) · **A Complete Guide to useEffect** (Dan Abramov)
- ***Crafting Interpreters*** (Robert Nystrom): lexer e parser na prática
- **MDN Web Docs:** HTML, CSS, JS, HTTP, ES Modules
- **web.dev:** performance, Core Web Vitals, acessibilidade
- **WCAG** (w3.org) e **WebAIM**
- **WorldSkills / SENAI:** Technical Description e Test Projects da sua competição

## Glossário rápido
- **Bare import:** `import x from 'react'` (sem caminho); o navegador não resolve sozinho
- **AST:** árvore que representa a estrutura do código
- **HMR:** troca de módulos em execução sem recarregar a página
- **Tree-shaking:** remoção de exports não usados
- **Chunk:** arquivo JS gerado pelo build
- **Reconciliação:** comparação entre árvores de elementos para atualizar o mínimo
- **Fiber:** estrutura interna do React que representa uma unidade de trabalho
- **Hidratação:** React assumindo um HTML já renderizado no servidor
- **SPA fallback:** servidor devolve `index.html` para rotas do front

> Lembrete: versões mudam rápido (Vite, React, Node). Antes de estudar um detalhe de versão, confirme na documentação oficial atual.
