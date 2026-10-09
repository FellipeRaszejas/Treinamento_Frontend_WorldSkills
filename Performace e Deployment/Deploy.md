# Camada 7: Performance e Deploy

**Foco:** medir antes de otimizar, e entender o caminho do `dist/` até o usuário.

## 1. Métricas
- [ ] **Core Web Vitals:** LCP, INP, CLS (e TTFB, FCP)
- [ ] Lighthouse, PageSpeed, DevTools Performance, WebPageTest
- [ ] Lab data vs field data
- [ ] Orçamentos de performance (budgets)

## 2. Carregamento
- [ ] Tamanho do bundle, `rollup-plugin-visualizer` / análise de bundle
- [ ] Code splitting por rota e por componente pesado (`React.lazy`)
- [ ] Preload, prefetch, `modulepreload`
- [ ] Imagens: formatos (WebP/AVIF), `srcset`, `sizes`, `loading="lazy"`, dimensões (evita CLS)
- [ ] Fontes: `font-display`, subset, preload
- [ ] Remover dependências pesadas (ex.: trocar libs grandes por menores)
- [ ] Minificação, compressão (gzip/brotli)

## 3. Runtime
- [ ] Long tasks, main thread, `requestIdleCallback`, Web Workers
- [ ] Virtualização de listas, debounce/throttle
- [ ] Reflow/repaint, animações só com `transform`/`opacity`
- [ ] Re-renders do React (Camada 3) e Profiler
- [ ] Vazamento de memória (Memory tab, heap snapshots)

## 4. Cache
- [ ] `Cache-Control`, `ETag`, `immutable`
- [ ] Por que arquivos com hash no nome podem ter cache de 1 ano
- [ ] `index.html` sem cache longo
- [ ] CDN e invalidação
- [ ] Service Worker e estratégias (cache-first, stale-while-revalidate)

## 5. Deploy
- [ ] O que é o `dist/`: arquivos estáticos
- [ ] Hospedagens estáticas (Netlify, Vercel, Cloudflare Pages, GitHub Pages)
- [ ] **`base`** do Vite quando o app roda em subpasta
- [ ] **SPA fallback:** servir `index.html` para rotas desconhecidas (Nginx `try_files`, Apache `.htaccess`, `_redirects`)
- [ ] Nginx/Apache: servir estáticos, headers, gzip, proxy reverso para API
- [ ] Docker: build multi-stage (Node para build, Nginx para servir)
- [ ] Variáveis de ambiente em build (são embutidas no bundle, não lidas em runtime)
- [ ] HTTPS, domínios, CI/CD

## Exercícios
- Medir uma página, aplicar 3 otimizações e comparar números antes e depois
- Gerar o relatório do visualizer e eliminar a maior dependência
- Servir o `dist/` com Nginx em Docker, com fallback de SPA funcionando ao recarregar `/rota`
- Quebrar de propósito o `base` e ler o erro 404 dos assets