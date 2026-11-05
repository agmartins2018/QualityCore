Os testes estão organizados por camada e responsabilidade em `src/test/java/com/qualitycore/`.

## Áreas cobertas

### API / Controllers
- Associação e pertencimento de Collections.
- Health check.
- Controller de Monitors.

### Services
- Avaliação de respostas e assertions.
- Geração/instalação de script k6.
- Amostragem de métricas do alvo.
- Agregação de timeline.
- Edição e exclusão de Contracts.
- Geração de script k6.
- Relatório de performance.
- Importação e verificação de contratos.
- Scheduler de monitores.
- Estatísticas de latência.
- Validações de performance.

### Security
- Autenticação e regras de segurança.
- Wiring de beans.

## Executar

```bash
mvn clean test
```

## Evidência da versão

O README incluído no pacote 0.19.2 registra 122 testes Java passando na validação daquela entrega. Reexecute a suíte no ambiente de destino antes de declarar a versão aprovada.

## Validações manuais recomendadas

- Primeiro setup, login, criação de usuário e logout.
- Chamada protegida sem token deve ser rejeitada.
- Criar, editar, executar e excluir API Test.
- Validar Assertions de todos os tipos disponíveis.
- Criar Collection com STOP e CONTINUE.
- Criar Monitors para API Test e Collection.
- Importar OpenAPI JSON e executar Contract.
- Executar k6 com cenário saudável e com resposta de erro.
- Conferir timeline, métricas e relatório HTML.
