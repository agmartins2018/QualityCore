## v0.19.2 — Correções de Contracts e relatório de performance

- Corrigida exclusão de Contract, removendo histórico pelas entidades carregadas.
- Adicionada edição de Contract (nome e JSON) por `PUT /api/contracts/{id}`.
- Preservação do histórico ao editar contrato.
- Validação de `additionalProperties: false`.
- Relatório HTML de performance solicitado com token e aberto como conteúdo local.
- README de origem registra 122 testes Java passando.
- Não foi executada validação contra JSONPlaceholder no ambiente de desenvolvimento.

## v0.19.1 — Correção de inicialização

- Corrigido wiring do `JwtService` no contexto Spring, com construtor de produção explicitamente selecionado.
- Adicionado teste de regressão para bean com múltiplos construtores.

## v0.19.0 — Autenticação JWT e Contracts

- Setup inicial de administrador, login e gerenciamento básico de usuários.
- PBKDF2-HMAC-SHA256 com 210 mil iterações e salt aleatório.
- JWT HS256 com validade configurável.
- Módulo Contracts com cadastro JSON e importação OpenAPI 3 JSON.
- Validação de status, schemas, campos e parâmetros obrigatórios.
- Histórico, cobertura e gráficos de verificações.
- Detalhes por API no Dashboard e monitores de Collection.

## v0.18.0 — Detalhe por API e edição de Collection

- Detalhamento por API em verificações de monitores de Collection.
- Edição do nome da Collection.
- Bloqueio de nome duplicado no mesmo projeto.

## v0.17.1 / v0.17.0

- Evolução de Collections, performance e interface.
- Métricas de servidor/processo e melhorias nos relatórios e gráficos.
- Renomeação e consolidação da identidade QUALITYCORE.

## v0.16.0 / v0.15.0

- Acompanhamento ao vivo e métricas de performance.
- Integração com k6, tooltips e evolução dos cenários.

## v0.14.0

- Uma API Test pode pertencer a várias Collections.
- Cada Collection mantém sua própria sequência.
- Monitores passam a ter um único alvo: API Test ou Collection.

## v0.13.1 / v0.13.0

- Ajuste responsivo do gráfico de latência.
- Monitores para Collections e histórico de latência por monitor.

## v0.12.0

- Assertion Engine centralizado.
- Relatórios Expected vs Actual.
- Persistência estruturada das verificações e limite de tempo de resposta.

## v0.11.2 / v0.11.1

- Construtor visual de Assertions.
- Query Parameters, Headers, Request Preview e suporte a HEAD/OPTIONS.

> O histórico acima resume as informações presentes no README incluído no pacote 0.19.2. Consulte o histórico Git para detalhes completos de cada alteração.
