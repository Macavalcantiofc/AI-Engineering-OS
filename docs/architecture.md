# Architecture

```text
AI-Engineering-OS
├── knowledge/       # fatos, conceitos e padrões validados
├── learning/        # trilhas, gaps e progresso
├── research/        # descobertas e tecnologias emergentes
├── cases/           # problemas reais, experimentos e resultados
├── agents/          # personas, contratos e quality gates
├── cost-engine/     # estimativa e otimização de custo de IA
├── decisions/       # ADRs e decisões reversíveis/irreversíveis
├── questions/       # backlog de dúvidas
├── summit/          # eventos e talks
└── templates/       # formatos padronizados
```

## Agentes conceituais
- **Orchestrator**: decide fluxo e delegação.
- **Researcher**: busca evidência e compara fontes.
- **Architect**: valida desenho técnico.
- **Implementer**: transforma especificação em implementação.
- **Reviewer**: procura inconsistências, riscos e regressões.
- **Evaluator**: mede qualidade contra critérios objetivos.
- **Cost Engineer**: estima custo e sugere modelos/estratégias.
- **Knowledge Curator**: transforma resultados em conhecimento versionado.

Os agentes não substituem gates. Cada artefato importante deve ter critérios explícitos de entrada, saída e qualidade.
