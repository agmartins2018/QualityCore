Esta página separa capacidades existentes de possíveis evoluções. Itens futuros são sugestões de planejamento, não compromissos de entrega.

## Implementado até 0.19.2

- Projects, API Tests e Collections.
- Assertions visuais e relatórios Expected vs Actual.
- Monitors para API Test e Collection.
- Dashboard com latência por monitor.
- Contracts com cadastro JSON e importação OpenAPI 3 JSON.
- Execução de performance com k6, timeline e relatório.
- Autenticação JWT e gerenciamento básico de contas.

## Evoluções possíveis

| Área | Possível evolução |
|---|---|
| Segurança | Rate limiting, RBAC, gestão de segredos, hardening SSRF |
| Contratos | OpenAPI YAML, referências externas controladas, cobertura ampliada |
| Performance | Métrica explícita de checks, thresholds configuráveis, integração com fontes externas |
| Observabilidade | Métricas de host/containers/banco, retenção e exportação |
| Collections | Drag-and-drop, dependências entre etapas, variáveis e setup/teardown |
| Persistência | Banco de produção, migrações e estratégia de backup |
| Engenharia | Pipeline CI/CD, cobertura automatizada e quality gates |

## Governança do roadmap

Toda evolução deve ter requisito, critérios de aceite, riscos, impacto arquitetural e plano de testes. Atualize esta página somente após decisão do responsável pelo produto.
