# AI Development Cost Engine

## Objetivo
Estimar o custo total de uma execução de desenvolvimento assistido por IA a partir de uma spec, comparar com um orçamento limite e recomendar uma estratégia de modelos que maximize qualidade por custo.

## Dimensões de custo
- tokens de entrada
- tokens de saída
- quantidade de chamadas
- ferramentas e agentes utilizados
- retries
- contexto recuperado
- execução de testes
- infraestrutura complementar
- custo de revisão humana, quando desejado

## Estratégia
O engine não deve simplesmente escolher o modelo mais barato. Deve otimizar uma função de valor:

`quality >= threshold` AND `risk <= threshold` AND `cost <= budget`

Dentro dessas restrições, minimizar custo e latência.

## Pipeline
1. Parse da spec
2. Classificação de complexidade
3. Decomposição por etapa
4. Estimativa de tokens/chamadas
5. Geração de cenários low/base/high
6. Seleção de modelos por etapa
7. Cálculo do custo total
8. Comparação com limite
9. Recomendações de otimização
10. Registro da estimativa para posterior comparação com o realizado

## Métrica importante
Guardar `estimated_cost` e `actual_cost` para calibrar o modelo continuamente.
