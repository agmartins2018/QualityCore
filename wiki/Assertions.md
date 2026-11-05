# Assertions e validação de respostas

O QUALITYCORE disponibiliza um construtor visual para definir verificações de resposta. As configurações são persistidas no formato JSON e avaliadas pelo backend.

## Tipos de assertion disponíveis no construtor

| Tipo | O que valida |
|---|---|
| `status` | Código de status HTTP |
| `header` | Presença/valor de um header |
| `jsonPath` | Valor localizado por JSONPath, com operadores suportados |
| `contains` | Presença de texto no corpo |
| `schema` | Tipo da raiz, propriedades obrigatórias e tipos declarados |

A interface permite adicionar, editar e remover assertions, validar campos e visualizar a prévia do JSON que será salvo.

## Resultado estruturado

A camada `ResponseChecker` centraliza a avaliação e gera resultados estruturados com informações como tipo, descrição, operador, valor esperado, valor obtido, status e motivo.

O relatório apresenta **Expected vs Actual**. Uma assertion inválida ou com erro de avaliação reprova aquela verificação, sem impedir necessariamente a avaliação das demais.

## Tempo de resposta

O limite `maxResponseTimeMs` é aplicado a execuções individuais e a Collections. O limite é inclusivo: uma resposta exatamente no valor máximo configurado passa.

## Segurança de apresentação

Valores de resposta são exibidos com escape para evitar interpretação como HTML. Ainda assim, não utilize dados sensíveis em testes de demonstração.

## Referências técnicas

- `service/AssertionEvaluator.java`
- `service/ResponseChecker.java`
- `service/AssertionResult.java`
- `service/ExecutionChecks.java`
- `static/assertion-builder.js`
- `static/execution-checks.js`
