# Camada 6: Qualidade, Acessibilidade e Segurança

**Foco:** o que separa código que "funciona" de código profissional. Em WorldSkills isso rende pontos diretos (acessibilidade, validação, estrutura, segurança).

## 1. Acessibilidade (a11y)
- [ ] WCAG 2.x (princípios POUR: perceptível, operável, compreensível, robusto)
- [ ] HTML semântico primeiro; ARIA só quando necessário ("sem ARIA é melhor que ARIA ruim")
- [ ] Teclado: ordem de foco, `tabindex`, foco visível, armadilhas de foco em modais
- [ ] Gerenciamento de foco em SPA (mudança de rota, modais, `aria-live`)
- [ ] Labels de formulário, `alt`, contraste de cor, `prefers-reduced-motion`
- [ ] Leitores de tela (NVDA, VoiceOver), árvore de acessibilidade no DevTools
- [ ] Ferramentas: axe, Lighthouse, `eslint-plugin-jsx-a11y`

## 2. Segurança no front-end
- [ ] **XSS:** como ocorre; escape automático do React; `dangerouslySetInnerHTML`, `innerHTML`, `href="javascript:"`
- [ ] Sanitização (DOMPurify)
- [ ] **CSRF**, `SameSite`, tokens
- [ ] **CSP** (Content Security Policy), CORS (o que protege e o que não)
- [ ] Armazenamento de token: `localStorage` vs cookie `HttpOnly`
- [ ] **Nunca colocar segredos no front** (`VITE_*` é público)
- [ ] Supply chain: `npm audit`, lockfile, dependências suspeitas, versões fixas
- [ ] Validação no cliente é conveniência; **a segurança está no servidor**
- [ ] Clickjacking, `rel="noopener"`, HTTPS

## 3. Testes
- [ ] Pirâmide: unitário, integração, e2e
- [ ] **Vitest** (integrado ao Vite), Jest (conceitos), jsdom/happy-dom
- [ ] **React Testing Library:** testar comportamento, queries por role/label
- [ ] **Playwright** ou Cypress: e2e
- [ ] Mocks (`vi.fn`, MSW para APIs), snapshots (com moderação)
- [ ] Cobertura e o que ela não garante
- [ ] TDD básico

## 4. Análise estática e formatação
- [ ] ESLint (flat config), `eslint-plugin-react-hooks` (regras de hooks e deps), regras de a11y
- [ ] Prettier
- [ ] TypeScript estrito (`strict`), `tsc --noEmit`, tipos para props, eventos, hooks, generics
- [ ] Husky/lint-staged (noções)

## 5. Git e fluxo de trabalho
- [ ] `commit`, `branch`, `merge`, `rebase`, `stash`, `reset` vs `revert`, `cherry-pick`
- [ ] Resolver conflitos, `git diff`, `git log`, `git bisect`, `.gitignore`
- [ ] Conventional Commits, Pull Requests, code review
- [ ] CI básico (GitHub Actions: instalar, lint, test, build)

## 6. Tratamento de erros e observabilidade
- [ ] Erros esperados vs inesperados, estados de erro na UI
- [ ] Logging, source maps em produção, Sentry (conceito)
- [ ] Mensagens de erro úteis ao usuário

## Exercícios
- Auditar uma página com axe/Lighthouse e corrigir todas as falhas
- Injetar um XSS em uma página de propósito e corrigi-la
- Escrever testes com Vitest + Testing Library para um formulário
- Configurar ESLint + Prettier + hook de pre-commit
- Fazer um PR com review de si mesmo (checklist)
