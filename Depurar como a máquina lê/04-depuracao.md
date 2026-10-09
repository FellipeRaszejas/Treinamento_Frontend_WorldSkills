# Camada 4: Depurar Como a Máquina Lê

**Foco:** diagnosticar rápido. A habilidade-chave é descobrir **em qual máquina o erro aconteceu** (parser, resolução do Vite, runtime JS, React, navegador, rede).

## Método de depuração
1. **Ler a mensagem inteira**, de cima para baixo, incluindo arquivo e linha
2. **Classificar:** erro de build (terminal/overlay), de console (runtime), de rede (Network) ou visual (CSS)
3. **Reproduzir** de forma mínima
4. **Formular hipótese** e testar uma coisa por vez
5. **Confirmar** a causa antes de corrigir (não "chutar mudanças")
6. Se travar: isolar (comentar partes), `git diff`, `git bisect`

## Ferramentas
- [ ] **DevTools → Console:** erros, warnings do React, `console.table/trace`
- [ ] **Network:** ver o JS que o Vite entrega, status, headers, MIME, timing, cache
- [ ] **Sources:** source maps, breakpoints, conditional breakpoints, `debugger`, call stack
- [ ] **Elements:** DOM vivo, computed styles, box model, forçar `:hover`
- [ ] **Application:** storage, cookies, service workers
- [ ] **Performance / Lighthouse:** gargalos
- [ ] **React DevTools:** componentes, props, hooks, Profiler
- [ ] **Terminal do Vite:** erros de transformação aparecem aqui primeiro
- [ ] **astexplorer.net**, **rollup-plugin-visualizer**, `vite --debug`, `vite build --sourcemap`

## Tabela: sintoma → quem falhou
| Sintoma | Quem falhou | Onde olhar |
|---|---|---|
| Overlay "Unexpected token" / "Transform failed" | Parser (esbuild/Babel/SWC/Oxc) | Arquivo e linha indicados |
| "Failed to resolve import" | `resolveId` do Vite | Caminho, extensão, maiúsculas, alias, dependência instalada |
| "does not provide an export named X" | Módulo ESM/CJS | Named vs default, pré-bundling |
| Tela branca + erro no console | Runtime JS / React | Console + stack |
| "Objects are not valid as a React child" | Render | Você renderizou um objeto |
| "Cannot read properties of undefined" | Dado ainda não existe | Estado inicial, async, optional chaining |
| "Rendered more/fewer hooks than expected" | Regra de hooks | Hook condicional / return antecipado |
| "Maximum update depth exceeded" | Loop de render/effect | setState em render ou deps erradas |
| "Each child should have a unique key" | Reconciliação | Lista sem `key` estável |
| Estado "volta" ou valor antigo | Closure/snapshot | Stale closure, updater function |
| Effect roda duas vezes | StrictMode (dev) | Cleanup ausente |
| "Hydration mismatch" | react-dom (SSR) | Conteúdo diferente servidor/cliente |
| Funciona em dev, quebra no build | Dev ≠ prod | `base`, case de arquivo, `process.env`, imports |
| Variável de ambiente `undefined` | Config | Prefixo `VITE_`, `mode`, reiniciar dev |
| 404 ao recarregar rota | Servidor sem SPA fallback | Config do servidor/`base` |
| CORS error | Navegador/HTTP | Headers do servidor, proxy do Vite |
| `MIME type ... not "text/javascript"` | Servidor | Caminho errado devolvendo HTML |
| CSS não aplica | Cascata | Specificity, ordem, Computed |
| 304/cache velho | HTTP cache | Disable cache, hard reload |

## Tópicos
- [ ] Ler stack traces (e source maps)
- [ ] Erros sincronos vs assíncronos vs não tratados (`unhandledrejection`)
- [ ] Error boundaries e `window.onerror`
- [ ] Depurar rede (payload, status, CORS)
- [ ] Depurar CSS (layout, overflow, stacking)
- [ ] Depurar performance (long tasks, re-renders)
- [ ] Leitura rápida de código alheio

## Exercícios
- Criar um repositório com 15 bugs plantados (um por linha da tabela) e resolvê-los cronometrado
- Pedir para um colega plantar bugs no seu projeto
- Praticar `git bisect` em um histórico com regressão
- Para cada bug resolvido, escrever uma linha: "quem falhou / sintoma / causa / correção"
