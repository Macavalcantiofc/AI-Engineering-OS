# Governance

## Source of truth
O Git repository é a fonte de verdade do OS. Conversas, notas soltas e ferramentas externas são entradas; decisões e conhecimento relevante devem ser promovidos para o repositório.

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
Decisões arquiteturais relevantes devem gerar ADR. Mudanças estruturais do OS devem atualizar `docs/architecture.md` quando alterarem o modelo conceitual.
