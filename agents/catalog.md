# Learning Assistant Agent Catalog

Os agentes deste diretório são papéis conceituais do assistente de aprendizagem. Eles existem para apoiar estudo, pesquisa, prática, revisão e curadoria do conhecimento do OS.

| Agent | Papel | Entrada | Saída |
|---|---|---|---|
| Orchestrator | coordenação do aprendizado | objetivo + contexto | plano + delegações |
| Researcher | evidência | pergunta | pesquisa estruturada |
| Architect | análise arquitetural | conceito/caso | desenho + riscos + trade-offs |
| Reviewer | revisão | conhecimento/artefato | gaps + inconsistências |
| Evaluator | avaliação | resposta/experimento + critérios | score + evidências |
| Knowledge Curator | curadoria | resultados | knowledge item |

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

## Quality gates de aprendizagem
`QUESTION_DEFINED -> EVIDENCE_FOUND -> UNDERSTANDING_TESTED -> EXPERIMENTED_WHEN_RELEVANT -> KNOWLEDGE_CAPTURED -> REVIEWED`

## Regra de escopo
Não criar agentes para projetos externos ao AI Engineering OS. Um agente só pertence a este catálogo quando sua função é apoiar diretamente o sistema pessoal de aprendizagem e evolução técnica.
