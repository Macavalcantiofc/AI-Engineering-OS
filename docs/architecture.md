# Architecture

```text
AI-Engineering-OS
├── knowledge/       # conhecimento consolidado
├── learning/        # trilhas, progresso e revisão
├── research/        # pesquisas e tecnologias relevantes
├── cases/           # práticas, experimentos e aprendizados
├── questions/       # dúvidas e gaps
├── summit/          # aprendizados de eventos
├── agents/          # papéis do assistente de aprendizagem
├── decisions/       # ADRs e decisões do próprio OS
└── templates/       # formatos padronizados
```

## Propósito arquitetural
O repositório é um sistema pessoal de aprendizagem contínua. Seu objetivo é organizar conhecimento, pesquisa, prática, revisão e evolução profissional.

Não é um produto comercial, nem um projeto de IA específico. Projetos externos não devem ser incorporados ao OS como domínio próprio.

## Assistente de aprendizagem
Os papéis conceituais apoiam o ciclo de aprendizagem:
- **Orchestrator**: coordena o fluxo de aprendizagem e decide quando pesquisar, experimentar, revisar ou registrar.
- **Researcher**: busca evidências e compara fontes.
- **Architect**: ajuda a analisar arquitetura, padrões e trade-offs como conteúdo de aprendizagem.
- **Reviewer**: procura inconsistências e lacunas no conhecimento.
- **Evaluator**: testa entendimento e mede resultados de experimentos/revisões.
- **Knowledge Curator**: transforma resultados validados em conhecimento versionado.

Esses papéis são mecanismos do assistente de aprendizagem, não projetos independentes.

## Ciclo
`Learn -> Capture -> Validate -> Experiment -> Review -> Connect -> Teach`

Cada artefato deve existir porque contribui para esse ciclo. Evitar estrutura ou automação que não agregue valor ao aprendizado.
