Esta página registra limites explicitamente identificados na versão 0.19.2 e no histórico de desenvolvimento.

## Produto e operação

- MVP local, não endurecido para produção.
- Não há perfis ou permissões granulares de usuário.
- Não há proteção contra força bruta no login.
- HTTPS deve ser providenciado ao sair do localhost.
- Console H2 está habilitado na configuração padrão.
- Banco H2 local e `ddl-auto=update` não substituem estratégia de produção.

## Collections e Monitors

- Reordenação de etapas por botões up/down, sem drag-and-drop.
- Não existe grafo explícito de dependências nem política de pular dependentes.
- Monitor de Collection utiliza timeout fixo de 10 segundos por etapa.
- Histórico `monitor_checks` não possui limpeza automática.
- Detalhe histórico por API em Collection existe a partir de 0.18.0.

## Contracts

- Importação aceita OpenAPI 3 em JSON; YAML não é aceito.
- Referências externas são recusadas.
- Não existe matriz de dependências consumidor/provedor.
- Não existe cobertura baseada em tráfego real de produção.
- O validador implementa um conjunto de keywords/formats, não se deve assumir suporte integral a JSON Schema.

## Performance e infraestrutura

- Limites padrão de 50 VUs e 600 segundos.
- Métricas do processo/JVM e HikariCP não equivalem a monitoramento completo de infraestrutura.
- Métricas de CPU/memória de host, banco remoto, rede e containers dependem de instrumentação externa.
- A taxa de checks do k6 deve ser distinguida da taxa de erros HTTP; só deve ser exibida se a métrica de checks for coletada e agregada.

## Verificação antes de cada release

Revise esta página contra o código e remova itens somente após implementação e validação reproduzível.
