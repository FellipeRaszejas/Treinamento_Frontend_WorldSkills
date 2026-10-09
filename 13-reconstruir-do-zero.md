# Camada 13: Reconstruir do Zero (método anti-branco)

**Foco:** se der branco em algo mínimo, você consegue **substituir**, **montar do zero** e **explicar por que quebra**. O objetivo não é decorar sintaxe, é ter **modelos mentais, equivalências e um kit de peças que você já construiu à mão**. Esta é a camada que junta todas as outras.

> Ideia central: sintaxe se consulta em 10 segundos; **o modelo mental não se consulta**, é ele que diz o que procurar.

## 1. Os três recursos contra o branco
1. **Modelo mental:** saber *o que a máquina faz* (camadas 0, 1, 3, 10). Dá para reconstruir o comportamento mesmo sem lembrar o nome da função.
2. **Equivalências:** saber que a mesma coisa pode ser feita de outra forma (seção 3). Se esqueceu uma, usa a vizinha.
3. **Menor experimento:** escrever o mínimo no console/arquivo e **ver** o resultado, em vez de adivinhar.

## 2. Protocolo quando travar (6 passos)
1. **Dizer em português** o que eu quero que aconteça (entrada → saída)
2. **Identificar a máquina:** parser, resolução do Vite, runtime JS, React, navegador, rede?
3. **Descer uma camada:** qual é a primitiva mais simples que eu lembro? (ex.: esqueci `useEffect` → lembro que "roda depois do commit; o retorno limpa antes do próximo")
4. **Escrever o mínimo** (um arquivo, um console) e testar sozinho
5. **Consultar a documentação só para confirmar a sintaxe** (MDN, react.dev, vite.dev)
6. **Registrar** no caderno: o que travou, o que resolveu, a regra geral

## 3. Equivalências e substituições (X ≈ Y)
Aprender cada par significa: **saber fazer dos dois jeitos** e saber **o que muda**.

| Se esqueci... | Posso usar... | O que muda |
|---|---|---|
| JSX | `React.createElement` / `jsx()` | Sintaxe; é o que o compilador gera |
| `map` | `for…of` + `push`, `reduce` | Estilo; `map` não muta e retorna array |
| `async/await` | `.then/.catch`, callbacks | Legibilidade e tratamento de erro (`try/catch`) |
| `fetch` | `XMLHttpRequest` | `fetch` não rejeita em 404/500 (checar `response.ok`) |
| Arrow function | `function` | `this` e `arguments` |
| `import/export` | IIFE + objeto global | Escopo, singleton, análise estática |
| Destructuring/spread | `Object.assign`, atribuição manual | Cópia rasa; clareza |
| `useState` | `useReducer`, variável em closure + re-render manual | Como o React sabe que mudou |
| `useEffect` | `addEventListener` + cleanup manual | Quando roda (após commit) e dependências |
| `useRef` | objeto `{ current }` fora do render | Não dispara re-render |
| `useMemo/useCallback` | cache manual por dependências | Só otimização, não garantia |
| Input controlado | `FormData` + `ref` (não controlado) | Quem é a "fonte da verdade" do valor |
| React Router | History API + `popstate` + `switch` por path | Navegação sem recarregar |
| Context | prop drilling, store externa (`useSyncExternalStore`) | Quem re-renderiza |
| Flexbox | Grid, `margin: auto`, `inline-block` | Eixos e distribuição de espaço |
| Centralizar (3 formas) | flex, grid (`place-items`), `position`+`transform` | Contexto de layout |
| Tailwind / CSS Modules | CSS puro (BEM + custom properties) | Escopo e organização |
| Vite | sem build com **import maps**, webpack | Quem resolve imports e quando |
| `import.meta.env` | constantes de configuração | Em build, é substituição estática |
| Axios | `fetch` | Erros, interceptors, JSON |
| `localStorage` | cookie, IndexedDB, estado em memória | Persistência, segurança, tamanho |
| Classe | função fábrica + closure | `this`, instâncias, privacidade |
| `Promise.all` | loop com `await` | Paralelo vs sequencial |
| TypeScript | JSDoc + `// @ts-check` | Checagem em tempo de edição |
| Testing Library | asserções manuais no DOM | Consultas por papel/rótulo |

## 4. Kit de reconstrução (monte do zero, sem consultar)
Meta sugerida: conseguir refazer cada item **sem olhar nada**. Quando travar, anote onde e revise a camada correspondente. Os tempos são só referência; ajuste ao seu ritmo.

**Web e JS (camada 0, 10)**
- [ ] Página HTML com **módulos ES nativos** (sem nenhuma ferramenta)
- [ ] **Delegação de eventos** em lista dinâmica
- [ ] **Debounce** e **throttle**
- [ ] **Wrapper de `fetch`** (JSON, `response.ok`, timeout, erro padronizado)
- [ ] **Event emitter** / mini store (`subscribe`, `dispatch`)
- [ ] Layout responsivo (cabeçalho, barra lateral, cartões) com **Flex e Grid**

**Compilação e Vite (camadas 1, 2)**
- [ ] **Tokenizer** de expressões aritméticas
- [ ] Script Node que **transforma JSX** com Babel
- [ ] **Mini dev server** (Node `http`: serve ESM e reescreve *bare imports*)
- [ ] **Mini bundler** (resolve imports, concatena módulos)
- [ ] **Plugin do Vite** (`transform` + módulo virtual)

**React (camada 3)**
- [ ] **`createElement` + `render`** recursivo
- [ ] **`useState` com array de hooks** (e provar por que a ordem importa)
- [ ] **`useEffect` simplificado** (dependências + cleanup)
- [ ] Lista com **`key`** e demonstração de remount
- [ ] **Mini router** com History API

**Formulários e CRUD (camada 11)**
- [ ] **Formulário** com validação nativa, erros acessíveis e foco no primeiro erro
- [ ] Listagem com **filtros, paginação e estado na URL**
- [ ] **Busca com debounce** + `AbortController` (resolver respostas fora de ordem)
- [ ] **Atualização otimista** com rollback
- [ ] **Modal acessível** (foco preso, `Esc`, `aria-*`)

## 5. "Por que quebra": 6 perguntas diante de qualquer bug
1. **O dado é o que eu acho que é?** (tipo, formato, `undefined`, string vs número)
2. **Rodou na ordem que eu acho?** (assíncrono, microtasks, efeitos)
3. **É a mesma referência ou uma cópia?** (mutação, igualdade por referência)
4. **Qual valor a closure capturou?** (stale closure, snapshot do estado)
5. **Qual máquina falhou?** (parser, Vite, runtime, React, navegador, rede)
6. **O ambiente é o que eu acho?** (dev vs build, cache, versão, maiúsculas, `base`)

### Os modos de falha mais comuns no front (e a pergunta que os revela)
| Modo de falha | Pergunta | Onde olhar |
|---|---|---|
| `undefined`/`null` inesperado | 1 | `console.log`, estado inicial, dado assíncrono |
| Resposta antiga sobrescreve a nova | 2 | `AbortController`, cleanup do effect |
| Estado "volta" ou valor antigo | 4 | Updater function, dependências do effect |
| Estado mutado e a tela não atualiza | 3 | Criar novo objeto/array |
| Lista pisca/perde estado | 3 | `key` estável |
| Efeito roda demais/de menos | 2, 4 | Array de dependências, StrictMode |
| Erro de import / só quebra no build | 5, 6 | Caminho, maiúsculas, `base`, dependência CJS |
| 404/CORS/MIME | 5 | Network, proxy, headers |
| CSS não aplica | 6 | Especificidade, ordem, `Computed`, stacking context |
| Mudança não aparece | 6 | Cache, hot reload, build antigo |

## 6. Rotina de reconstrução
- [ ] **1 vez por semana:** escolher 3 itens do kit e refazer **sem consultar**
- [ ] **Quando travar:** registrar no caderno (o que, por quê, regra geral) e revisar a camada do assunto
- [ ] **Repetir em 7 dias** os itens em que travou (revisão espaçada)
- [ ] **Antes de usar uma biblioteca nova:** escrever à mão uma versão mínima do que ela faz
- [ ] **Depois de consertar um bug:** reescrever o trecho de memória e anotar a causa em uma linha
- [ ] **Explicar em voz alta** (ou por escrito) o caminho de um clique até a tela

## Autoavaliação
1. Se eu esquecer `useEffect`, que modelo mental me diz o que procurar?
2. Consigo fazer a mesma tarefa de 2 jeitos diferentes (`fetch` + `then` e `async/await`)?
3. Consigo construir do zero um mini-React com estado e lista?
4. Dado um bug novo, consigo aplicar as 6 perguntas e chegar à camada que falhou?
5. Em quais itens do kit eu travo? (esses são o meu plano de estudo)

## Exercícios
- **Semana 1:** refazer sem consultar 5 itens do kit (web e JS)
- **Semana 2:** mini-React (`createElement`, `render`, `useState`) e mini router
- **Semana 3:** mini dev server e mini bundler
- **Semana 4:** CRUD com formulário, filtros na URL, otimista e testes
- Montar um **caderno de equivalências** próprio (a tabela da seção 3, com as suas anotações)
- Pedir a alguém para plantar 10 bugs e resolver aplicando só as 6 perguntas, sem adivinhar
