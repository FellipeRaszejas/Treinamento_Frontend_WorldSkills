# Camada 11: Formulários, CRUD e Regras de Negócio no Front-end

**Foco:** a parte prática que mais aparece em competição e em sistemas reais: formulários, listagens, criação/edição/exclusão, erros vindos da API e estados de interface. Complementa as camadas 16 (PHP) e 18 (Laravel) do lado do servidor. Nível: básico a avançado.

> **Regra de ouro:** no front-end, validação e regras são **conveniência e experiência de uso**. **Quem decide é o servidor** e quem garante é o banco. A UI nunca é segurança.

## 1. O papel do front nas regras
- [ ] Validar no cliente para **feedback rápido**; o servidor valida de novo, sempre
- [ ] **Não duplicar regras complexas** à mão: preferir mensagens/erros vindos da API (422) e, se necessário, um schema compartilhado (ex.: Zod)
- [ ] **Permissões:** esconder/desabilitar botões com base no que a API informa; mas o servidor continua autorizando
- [ ] **Valores calculados na tela** (totais, descontos) são informativos; o servidor recalcula
- [ ] Mesmos formatos de dado nos dois lados (datas ISO, enums, dinheiro em centavos)

## 2. Formulários (do nativo ao avançado)
- [ ] **HTML nativo:** `required`, `type`, `min/max`, `pattern`, `maxlength`, `autocomplete`, `inputmode`
- [ ] **Constraint Validation API:** `checkValidity`, `reportValidity`, `setCustomValidity`, evento `invalid`
- [ ] `FormData`, `new FormData(form)`, `Object.fromEntries`
- [ ] **Controlado vs não controlado** no React; quando cada um é melhor
- [ ] Estados do campo: *pristine/dirty*, *touched*, *valid/invalid*; quando mostrar o erro (blur vs submit)
- [ ] **Normalizar entrada:** `trim`, máscaras (CPF, telefone) sem quebrar colagem, números e dinheiro (string → centavos), datas/fusos
- [ ] **Acessibilidade:** `<label for>`, `aria-invalid`, `aria-describedby`, mensagens com `aria-live`, **foco no primeiro erro**
- [ ] **Evitar duplo envio:** desabilitar durante o envio, ignorar cliques repetidos
- [ ] **Aviso de saída** com alterações não salvas (`beforeunload`, bloqueio de navegação)
- [ ] Uploads: validar tipo/tamanho, *preview*, progresso, `FormData` com arquivos
- [ ] Bibliotecas: React Hook Form + Zod (entender o que fazem por baixo)

## 3. Estados de interface
- [ ] **Evitar booleanos que se contradizem** (`isLoading` e `isError` juntos): modelar como estado único `idle | loading | success | error`
- [ ] **Máquina de estados** com `useReducer` (e XState como noção): transições permitidas
- [ ] Estados de tela: *loading/skeleton*, **vazio**, **erro**, sucesso, **parcial**
- [ ] Mensagens de erro úteis, com ação de tentar novamente
- [ ] Suspense e Error Boundaries (conceito)

## 4. CRUD no front (checklist por operação)
**Listagem**
- [ ] Paginação, filtros, ordenação e **busca com debounce**
- [ ] **Estado na URL** (`searchParams`) para compartilhar/recarregar
- [ ] Cancelar requisições antigas (`AbortController`) e **ignorar respostas fora de ordem**
- [ ] Preservar rolagem/seleção ao voltar

**Detalhe**
- [ ] Carregar por id, 404 tratado, permissões

**Criar / Editar**
- [ ] Formulário preenchido a partir dos dados, **dirty check**, enviar só o que mudou (`PATCH`) ou tudo (`PUT`)
- [ ] Mapear **erros 422 por campo** para os inputs
- [ ] Conflito **409** (alguém alterou antes): avisar e recarregar

**Excluir**
- [ ] Confirmação, estado de carregamento, desfazer (soft delete), atualizar a lista

**Em lote e extras**
- [ ] Seleção múltipla, ações em lote, importação/exportação CSV, ordenação por colunas

## 5. Tratamento de erros da API
| Código | O que o front faz |
|---|---|
| 400 | Mostrar mensagem genérica de requisição inválida |
| 401 | Levar ao login, limpar sessão |
| 403 | Mostrar "sem permissão", não tentar de novo |
| 404 | Tela de não encontrado |
| 409 | Conflito: explicar e oferecer recarregar |
| 422 | Mapear erros por campo |
| 429 | Aguardar/`backoff`, avisar o usuário |
| 5xx / rede | Mensagem de falha e botão de tentar novamente; offline |
- [ ] Função única de requisição (wrapper de `fetch`) que trata `response.ok`, JSON, timeout e erros padronizados

## 6. Atualização otimista e cache
- [ ] **Atualização otimista:** mostrar o resultado antes da resposta e **reverter** se falhar
- [ ] **TanStack Query:** cache, `staleTime`, invalidação após mutação, `useMutation`
- [ ] Sincronizar lista e detalhe após editar
- [ ] Idempotência: repetir uma ação não pode duplicar o efeito (chave de idempotência, desabilitar botão)

## 7. Condições de corrida no front
- [ ] Respostas chegando fora de ordem (busca digitada rápido)
- [ ] `useEffect` com *cleanup* e flag de cancelamento, ou `AbortController`
- [ ] Duplo envio e cliques repetidos
- [ ] Estado obsoleto (*stale*) em closures

## 8. Testes
- [ ] Testing Library: preencher formulário, enviar, ver erros
- [ ] MSW para simular 422, 409, 401, 500 e rede lenta
- [ ] Testar estados: vazio, erro, carregando, sucesso

## Autoavaliação
1. Por que validar no front **e** no servidor, e qual dos dois decide?
2. Por que `isLoading` e `isError` como booleanos separados geram bugs?
3. Como mapear um 422 do Laravel para os campos de um formulário React?
4. Como evitar que a resposta de uma busca antiga sobrescreva a nova?
5. O que acontece se o usuário clicar duas vezes em "Salvar"?
6. Como funciona uma atualização otimista e quando ela deve reverter?

## Exercícios
- Formulário completo (validação nativa + React) com erros acessíveis, foco no primeiro erro e prevenção de duplo envio
- Listagem com filtros, paginação e busca com debounce, **estado na URL** e cancelamento de requisições
- CRUD de pedidos consumindo uma API (Laravel/PHP) com tratamento de 401/403/404/409/422/5xx
- Atualização otimista com *rollback* ao falhar
- Reproduzir a corrida de buscas (respostas fora de ordem) e corrigir
- Máquina de estados de um pedido na UI (`rascunho → pago → enviado`) respeitando as permissões vindas da API
