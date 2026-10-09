# Camada 1: Teoria de Compilação (o seu "lexer/parser")

**Foco:** entender o pipeline que transforma o código que você escreve (JSX, TS) no JS que o navegador roda. É o equivalente ao lexer → parser → opcodes do PHP.

```
código-fonte → LEXER (tokens) → PARSER (AST) → TRANSFORMAÇÃO (visitors) → GERAÇÃO (código + source map)
```

## Tópicos
- [ ] **Lexer/tokenizer:** como o texto vira tokens (`import`, `{`, `Button`, `<`, string...)
- [ ] **Parser:** gramática, precedência, parsing recursivo descendente
- [ ] **AST:** padrão ESTree; nós `Program`, `ImportDeclaration`, `VariableDeclaration`, `CallExpression`, `ArrowFunctionExpression`, `JSXElement`, `JSXAttribute`, `JSXExpressionContainer`
- [ ] **Transformações** com o padrão visitor (como plugins do Babel funcionam)
- [ ] **Geração de código** a partir da AST e **source maps** (formato, `sourcesContent`, VLQ)
- [ ] **Transpilar vs empacotar (bundle) vs minificar:** etapas distintas
- [ ] **Tree-shaking:** por que depende de ESM, side effects, `"sideEffects": false`
- [ ] **Dead code elimination, constant folding, scope hoisting**
- [ ] **Code splitting** e análise do grafo de dependências

## JSX
- [ ] JSX é açúcar sintático; **não existe** no navegador
- [ ] `<A x="1">oi</A>` vira `jsx(A, { x: "1", children: "oi" })`
- [ ] Runtime **clássico** (`React.createElement`, exige `import React`) vs **automático** (`react/jsx-runtime`, `jsxImportSource`)
- [ ] Por que componente começa com maiúscula (`<div>` vira string, `<Div>` vira referência)
- [ ] Fragmentos, spread de props, `key` e `ref`
- [ ] Expressões `{}` aceitam expressões, não statements
- [ ] Por que `class` virou `className`

## TypeScript
- [ ] O compilador do Vite/esbuild **apenas remove os tipos**, sem checar
- [ ] A checagem é feita por `tsc --noEmit` (separado, geralmente em `npm run build` ou CI)
- [ ] `isolatedModules`, `verbatimModuleSyntax`, `import type`
- [ ] Por que `enum` e `namespace` exigem cuidado com transpilers por arquivo
- [ ] `tsconfig.json` (e `tsconfig.app.json`/`tsconfig.node.json` no template Vite)
- [ ] Diferença entre erro de tipo (tsc) e erro de runtime

## Ferramentas (confirme na documentação o que sua versão do Vite usa)
- **Babel** (JS): flexível, lento, ecossistema enorme de plugins
- **SWC** (Rust) e **esbuild** (Go): muito rápidos
- **Oxc / Rolldown** (Rust): direção recente do ecossistema Vite
- **Rollup:** bundler com foco em ESM e tree-shaking
- **Lightning CSS / PostCSS:** processamento de CSS

## Autoavaliação
1. Qual a diferença entre token e nó da AST?
2. Por que o JSX precisa de uma etapa de compilação, mas o JS não?
3. Por que `import type` ajuda o transpilador?
4. Por que tree-shaking não funciona bem com `require`?
5. O que acontece se você escrever `if` dentro de `{}` no JSX?

## Exercícios
- Colar JSX no astexplorer.net e navegar pela AST (parser Babel e acorn)
- Escrever um tokenizer para uma mini-linguagem (ex.: expressões aritméticas), depois um parser recursivo descendente que gere AST
- Usar `@babel/core` em um script Node para transformar JSX e imprimir o resultado
- Escrever um plugin Babel que troque `console.log` por nada
- Gerar um source map e abrir no DevTools
- Livro de apoio: *Crafting Interpreters* (gratuito online)
