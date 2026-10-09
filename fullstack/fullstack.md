# Camada 8: Back-end, Dados e Integração

**Foco:** WorldSkills Web Technologies cobra aplicações completas. O front precisa conversar bem com um back-end (PHP, Node ou outro) e com um banco.

## 1. APIs
- [ ] REST: recursos, métodos, status codes corretos, idempotência
- [ ] JSON, paginação, filtros, ordenação, versionamento
- [ ] Consumo: `fetch`, tratamento de erro (`response.ok`), `AbortController`, timeouts
- [ ] Estados de UI: loading, erro, vazio, sucesso
- [ ] Proxy do Vite (`server.proxy`) vs CORS no servidor
- [ ] GraphQL e WebSocket/SSE (noções)
- [ ] Documentação de API (OpenAPI/Swagger)

## 2. Autenticação e autorização
- [ ] Sessão (cookie) vs token (JWT), refresh token
- [ ] Fluxo de login, logout, rotas protegidas no front e **verificação no servidor**
- [ ] Hash de senha (bcrypt/argon2), nunca texto puro
- [ ] Papéis e permissões (RBAC)
- [ ] OAuth (noção)

## 3. Back-end
- [ ] PHP moderno (PDO, prepared statements, namespaces, Composer, PSR) e/ou Node (Express/Fastify)
- [ ] Roteamento, middlewares, validação e tratamento de erros no servidor
- [ ] Upload de arquivos, sessões, variáveis de ambiente
- [ ] Padrão MVC e separação de camadas

## 4. Banco de dados
- [ ] SQL sólido: `SELECT`, `JOIN`, `GROUP BY`, subqueries, índices, transações
- [ ] Modelagem relacional, normalização, chaves estrangeiras, integridade
- [ ] **SQL Injection** e prepared statements
- [ ] ORMs/query builders (noção)
- [ ] NoSQL (quando faz sentido)

## 5. Integração em desenvolvimento
- [ ] Rodar front (Vite) e back em portas diferentes; proxy
- [ ] Docker Compose (front + API + banco)
- [ ] `.env` separados por ambiente, seeds e migrations
- [ ] Testar API com curl/Postman/Insomnia/REST Client

## Exercícios
- CRUD completo: React (Vite) + API (PHP ou Node) + MySQL/PostgreSQL, com login
- Quebrar CORS de propósito e resolver de duas formas (proxy e headers)
- Plantar um SQL injection e corrigir com prepared statements
- Subir tudo com `docker compose up`000,