# Camada 12: Complementos (o que costuma ficar de fora)

**Foco:** habilidades transversais que aumentam velocidade, precisão e independência de framework.

## 1. JavaScript e DOM sem framework
- [ ] Montar uma aplicação **sem React**: módulos ES nativos, DOM, `fetch`, History API
- [ ] Criar elementos (`createElement`), delegação de eventos, templates (`<template>`), `dataset`
- [ ] **Web Components** (noção): custom elements, shadow DOM
- [ ] Saber fazer sem biblioteca: modal, abas, acordeão, carrossel, validação, máscara, debounce
- [ ] Entender o que o React "automatiza" por já ter feito à mão

## 2. Design e leitura de especificação
- [ ] **Ler um protótipo (Figma):** medidas, espaçamento, tipografia, cores, grid, estados (hover, focus, disabled, erro)
- [ ] Hierarquia visual, alinhamento, ritmo de espaçamento, escala tipográfica
- [ ] **Contraste de cor** (WCAG), tamanhos de alvo de toque
- [ ] **Responsividade real:** mobile-first, pontos de quebra pelo conteúdo, imagens responsivas
- [ ] Implementação **fiel** (design implementation): comparar sobreposto ao protótipo

## 3. Engenharia de software
- [ ] Clean code: nomes, funções pequenas, um nível de abstração, remover duplicação
- [ ] Componentes com responsabilidade única; pastas por feature
- [ ] Refatoração segura (com testes), *code smells*
- [ ] Documentação: README, instruções de execução, comentários que explicam o porquê
- [ ] Git na linha de comando, mensagens de commit claras, resolver conflitos

## 4. Lógica e estruturas de dados
- [ ] Arrays, objetos, `Map`, `Set`, filas, pilhas, árvores (noção), recursão
- [ ] Ordenação e busca, **Big-O** prático (loop dentro de loop, `includes` em array vs `Set`)
- [ ] Resolver problemas de strings, datas, agrupamentos, totais
- [ ] `reduce`, `groupBy` manual, normalização de dados (`byId`)
- [ ] Transformar listas em árvores (menus, categorias)

## 5. SEO e metadados
- [ ] `<title>`, `meta description`, Open Graph, `canonical`, `lang`
- [ ] HTML semântico e títulos (`h1`–`h6`), `alt`, links com texto significativo
- [ ] Por que SPA puro sofre com SEO e o que SSR/SSG resolve (camada 5)
- [ ] `robots.txt`, `sitemap.xml` (noção)

## 6. Gráficos e movimento
- [ ] **SVG** (inline, `viewBox`, `path`), **Canvas 2D**
- [ ] Animações: transições e `@keyframes` no CSS, Web Animations API, `requestAnimationFrame`
- [ ] `prefers-reduced-motion`
- [ ] Gráficos de dados (noção de bibliotecas e de como funcionam)

## 7. Editor, terminal e hábitos
- [ ] VS Code: atalhos, multi-cursor, busca global, renomear símbolo, depurador
- [ ] DevTools: atalhos, *breakpoints*, *watch*, *conditional breakpoints*
- [ ] Terminal: histórico, `grep`, `find`, pipes; npm/pnpm scripts
- [ ] **Inglês técnico:** documentação, mensagens de erro e enunciados
- [ ] Caderno de bugs, resumos e revisão espaçada

## Autoavaliação
1. Consigo construir uma tela interativa só com JS puro?
2. Consigo implementar uma tela a partir de um protótipo sem "chutar" medidas?
3. Sei explicar o custo de `array.includes` dentro de um `map`?

## Exercícios
- Reescrever um componente React (lista com filtro) em JS puro e comparar
- Implementar 3 telas de um protótipo, sobrepondo ao design para conferir
- Resolver 30 problemas pequenos de lógica, cronometrados
- Escrever o `<head>` completo (SEO e social) para uma página
- Criar uma animação com SVG e outra com Canvas
