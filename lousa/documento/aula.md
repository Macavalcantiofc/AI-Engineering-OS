# 01. Documento

## 1. O que estamos tentando resolver?

Antes de falar em RAG, embeddings ou vector databases, existe uma pergunta muito mais básica:

> **Como transformar informação existente em algo que um sistema de IA consiga utilizar de forma confiável?**

Imagine que você tenha:

-  PDF 
-  documentação técnica 
-  Markdown 
-  páginas HTML 
-  contratos 
-  tickets 
-  e-mails 
-  código 
-  planilhas 
-  imagens contendo texto 
-  documentos escaneados 

Para nós, humanos, tudo isso pode ser simplesmente chamado de **documento**.

Para um sistema de IA, não é tão simples.

Um PDF, por exemplo, não é necessariamente "texto". Ele pode conter:

```
```

```
PDF
├── texto
├── títulos
├── tabelas
├── imagens
├── cabeçalhos
├── rodapés
├── páginas
├── metadados
└── ordem visual
```

O primeiro problema de AI Engineering é justamente transformar essa informação em uma representação que preserve aquilo que importa.

---

# 2. Documento não é simplesmente texto

Essa é uma distinção importante.

Podemos pensar em três níveis:

```
```

```
Documento original
       ↓
Conteúdo extraído
       ↓
Representação estruturada
```

Por exemplo:

### Documento original

Um PDF contendo:

> Manual de autenticação
>
> 3.2 OAuth 2.0
>
> O sistema utiliza Authorization Code Flow\...
>
> [Tabela de configurações]

Depois da extração, podemos ter:

```
```

```
Manual de autenticação

3.2 OAuth 2.0

O sistema utiliza Authorization Code Flow...
```

Mas talvez tenhamos perdido:

-  que aquilo era um título; 
-  que determinado texto pertencia à seção 3.2; 
-  que uma tabela estava relacionada à seção; 
-  número da página; 
-  posição; 
-  relação entre elementos. 

Então existe uma questão fundamental:

> **Extrair texto não é necessariamente extrair conhecimento.**

---

# 3. Por que isso importa para IA?

Porque todas as etapas posteriores dependem da qualidade dessa representação.

Nossa cadeia é:

```
```

```
Documento
   ↓
Chunking
   ↓
Embedding
   ↓
Vector Store
   ↓
Retrieval
   ↓
RAG
   ↓
Contexto
   ↓
LLM
```

Se começarmos com uma representação ruim:

```
```

```
Documento ruim
      ↓
Chunking ruim
      ↓
Embedding ruim
      ↓
Retrieval ruim
      ↓
RAG ruim
```

Existe uma espécie de efeito cascata.

Por isso a frase:

> **RAG não começa em RAG.**

Na verdade, ele começa muito antes.

---

# 4. O que é ingestão?

Aqui aparece um conceito importante de AI Engineering:

**Ingestion**, ou ingestão.

É o processo de pegar uma fonte externa e transformá-la em dados utilizáveis pelo sistema.

Simplificando:

```
```

```
Fonte
 ↓
Ingestão
 ↓
Representação
 ↓
Processamento
 ↓
Armazenamento
```

Exemplo:

```
```

```
manual.pdf
   ↓
PDF parser
   ↓
texto + estrutura + metadata
   ↓
document representation
```

A ingestão pode envolver várias etapas:

```
```

```
Download
   ↓
Parsing
   ↓
Extraction
   ↓
Normalization
   ↓
Structuring
   ↓
Metadata enrichment
   ↓
Validation
```

Nem todo sistema precisa de todas elas, mas conceitualmente são problemas diferentes.

---

# 5. Parsing ≠ Extraction ≠ Transformation

Esses conceitos costumam ser misturados.

### Parsing

É entender a estrutura do formato.

Exemplo:

```
```

```
PDF
HTML
JSON
DOCX
```

O parser tenta interpretar aquele formato.

### Extraction

É extrair o conteúdo relevante.

Por exemplo:

```
```

```
HTML
 ↓
texto
títulos
links
tabelas
```

### Transformation

É transformar esse conteúdo em uma representação adequada ao nosso sistema.

Por exemplo:

```
```

```
{
  "content": "OAuth 2.0...",
  "title": "OAuth 2.0",
  "section": "3.2",
  "page": 17
}
```

Isso já é muito mais interessante para uma arquitetura de AI.

---

# 6. Metadata

Aqui está um dos conceitos mais importantes deste tema.

Além do conteúdo, normalmente queremos carregar **contexto sobre o conteúdo**.

Exemplo:

```
```

```
{
  "content": "O sistema utiliza Authorization Code Flow.",
  "metadata": {
    "source": "authentication-guide.pdf",
    "page": 17,
    "section": "3.2",
    "document_type": "technical_document",
    "language": "pt-BR",
    "created_at": "2026-01-10"
  }
}
```

Observe:

```
```

```
content
```

é o conteúdo.

Enquanto:

```
```

```
metadata
```

descreve aquele conteúdo.

---

# 7. Por que metadata é importante?

Porque posteriormente podemos querer fazer perguntas como:

> "Encontre informações sobre OAuth."

Mas talvez também:

> "Encontre informações sobre OAuth **somente na documentação de autenticação**."

Ou:

> "Encontre informações sobre OAuth **na versão mais recente**."

Ou:

> "Encontre informações sobre OAuth **produzidas pela equipe X**."

O conteúdo sozinho pode não ser suficiente.

Metadata permite adicionar dimensões de filtragem.

```
```

```
Query
 ↓
Retrieval
 ↓
Semantic similarity
 +
Metadata filters
```

Isso vai ser extremamente importante quando chegarmos em Retrieval.

---

# 8. Identidade do documento

Outra questão arquitetural:

> **Como sabemos de onde determinada informação veio?**

Imagine que o sistema responda:

> "O token expira em 60 minutos."

Excelente.

Mas...

**De onde veio isso?**

Precisamos conseguir rastrear.

Por isso uma representação de documento frequentemente possui algo como:

```
```

```
{
  "document_id": "doc-123",
  "source": "authentication-guide.pdf",
  "version": "v3",
  "content": "...",
  "metadata": {}
}
```

Isso cria uma relação:

```
```

```
Resposta
   ↓
Chunk
   ↓
Documento
   ↓
Fonte original
```

Esse conceito de **proveniência** ou **traceability** será muito importante posteriormente.

---

# 9. Documento, versão e atualização

Agora aparece um problema real.

Imagine:

```
```

```
authentication-guide.pdf
v1
```

Depois:

```
```

```
authentication-guide.pdf
v2
```

O sistema precisa saber:

-  é o mesmo documento? 
-  é uma nova versão? 
-  devemos remover a antiga? 
-  devemos manter ambas? 
-  qual é a atual? 
-  quando foi atualizada? 

Se você simplesmente fizer:

```
```

```
v1 → ingest
v2 → ingest
```

pode acabar com:

```
```

```
Vector Store

"Token expira em 60 minutos."
"Token expira em 30 minutos."
```

E agora temos um problema de conhecimento conflitante.

Isso não é um problema de LLM.

É um problema de **data lifecycle**.

---

# 10. Document lifecycle

Começamos a perceber que documentos têm um ciclo de vida:

```
```

```
Created
   ↓
Ingested
   ↓
Processed
   ↓
Indexed
   ↓
Updated
   ↓
Reprocessed
   ↓
Deprecated
   ↓
Deleted
```

Uma arquitetura madura precisa considerar isso.

Porque RAG não é apenas:

> "jogue PDF no vector database."

É um **pipeline de conhecimento**.

---

# 11. O problema dos documentos ruins

Imagine um PDF visualmente perfeito:

```
```

```
┌──────────────────────────────┐
│  Arquitetura                 │
│                              │
│  ┌─────────┐   ┌─────────┐ │
│  │ Service │ → │ Database│ │
│  └─────────┘   └─────────┘ │
│                              │
│  O sistema utiliza...        │
└──────────────────────────────┘
```

O parser pode produzir:

```
```

```
Arquitetura
Service Database
O sistema utiliza...
```

Para um humano:

> "Tranquilo."

Para o sistema:

> "Service Database O sistema utiliza..."

A estrutura semântica foi parcialmente destruída.

Isso nos leva a uma ideia importante:

> **Document processing é uma etapa de preservação de significado.**

Não queremos apenas preservar caracteres.

Queremos preservar **relações relevantes**.

---

# 12. Tipos de documento

Documentos possuem características diferentes.

### Texto simples

```
```

```
TXT
Markdown
```

Relativamente simples.

### Estruturados

```
```

```
HTML
JSON
XML
```

Possuem estrutura explícita.

### Documentos ricos

```
```

```
PDF
DOCX
PPTX
```

Podem conter múltiplas camadas de estrutura.

### Dados tabulares

```
```

```
CSV
XLSX
```

A relação entre linhas e colunas pode ser essencial.

### Documentos visuais

```
```

```
scanned PDF
images
diagrams
```

Podem exigir OCR ou modelos multimodais.

Portanto:

> **Não existe um único pipeline universal de ingestão perfeito para todos os documentos.**

---

# 13. Texto vs estrutura

Considere:

```
```

```
Tabela

Plano       Preço      Limite
Basic       $10        10 users
Pro         $50        100 users
Enterprise  $200       Unlimited
```

Se simplesmente transformarmos em:

```
```

```
Plano Preço Limite Basic $10 10 users Pro $50 100 users Enterprise $200 Unlimited
```

parte da relação entre os valores pode ficar ambígua.

Uma representação melhor poderia preservar:

```
```

```
{
  "type": "table",
  "headers": ["Plano", "Preço", "Limite"],
  "rows": [
    ["Basic", "$10", "10 users"],
    ["Pro", "$50", "100 users"],
    ["Enterprise", "$200", "Unlimited"]
  ]
}
```

Essa decisão terá impacto direto no que será possível recuperar depois.

---

# 14. Normalização

Documentos podem possuir ruído:

```
```

```
Página 1
Página 2
Página 3

CONFIDENTIAL
CONFIDENTIAL
CONFIDENTIAL
```

Ou:

```
```

```
Header
conteúdo
Footer
```

Se isso for repetido em milhares de páginas, podemos contaminar a representação.

Normalização pode envolver coisas como:

-  remover artefatos; 
-  normalizar espaços; 
-  corrigir encoding; 
-  remover headers/footers repetidos; 
-  padronizar caracteres; 
-  tratar quebras de linha; 
-  normalizar estruturas. 

Mas existe uma armadilha:

> **Limpar demais também pode destruir informação.**

Isso é um trade-off.

---

# 15. O princípio "lossless until necessary"

Uma boa heurística arquitetural é:

> **Não descarte informação cedo demais sem saber que ela é irrelevante.**

Imagine:

```
```

```
PDF
 ↓
extrai apenas texto
 ↓
descarta tabelas
 ↓
descarta páginas
 ↓
descarta headings
 ↓
descarta metadata
```

Talvez você tenha criado um pipeline simples.

Mas depois descubra:

> "Precisamos responder perguntas baseadas em tabelas."

Agora a informação já foi perdida.

Por isso pipelines de ingestão devem pensar cuidadosamente sobre **onde ocorre perda de informação**.

---

# 16. Documento como objeto de conhecimento

Uma maneira mais madura de pensar:

Não trate documento apenas como:

```
```

```
string
```

Pense:

```
```

```
Document
├── identity
├── source
├── version
├── metadata
├── structure
├── content
├── provenance
└── lifecycle
```

E talvez:

```
```

```
Document
 ├── Section
 │    ├── Paragraph
 │    ├── Table
 │    └── Image
 │
 └── Section
      ├── Paragraph
      └── Code
```

Essa estrutura pode ou não ser preservada dependendo do sistema.

Mas **pensar dessa maneira** ajuda a evitar uma simplificação perigosa:

> Documento = texto.

---

# 17. Onde entra o Chunking?

Só depois de termos uma representação razoável do documento.

```
```

```
Documento
    ↓
Ingestão
    ↓
Representação estruturada
    ↓
Chunking
```

Chunking responde:

> **Como dividir esse conteúdo em unidades que possam ser recuperadas?**

Mas observe:

Se não entendemos o documento, podemos fazer chunking errado.

Por exemplo:

```
```

```
Tabela
 ↓
chunk arbitrário
```

podemos separar:

```
```

```
Plano | Preço
```

de:

```
```

```
Limite
```

E destruir o significado.

Portanto, o próximo tema depende diretamente deste.

---

# 18. O grande problema de engenharia

O problema real não é:

> "Como faço upload de um PDF?"

É:

> **Como transformar fontes heterogêneas de informação em uma representação confiável, rastreável e útil para as etapas posteriores de recuperação e geração?**

Essa formulação é muito mais importante.

Porque agora conseguimos enxergar vários requisitos:

```
```

```
                    ┌── qualidade
                    ├── estrutura
Documento ──────────┼── metadata
                    ├── provenance
                    ├── versionamento
                    ├── lifecycle
                    └── rastreabilidade
```

---

# 19. Arquitetura conceitual

Uma arquitetura inicial poderia ser:

```
```

```
              ┌─────────────────┐
              │     Sources     │
              │ PDF / HTML / MD │
              │ DOCX / XLSX ... │
              └────────┬────────┘
                       │
                       ▼
              ┌─────────────────┐
              │    Ingestion    │
              └────────┬────────┘
                       │
                       ▼
              ┌─────────────────┐
              │     Parsing     │
              └────────┬────────┘
                       │
                       ▼
              ┌─────────────────┐
              │   Extraction    │
              └────────┬────────┘
                       │
                       ▼
              ┌─────────────────┐
              │ Normalization   │
              └────────┬────────┘
                       │
                       ▼
              ┌─────────────────┐
              │  Representation │
              │ + Metadata      │
              └────────┬────────┘
                       │
                       ▼
                 ┌────────────┐
                 │  Chunking  │
                 └────────────┘
```

Depois entram embeddings e retrieval.

---

# 20. O que pode dar errado?

Essa é a parte que eu quero que você comece a enxergar como engenheiro.

### Caso 1

O parser extrai texto errado.

**Consequência:**

Todo o pipeline posterior trabalha com informação errada.

### Caso 2

A estrutura de headings é perdida.

**Consequência:**

Chunks podem perder contexto.

### Caso 3

Metadata não é preservada.

**Consequência:**

Filtragem e rastreabilidade ficam prejudicadas.

### Caso 4

Documento antigo continua indexado.

**Consequência:**

Retrieval pode retornar informação obsoleta.

### Caso 5

Tabela é transformada em texto sem estrutura.

**Consequência:**

Relações entre valores podem ser perdidas.

### Caso 6

OCR erra uma palavra crítica.

**Consequência:**

O sistema pode recuperar informação incorreta.

### Caso 7

Normalização remove informação relevante.

**Consequência:**

Perda silenciosa de conhecimento.

E essa última é especialmente perigosa.

---

# 21. Como avaliar a qualidade?

Ainda não vamos entrar profundamente em avaliação, mas já podemos começar a pensar.

Perguntas importantes:

### Fidelity

O conteúdo extraído representa corretamente a fonte?

### Completeness

Perdemos informação?

### Structure preservation

Mantivemos relações importantes?

### Metadata quality

Sabemos de onde veio cada informação?

### Freshness

Estamos trabalhando com a versão correta?

### Traceability

Conseguimos voltar até a fonte original?

Isso será fundamental quando chegarmos em **Evaluation e Observability** mais adiante.

---

# 22. Documento e RAG

Agora podemos montar o quebra-cabeça:

```
```

```
                    RAG
                     │
                     ▼
                 Retrieval
                     │
                     ▼
                  Embedding
                     │
                     ▼
                  Chunking
                     │
                     ▼
              Document Representation
                     │
                     ▼
                  Ingestion
                     │
                     ▼
                   Source
```

Ou seja:

**RAG é apenas uma camada de uma cadeia muito maior.**

E quanto mais avançarmos, mais você vai perceber que muitas falhas atribuídas ao "LLM" na verdade acontecem **antes do modelo receber o contexto**.

---

# 23. O que você deve guardar deste tema

Se eu tivesse que reduzir toda essa aula a **10 ideias**, seriam estas:

1. **Documento não é necessariamente texto.** 
2. **Extrair texto não significa preservar conhecimento.** 
3. **Ingestão transforma fontes externas em dados utilizáveis.** 
4. **Parsing, extraction e transformation são problemas diferentes.** 
5. **Estrutura pode ser tão importante quanto conteúdo.** 
6. **Metadata adiciona contexto operacional ao conteúdo.** 
7. **Proveniência permite rastrear informação até sua fonte.** 
8. **Versionamento e lifecycle são parte do problema de conhecimento.** 
9. **Perder informação cedo demais pode comprometer todo o pipeline.** 
10. **A qualidade do RAG começa antes do RAG.** 

---

## 🎯 E uma provocação para você

Não precisa responder agora. Leia, pense e depois conversamos.

Imagine que alguém diga:

> **"Para fazer RAG, basta extrair todo o texto do PDF, dividir em pedaços, gerar embeddings e colocar tudo no vector database."**

Você, agora como engenheiro, deveria conseguir responder:

**O que está errado nessa afirmação?**

Essa será uma ótima porta de entrada para nossa conversa e, depois, para a primeira revisão formal do tema.

E por enquanto, **Documento**