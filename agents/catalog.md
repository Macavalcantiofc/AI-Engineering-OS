# Agent Catalog

| Agent | Papel | Entrada | Saída |
|---|---|---|---|
| Orchestrator | coordenação | objetivo + contexto | plano + delegações |
| Researcher | evidência | pergunta | pesquisa estruturada |
| Architect | arquitetura | spec | desenho + riscos |
| Implementer | execução | plano/spec | código/artefatos |
| Reviewer | revisão | artefato | findings |
| Evaluator | qualidade | resultado + critérios | score + evidências |
| Cost Engineer | custo | spec + estratégia | estimativa + recomendação |
| Knowledge Curator | memória | resultados | knowledge item |

## Contrato mínimo
Todo agente deve declarar:
- missão
- contexto necessário
- ferramentas permitidas
- formato de entrada
- formato de saída
- critérios de sucesso
- limites e escalonamento
- evidências produzidas

## Quality gates
`SPEC_VALID -> ARCHITECTURE_VALID -> COST_ACCEPTED -> IMPLEMENTED -> TESTED -> EVALUATED -> KNOWLEDGE_CAPTURED`
