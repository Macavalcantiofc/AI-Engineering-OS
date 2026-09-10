# 🧠 AI Engineering OS — Control Room

> Um sistema pessoal para transformar curiosidade técnica em capacidade de engenharia.
>
> **A conversa é o laboratório. O GitHub é o registro. O PR é o gate.**

## 🎯 Onde estamos

**Fase:** Foundation / construção do sistema de aprendizagem

**Objetivo:** sair de *"vi isso no Summit"* para *"entendo o problema, a abstração, a arquitetura, os trade-offs e consigo aplicar"*.

## 🔥 Missão atual

### Desbravar a cadeia de AI Engineering

```text
documento
   ↓
chunking
   ↓
embeddings
   ↓
vector store
   ↓
retrieval
   ↓
RAG
   ↓
contexto
   ↓
LLM
   ↓
tool calling
   ↓
MCP
   ↓
agent
```

A pergunta não é decorar a sequência.

A pergunta é:

> **O que cada camada resolve, por que ela existe, o que ela abstrai e em que momento realmente precisamos dela?**

Vamos partir do fundamento: **RAG não começa em RAG.** Antes dele existem representação de conhecimento, particionamento, indexação, busca e recuperação de informação.

## 🧭 Como aprendemos

```text
Learn
  ↓
Capture
  ↓
Validate
  ↓
Experiment
  ↓
Apply
  ↓
Measure
  ↓
Review
  ↓
Connect
  ↓
Teach
```

Nem todo conceito precisa virar documento.
Nem toda tecnologia precisa virar experimento.
Nem todo slide bonito merece entrar para o OS. 😌

## 🚦 Estado do conhecimento

| Estado | Significado |
|---|---|
| 🔴 Gap | Ainda não conseguimos explicar/aplicar com segurança |
| 🟡 Em aprendizado | Estamos construindo o modelo mental |
| 🟠 Em consolidação | Já entendemos, mas falta evidência/prática |
| 🟢 Consolidado | Evidência e capacidade de explicar/aplicar |
| 🔵 Experimental | Hipótese sendo testada |
| ⚫ Revisar | Conhecimento pode estar obsoleto ou inconsistente |

## 🧪 Regra de ouro

Antes de adicionar algo ao OS:

> **Isso pertence ao meu sistema de aprendizagem e evolução técnica?**

Se não estiver claro, fica fora.

## 🔄 A esteira de mudanças

O `main` não é nosso bloco de notas.

```text
Conversa / ideia
      ↓
Diagnóstico no GitHub
      ↓
Proposta
      ↓
Branch
      ↓
Pull Request
      ↓
👀 Revisão do Math
      ↓
Merge
      ↓
main = estado oficial
```

A IA pode preparar a mudança. **O merge continua sendo humano.**

Isso também resolve um problema importante: contexto de conversa é temporário; o GitHub preserva o estado que foi realmente aprovado.

## 🧠 Contexto de continuidade

O OS pode manter um pequeno registro operacional das conversas recentes, mas ele deve ser tratado como **contexto auxiliar**, não como fonte de verdade.

O princípio será:

- curto o suficiente para não virar um segundo repositório de conhecimento;
- focado em decisões, perguntas abertas, estado atual e próximo passo;
- atualizado somente quando houver uma mudança relevante de contexto;
- nunca substituir `knowledge/`, `decisions/`, `research/` ou outros artefatos oficiais.

> **Resumo de conversa ajuda a retomar. Conhecimento versionado ajuda a lembrar. Evidência ajuda a acreditar.**

## 🤖 O detector de hype

Quando aparecer uma tecnologia nova, vamos perguntar:

1. Qual problema ela resolve?
2. Qual é o conceito fundamental por trás dela?
3. Qual abstração ela introduz?
4. O que existia antes?
5. Quais são as alternativas?
6. Quais são os trade-offs?
7. Temos esse problema no nosso contexto?
8. Temos evidência suficiente para considerar isso conhecimento?

Se depois disso a tecnologia continuar interessante, aí sim abrimos o capô. 🔧

## 🏁 Próximo passo

Desmontar a cadeia **documento → chunking → embeddings → vector store → retrieval → RAG → contexto → LLM → tool calling → MCP → agent**, começando pelos conceitos fundamentais e conectando cada camada à arquitetura.

**Sem correr para implementar um Agent só porque o slide tinha um robô.** 🤖
