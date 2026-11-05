```text
src/
├── main/
│   ├── java/com/qualitycore/
│   │   ├── api/
│   │   ├── model/
│   │   ├── repo/
│   │   ├── security/
│   │   ├── service/
│   │   └── QualityCoreApplication.java
│   └── resources/
│       ├── static/
│       └── application.properties
└── test/java/com/qualitycore/
    ├── api/
    ├── service/
    └── security/
```

## Pacotes

| Pacote | Responsabilidade |
|---|---|
| `api` | Endpoints e controllers REST |
| `model` | Entidades JPA |
| `repo` | Interfaces de persistência |
| `security` | Usuários, hashing, JWT e configuração de segurança |
| `service` | Regras de negócio e integrações |

## Serviços de domínio relevantes

- `ExecutionService` e `CollectionExecutionService`: execução individual e sequencial.
- `ResponseChecker` e `AssertionEvaluator`: avaliação de respostas.
- `MonitorScheduler`: execução agendada dos monitores.
- `ContractService`, `ContractVerifier`, `OpenApiImporter`: contratos e importação.
- `PerformanceService`, `K6ScriptBuilder`, `K6Installer`, `K6Summary`: execução de performance.
- `TargetMetricsSampler`: coleta de métricas do alvo/processo.
- `K6TimelineAggregator`: agregação da timeline.
