# 🔄 AI Engineering OS — Workflow

O OS usa o GitHub como memória versionada e o Pull Request como mecanismo de revisão humana.

## Fluxo

```text
1. Conversa
   ↓
2. Diagnóstico do estado atual
   ↓
3. Proposta da mudança
   ↓
4. Branch dedicada
   ↓
5. Implementação
   ↓
6. Pull Request
   ↓
7. Revisão do Math
   ↓
8. Ajustes, se necessário
   ↓
9. Merge humano
   ↓
10. Verificação do estado final
```

## Regras

- Nunca assumir que uma ideia de conversa deve entrar no repositório.
- Nunca alterar `main` diretamente para uma evolução normal do OS.
- Nunca considerar silêncio como aprovação.
- O PR deve explicar o que mudou, por que mudou e quais arquivos foram afetados.
- O usuário decide o merge.
- Depois do merge, verificar se o estado do repositório corresponde ao que foi aprovado.

## Por que PR?

Além de proteger o repositório, o PR funciona como checkpoint de conhecimento.

A conversa é naturalmente volátil. O PR cria uma fronteira explícita entre:

**"estamos pensando sobre isso"**

e

**"decidimos preservar isso no OS"**.

Isso reduz dependência de contexto conversacional e mantém o sistema auditável.

## Tamanho das mudanças

Preferir PRs pequenos e coerentes.

Um PR deve representar uma unidade compreensível de evolução, evitando misturar uma nova trilha de conhecimento, uma mudança arquitetural e uma reorganização estrutural sem necessidade.

## Critério de conclusão

Uma mudança está concluída quando:

- foi implementada na branch;
- o PR descreve a intenção e o impacto;
- o usuário revisou;
- o usuário fez o merge;
- o estado final foi verificado.

**O bot pode abrir a porta. Quem entra no `main` é o humano.** 🚪
