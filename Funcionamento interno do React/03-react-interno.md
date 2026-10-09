# Camada 3: Funcionamento Interno do React

**Foco:** entender como o React decide o que renderizar, quando e por quê. Isso explica 90% dos bugs de estado, efeitos e re-renders.

## 1. Modelo mental
- [ ] UI = função do estado: `UI = f(state)`
- [ ] Componente = função que retorna **elementos** (objetos simples `{ type, props, key, ref }`)
- [ ] Elemento ≠ componente ≠ instância ≠ nó do DOM
- [ ] Declarativo vs imperativo
- [ ] Pureza: render sem efeitos colaterais, mesmos inputs → mesmo output

## 2. Render, Commit e Effects
- [ ] **Trigger → Render → Commit** (ver react.dev "Render and Commit")
- [ ] Render calcula, Commit toca o DOM, depois rodam efeitos
- [ ] Quando um componente re-renderiza (estado, pai re-renderiza, contexto)
- [ ] Re-render ≠ atualização do DOM

## 3. Reconciliação
- [ ] Virtual DOM e diffing (heurísticas O(n))
- [ ] Mesmo tipo na mesma posição = atualiza; tipo diferente = remonta
- [ ] **`key`**: identidade, por que não usar índice em listas mutáveis, `key` para resetar estado
- [ ] Componentes definidos dentro de componentes (anti-padrão: remontam todo render)

## 4. Fiber
- [ ] Fiber node por componente; árvore `current` e `workInProgress` (double buffering)
- [ ] Work loop, unidades de trabalho interrompíveis
- [ ] Lanes (prioridades), time slicing, renderização concorrente
- [ ] Scheduler
- [ ] Suspense, `startTransition`, `useDeferredValue`
- [ ] Error boundaries

## 5. Hooks por dentro
- [ ] Hooks são uma **lista encadeada no fiber**, por isso a regra da ordem
- [ ] Dispatcher (hooks diferentes em mount e update)
- [ ] `useState`: fila de updates, **snapshots**, batching, updater function (`setX(x => x + 1)`)
- [ ] **Stale closures:** por que o valor "antigo" aparece
- [ ] `useEffect` (após o paint), `useLayoutEffect` (antes do paint), cleanup, array de dependências
- [ ] `useRef` (valor mutável que não causa render), refs de DOM
- [ ] `useMemo`/`useCallback`: cache por dependências, não é garantia semântica
- [ ] `useReducer`, `useContext` (todos os consumidores re-renderizam), `useId`, `useSyncExternalStore`
- [ ] Custom hooks: reutilizar lógica, não estado
- [ ] **Você talvez não precise de um effect** (artigo em react.dev)
- [ ] StrictMode: render e effects duas vezes em dev, para expor impurezas

## 6. react-dom
- [ ] `createRoot`, `root.render`, `unmount`
- [ ] Delegação de eventos e SyntheticEvent
- [ ] Portals, `flushSync`
- [ ] Hidratação (`hydrateRoot`) e hydration mismatch

## 7. Performance
- [ ] O que realmente causa re-render e quando importa
- [ ] `memo`, igualdade de props, estabilidade de referências
- [ ] Colocar estado perto de onde é usado; elevar conteúdo (children como prop)
- [ ] Virtualização de listas, `lazy` + Suspense
- [ ] React Profiler, "Why did this render"
- [ ] React Compiler (memoização automática; confirme status na doc)

## 8. React moderno
- [ ] React 19: Actions, `use`, `useActionState`, `useOptimistic`, `useFormStatus`, `ref` como prop
- [ ] Server Components e Server Actions (conceito; exigem framework/bundler compatível)
- [ ] Padrões: composition, compound components, controlled vs uncontrolled, render props, lifting state up

## Autoavaliação
1. O que é um elemento React? Imprima um com `console.log(<div/>)`.
2. Por que `setCount(count + 1)` três vezes soma só 1?
3. Por que não se pode chamar hook dentro de `if`?
4. Quando um effect roda e o que o cleanup limpa?
5. Por que a `key` resetou meu estado?
6. Por que o effect rodou duas vezes?

## Exercícios
- **Mini-React:** `createElement`, `render` recursivo, depois `useState` com array de hooks (Build your own React, Rodrigo Pombo)
- Reproduzir stale closure com `setInterval` e consertar de 3 formas
- Usar Profiler para achar re-renders desnecessários e medir antes/depois
- Quebrar a regra dos hooks de propósito e ler a mensagem de erro
- Ler o código de `useState` no repositório do React (apenas o fluxo, sem decorar)
