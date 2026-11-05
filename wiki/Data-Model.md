A persistência utiliza entidades JPA e Spring Data JPA com banco H2 em arquivo.

## Entidades identificadas

| Entidade | Responsabilidade |
|---|---|
| `Project` | Agrupamento lógico |
| `ApiTest` | Configuração de uma chamada/teste de API |
| `ApiCollection` | Sequência de API Tests e política de falha |
| `Execution` | Histórico de execução de API Test |
| `Monitor` | Configuração de verificação recorrente |
| `MonitorCheck` | Resultado histórico de verificação do monitor |
| `ApiContract` | Definição do contrato de API |
| `ContractRun` | Histórico de verificação de contrato |
| `PerformanceRun` | Execução e resultados de performance |
| `UserAccount` | Conta de acesso |

## Relações funcionais

- Project agrupa API Tests e Collections.
- Uma Collection mantém sua própria lista ordenada de testes; um API Test pode ser reutilizado em mais de uma Collection.
- Monitor aponta para um API Test ou uma Collection, nunca ambos simultaneamente.
- MonitorCheck registra o resultado de uma verificação.
- ApiContract possui execuções ContractRun.
- PerformanceRun representa uma execução k6.
- UserAccount sustenta a autenticação local.

## Persistência e evolução

A URL H2 padrão aponta para `./data/qasentinel`. O projeto usa `spring.jpa.hibernate.ddl-auto=update`, portanto o Hibernate atualiza o schema na inicialização.

Para ambientes com dados importantes, documente migrações, backup, restore e compatibilidade antes de atualizar a aplicação.
