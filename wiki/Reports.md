# Reports

O QUALITYCORE disponibiliza relatórios e detalhes em diferentes áreas.

## API Test e Collection

Os relatórios apresentam resultado geral e detalhes por etapa, incluindo status HTTP, tempo de resposta, assertions e motivos de falha. O relatório de Collection permite analisar a sequência executada.

## Contracts

O histórico de verificações apresenta resultado por operação, falhas identificadas e cobertura da última execução.

## Performance

O módulo Performance oferece:
- Resumo final.
- Métricas agregadas.
- Timeline da execução.
- Relatório HTML por execução.

A rota do relatório é `GET /api/performance/runs/{id}/report`. O frontend solicita o conteúdo com a sessão autenticada e o apresenta localmente; abrir diretamente a URL sem token pode resultar em sessão não autenticada.

## Recomendações para análise

1. Identifique a versão e o cenário executado.
2. Registre ambiente, alvo, horário e configuração.
3. Diferencie falha funcional, falha HTTP e limite de performance.
4. Analise percentis, não apenas a média.
5. Preserve evidências sem expor tokens, senhas ou dados pessoais.
