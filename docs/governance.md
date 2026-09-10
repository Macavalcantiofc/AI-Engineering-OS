# Governance

## Source of truth
O Git repository é a fonte de verdade do OS. Conversas, notas soltas e ferramentas externas são entradas; decisões e conhecimento relevante devem ser promovidos para o repositório.

## Escopo
O repositório deve permanecer focado no sistema pessoal de aprendizagem: estudo, pesquisa, prática, experimentação, revisão, curadoria e evolução profissional.

Projetos externos, produtos e ideias independentes não devem ser incorporados como partes do OS. Quando forem úteis para aprender, podem ser registrados como cases ou exemplos de aprendizagem, sem transformar o projeto externo em domínio do repositório.

## Vendor neutrality
AWS, Google Cloud, Azure, provedores de modelos e ferramentas de desenvolvimento são implementações ou fontes de evidência. Conceitos fundamentais e padrões reutilizáveis devem ser registrados independentemente do fornecedor quando possível.

## Evidence
- `L0` hipótese
- `L1` evidência externa
- `L2` experimento próprio
- `L3` aplicação real
- `L4` padrão validado/repetível

## Review cadence
- semanal: dúvidas, progresso e próximos experimentos
- mensal: revisão da roadmap e gaps
- trimestral: revisão da arquitetura do OS, decisões e taxonomia

## Promotion
`question/research/case -> knowledge` exige evidência e uma conclusão explícita.

## Decision hygiene
Decisões arquiteturais relevantes do próprio OS devem gerar ADR. Mudanças estruturais do OS devem atualizar `docs/architecture.md` quando alterarem o modelo conceitual.

## Duplication
Antes de criar uma nova anotação, procurar conteúdo existente. Preferir atualizar, complementar, corrigir ou conectar conhecimento existente em vez de criar duplicatas.
