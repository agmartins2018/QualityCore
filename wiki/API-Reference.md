> Esta é uma visão dos grupos de rotas identificados nos controllers da versão 0.19.2. Para payloads, validações, códigos de resposta e contratos completos, consulte as assinaturas dos métodos e DTOs/entidades no código. A lista não substitui uma especificação OpenAPI publicada.

## Rotas

| Grupo | Método | Caminho | Finalidade |
|---|---|---|---|
| Health | GET | `/api/health` | Verificação de saúde |
| Auth | GET | `/api/auth/status` | Estado do setup |
| Auth | POST | `/api/auth/setup` | Criar conta inicial |
| Auth | POST | `/api/auth/login` | Autenticar |
| Auth | GET | `/api/auth/me` | Consultar usuário autenticado |
| Auth | POST | `/api/auth/users` | Criar usuário |
| Projects | GET/POST | `/api/projects` | Listar/criar projetos |
| Projects | DELETE | `/api/projects/{id}` | Excluir projeto |
| API Tests | GET/POST | `/api/tests` | Listar/criar testes |
| API Tests | PUT | `/api/tests/{id}` | Atualizar teste |
| API Tests | PATCH | `/api/tests/{id}/enabled` | Alterar habilitação |
| API Tests | DELETE | `/api/tests/{id}` | Excluir teste |
| Executions | POST | `/api/executions/{testId}` | Executar API Test |
| Executions | POST | `/api/executions/{executionId}/cancel` | Cancelar execução |
| Executions | GET | `/api/executions` | Consultar histórico |
| Collections | GET/POST | `/api/collections` | Listar/criar |
| Collections | PUT | `/api/collections/{id}` | Atualizar |
| Collections | POST | `/api/collections/{id}/run` | Executar por ID |
| Collections | POST | `/api/collections/run` | Executar por nome/configuração |
| Collections | DELETE | `/api/collections/{id}` | Excluir |
| Monitors | GET/POST | `/api/monitors` | Listar/criar |
| Monitors | PATCH | `/api/monitors/{id}/enabled` | Habilitar/desabilitar |
| Monitors | DELETE | `/api/monitors/{id}` | Excluir |
| Dashboard | GET | `/api/dashboard/summary` | Resumo |
| Dashboard | GET | `/api/dashboard/latency?hours=24` | Séries de latência |
| Contracts | GET | `/api/contracts` | Listar contratos |
| Contracts | GET | `/api/contracts/{id}` | Consultar contrato |
| Contracts | POST | `/api/contracts` | Criar contrato |
| Contracts | POST | `/api/contracts/import-openapi` | Importar OpenAPI JSON |
| Contracts | PUT | `/api/contracts/{id}` | Editar contrato |
| Contracts | POST | `/api/contracts/{id}/run` | Verificar contrato |
| Contracts | GET | `/api/contracts/{id}/runs` | Histórico |
| Contracts | GET | `/api/contracts/{id}/coverage` | Cobertura |
| Contracts | DELETE | `/api/contracts/{id}` | Excluir |
| Performance | GET | `/api/performance/limits` | Limites de execução |
| Performance | GET | `/api/performance/runs` | Listar execuções |
| Performance | GET | `/api/performance/runs/{id}` | Detalhe |
| Performance | GET | `/api/performance/runs/{id}/timeline` | Timeline |
| Performance | POST | `/api/performance/runs` | Criar execução |
| Performance | POST | `/api/performance/runs/{id}/stop` | Interromper execução |
| Performance | GET | `/api/performance/runs/{id}/report` | Relatório HTML |

## Autenticação

As rotas `/api/**` são protegidas por JWT, com exceções para health check e operações necessárias ao status/configuração inicial e login. Envie o token conforme o mecanismo de autenticação implementado pelo frontend/backend.

## Códigos de resposta

Os códigos específicos dependem da validação e das regras de cada controller. Não assuma que todos os endpoints retornam apenas 200; criação, validação, conflito e ausência de autenticação podem produzir respostas distintas.
