# Camada 5: Ecossistema e Arquitetura de Aplicação

**Foco:** decisões de arquitetura que cercam o React e o Vite: como renderizar, onde guardar estado, como organizar o projeto e estilizar.

## 1. Estratégias de renderização
- [ ] **CSR** (SPA): o que o Vite entrega por padrão; custo em SEO e primeira pintura
- [ ] **SSR**, **SSG**, **ISR**, streaming, hidratação
- [ ] Quando cada uma faz sentido
- [ ] Frameworks sobre React: Next.js, React Router (modo framework), TanStack Start (e como Vite se encaixa em alguns)
- [ ] API de SSR do Vite (conceito)

## 2. Roteamento
- [ ] History API (`pushState`, `popstate`) e como o router funciona
- [ ] React Router: rotas aninhadas, `Outlet`, params, loaders, `Navigate`, rotas protegidas
- [ ] Hash vs browser router; **fallback de SPA** no servidor
- [ ] Code splitting por rota

## 3. Estado
- [ ] Local vs compartilhado vs de servidor vs de URL vs de formulário
- [ ] Context API (limites), `useReducer`
- [ ] Zustand / Redux Toolkit / Jotai (diferenças conceituais)
- [ ] **Server state:** TanStack Query (cache, stale, invalidação, mutations)
- [ ] URL como estado (search params)

## 4. Formulários e validação
- [ ] Controlled vs uncontrolled, `FormData`
- [ ] React Hook Form, Zod/Yup (validação por schema)
- [ ] Mensagens de erro acessíveis, estados de loading/erro

## 5. Estilização
- [ ] CSS puro, CSS Modules, Tailwind, CSS-in-JS (custo em runtime)
- [ ] Design tokens com custom properties, temas claro/escuro (`prefers-color-scheme`)
- [ ] Bibliotecas de componentes (Radix, shadcn/ui, MUI) e acessibilidade embutida
- [ ] Animações: CSS, Web Animations API, Framer Motion/Motion

## 6. Padrões de projeto em React
- [ ] Composition vs herança, `children`, slots
- [ ] Compound components, render props, custom hooks, HOCs (legado)
- [ ] Container/presentational, feature-based folders
- [ ] Separação de UI, lógica e acesso a dados
- [ ] Controlled/uncontrolled components
- [ ] Error boundaries + Suspense para loading/erro

## 7. Estrutura de projeto
- [ ] Organização por feature vs por tipo
- [ ] Aliases (`@/`), barrel files (e o custo em tree-shaking)
- [ ] Convenções de nomes, imports, tipos
- [ ] Monorepos e workspaces (noções)

## 8. Web moderna adicional
- [ ] PWA: manifest, Service Worker, cache offline
- [ ] i18n (`Intl`, bibliotecas de tradução), formatação de datas/números
- [ ] Web Workers, WebSocket / SSE para tempo real
- [ ] Upload de arquivos, drag and drop, canvas/SVG

## Exercícios
- Construir o mesmo app em CSR e depois migrar uma tela para SSR/SSG com um framework, comparando HTML recebido
- Implementar um mini-router com History API
- Trocar `useState + useEffect + fetch` por TanStack Query e comparar
- Montar um design system pequeno com tokens e 5 componentes acessíveis
