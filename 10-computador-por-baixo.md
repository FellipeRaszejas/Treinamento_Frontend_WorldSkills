# Camada 10: O Computador Por Baixo (motor JS, memória e renderização)

**Foco:** entender o que o navegador e o motor JavaScript fazem com o seu código, para explicar *por que* algo quebra, trava ou fica lento. É o equivalente, no front-end, do "lexer → parser → opcodes" do PHP. Nível: básico a avançado.

```
JS fonte → parser → AST → bytecode (interpretador) → código otimizado (JIT) → executa
HTML/CSS → DOM/CSSOM → estilo → layout → pintura → composição → tela
```

## 1. Dados e números (básico)
- [ ] **`Number` é um `double` IEEE 754**: por que `0.1 + 0.2 !== 0.3`; `Number.MAX_SAFE_INTEGER`; `BigInt`
- [ ] **Dinheiro:** nunca em float; usar inteiros (centavos) ou bibliotecas; `toFixed` arredonda e devolve string
- [ ] **Strings são UTF-16** (unidades de código): emoji pode ter `length` 2; `[...str]`, `Intl.Segmenter`, normalização (`normalize()`)
- [ ] **Coerção:** `"5" + 1`, `"5" - 1`, `[] + {}`, `==` vs `===`, `NaN !== NaN`, `typeof null`
- [ ] **`null` vs `undefined`**, valores falsy, `??` vs `||`
- [ ] **Datas:** timestamp em ms (UTC), `Date` e fusos, formato ISO 8601, `Intl.DateTimeFormat`
- [ ] **Bytes:** `ArrayBuffer`, `TypedArray`, `TextEncoder/Decoder`, Base64, Blob/File

## 2. Memória (intermediário)
- [ ] **Stack e heap:** primitivos vs objetos; **tudo é passado por valor, mas objetos são referências**
- [ ] **Igualdade por referência** (`{} !== {}`): base do `Object.is` no React
- [ ] **Closures** capturam variáveis (e as mantêm vivas na memória)
- [ ] **Coleta de lixo:** alcançabilidade; marcação e varredura (noção de geracional)
- [ ] **Vazamentos típicos:** listeners não removidos, `setInterval` sem `clear`, closures grandes, **nós do DOM destacados**, caches sem limite
- [ ] **`WeakMap`, `WeakSet`, `WeakRef`** (noção)
- [ ] Imutabilidade e custo de cópia (`structuredClone`, spread raso vs profundo)

## 3. Motor JavaScript (V8): pipeline (intermediário a avançado)
- [ ] **Parse → AST → bytecode (interpretador Ignition) → JIT (TurboFan)**; versões recentes têm camadas intermediárias (confirme os nomes na doc do V8)
- [ ] **Parsing preguiçoso** (funções só são compiladas quando chamadas)
- [ ] **Deoptimização:** código otimizado assume tipos; mudar o formato do objeto desotimiza
- [ ] **Formas de objeto (hidden classes/shapes)** e *inline caches* (noção): por que inicializar propriedades sempre na mesma ordem ajuda
- [ ] Comparar com o pipeline do PHP (camada 2 do mapa de PHP) e com o do Vite (camada 1 deste mapa)
- [ ] Node usa o mesmo V8; navegadores diferentes usam motores diferentes (SpiderMonkey, JavaScriptCore)

## 4. Event loop em profundidade (essencial)
- [ ] **Call stack** → **Web APIs** → **fila de tarefas (macrotasks)** e **fila de microtasks** (Promises, `queueMicrotask`)
- [ ] Ordem: código síncrono → microtasks (todas) → renderização (se houver) → próxima macrotask
- [ ] **`requestAnimationFrame`** (antes do próximo paint) vs `setTimeout` vs `requestIdleCallback`
- [ ] **Tarefa longa bloqueia a UI** (a thread principal é uma só): clique, rolagem e pintura esperam
- [ ] **`await` não bloqueia a thread**, só suspende a função
- [ ] **Web Workers** (outra thread, comunicação por mensagens), `postMessage`
- [ ] Event loop do **Node** (fases: timers, poll, check) como contraste

## 5. Pipeline de renderização (intermediário)
- [ ] HTML → **DOM**; CSS → **CSSOM**; juntos → **árvore de renderização**
- [ ] **Style → Layout (reflow) → Paint → Composite**; GPU e camadas
- [ ] **O que causa reflow:** mudar tamanho/posição/conteúdo; ler `offsetHeight` após escrever estilo (*layout thrashing*)
- [ ] **`transform` e `opacity`** podem animar só no compositor (sem layout/paint)
- [ ] **Orçamento de quadro:** ~16,6 ms a 60 Hz
- [ ] Scripts bloqueantes, `defer`, `async`, `type="module"`, *preload scanner*
- [ ] Como o React se encaixa: render (JS) → commit (DOM) → o navegador faz layout/paint

## 6. Rede e bytes (intermediário)
- [ ] Uma requisição HTTP **é texto em um socket TCP**: linha inicial, headers, corpo
- [ ] DNS → TCP → TLS → HTTP; reuso de conexão; HTTP/2 multiplexa (por que muitos módulos ficaram viáveis)
- [ ] Cache (`Cache-Control`, `ETag`), prioridades de recursos, compressão (gzip/brotli)
- [ ] Streaming de resposta e `ReadableStream` (noção)

## 7. Observar a máquina (prática)
- [ ] **DevTools → Performance:** gráfico de chamas, tarefas longas, *recalculate style/layout/paint*
- [ ] **DevTools → Memory:** *heap snapshot*, comparação entre snapshots, procurar nós destacados
- [ ] **Rendering:** *paint flashing*, *layout shift regions*; aba **Layers**
- [ ] **Network:** *waterfall*, cache, tamanhos, headers
- [ ] `performance.mark/measure`, `console.time`, `PerformanceObserver`
- [ ] **Node:** `node --inspect`, `--cpu-prof`; flags do V8 para ver bytecode (ex.: `--print-bytecode`; confirme na doc da sua versão)

## Autoavaliação
1. Por que `0.1 + 0.2` não dá `0.3` e como guardar dinheiro?
2. Qual a ordem de execução de `setTimeout(0)`, `Promise.then` e código síncrono, e por quê?
3. Por que um loop de 2 segundos "congela" o botão e a rolagem?
4. O que é reflow e por que animar `left` é pior que animar `transform`?
5. Como um listener esquecido causa vazamento de memória?
6. Por que o JIT pode ficar mais lento quando o formato de um objeto muda?

## Exercícios
- Provocar uma tarefa longa e medi-la no Performance; depois quebrá-la em partes ou mover para um Worker
- Criar um vazamento (listener + closure), achá-lo com *heap snapshot* e corrigi-lo
- Animar o mesmo elemento com `left` e com `transform` e comparar no Performance
- Provocar *layout thrashing* (ler e escrever estilo em loop) e consertar em lote
- Escrever 10 expressões de coerção/igualdade, **prever** o resultado e conferir no console
- Desenhar de memória a linha do tempo de um clique: evento → handler → `setState` → render → commit → layout → paint
