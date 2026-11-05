# MCP: fundamentos e arquitetura

**Estado:** 🟡 Em aprendizado  
**Nível avaliado:** 35%  
**Evidência inicial:** 5/5 questões corretas em diagnóstico conceitual.  
**Escopo:** fundamentos estudados nesta etapa; não representa domínio de implementação ou operação.

## Objetivo de aprendizagem

Compreender o Model Context Protocol (MCP), seus participantes, capacidades, fluxo de execução, estrutura de mensagens e transportes, além de reconhecer as responsabilidades que permanecem na aplicação backend.

## O que é MCP

MCP é um protocolo aberto que padroniza a comunicação entre aplicações de IA e sistemas externos. Ele define contratos e comportamentos para disponibilizar capacidades como Tools, Resources e Prompts.

MCP não é um framework nem uma linguagem. Diferentes linguagens e bibliotecas podem implementar a especificação.

## Arquitetura: Host, Client e Server

- **MCP Host:** aplicação que coordena a experiência de IA, integra o modelo e gerencia quais servidores e capacidades estão disponíveis.
- **MCP Client:** componente gerenciado pelo Host que estabelece a comunicação com um Server, negocia a sessão e descobre capacidades.
- **MCP Server:** disponibiliza capacidades MCP e recebe solicitações, encaminhando-as aos handlers da aplicação.

O modelo não estabelece por conta própria a conexão MCP. O Host/Agent Runtime integra o modelo ao MCP Client e coordena as chamadas.

Client e Server podem estar em processos e deploys separados ou coexistir na mesma aplicação. Uma aplicação que apenas expõe capacidades MCP precisa implementar o Server; não precisa necessariamente conter um Client.

## Capacidades MCP

- **Tools:** funcionalidades executáveis que o modelo pode solicitar, como consultar ou contar registros.
- **Resources:** dados ou conteúdos que podem ser disponibilizados como contexto.
- **Prompts:** templates reutilizáveis para orientar interações.

O Host controla como essas capacidades são apresentadas e utilizadas. Sua disponibilidade não implica execução automática.

## Descoberta e execução de Tools

1. O desenvolvedor configura quais MCP Servers o Host pode utilizar.
2. O MCP Client conecta-se ao Server e descobre as Tools disponíveis, por exemplo, com `tools/list`.
3. O Agent Runtime disponibiliza ao modelo as definições relevantes, incluindo nome, descrição e schema de entrada.
4. O modelo decide se uma Tool é adequada e solicita uma chamada com argumentos.
5. O Runtime valida e encaminha a solicitação pelo MCP Client, usando `tools/call`.
6. O MCP Server executa o handler correspondente.
7. O handler aciona serviços e componentes backend, que aplicam regras de negócio, autorização e acesso a dados.
8. O resultado retorna ao Runtime e ao modelo, que pode continuar o fluxo ou responder ao solicitante.

Resumo: **o Client descobre, o modelo decide, o Server executa e o backend aplica as regras de negócio**. O Runtime faz a orquestração entre essas partes.

A descoberta não precisa ocorrer antes de cada pergunta. O Client pode manter as capacidades descobertas e atualizá-las quando necessário.

## Contrato de uma Tool

Uma definição de Tool inclui, entre outros elementos:

- `name`: identificador da ferramenta.
- `description`: descrição da finalidade e do comportamento.
- `inputSchema`: schema, normalmente JSON Schema, que descreve os argumentos aceitos.

Exemplo didático:

```json
{
  "name": "count_specifications",
  "description": "Conta especificações por ano e tipo de criação.",
  "inputSchema": {
    "type": "object",
    "properties": {
      "year": { "type": "integer" },
      "creationType": {
        "type": "string",
        "enum": ["AUTOMATIC", "MANUAL"]
      }
    },
    "required": ["year", "creationType"]
  }
}
```

O modelo constrói uma solicitação conforme o contrato. Ele não executa SQL nem acessa diretamente classes Java ou o banco de dados. O handler implementado pela aplicação recebe os argumentos e delega a operação aos serviços backend.

## JSON-RPC 2.0

JSON-RPC 2.0 define a estrutura das mensagens de chamada, incluindo versão, método, parâmetros e identificador da requisição.

Exemplo ilustrativo de descoberta:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "tools/list"
}
```

Exemplo ilustrativo de chamada:

```json
{
  "jsonrpc": "2.0",
  "id": 2,
  "method": "tools/call",
  "params": {
    "name": "get_pricing",
    "arguments": { "pricingId": "12345" }
  }
}
```

JSON-RPC, isoladamente, não é MCP. O MCP define os métodos, a semântica, o ciclo de vida e as regras específicas que utilizam mensagens JSON-RPC.

## Transportes

O transporte define como as mensagens chegam ao destino. Os transportes padrão estudados foram:

- **STDIO:** comunicação por entrada e saída padrão entre processos, frequentemente quando o Client inicia ou gerencia um processo Server local.
- **Streamable HTTP:** comunicação com um Server acessível por HTTP. As mensagens MCP continuam estruturadas em JSON-RPC; o transporte pode utilizar respostas JSON ou streaming com `text/event-stream`.

Portanto, JSON-RPC estrutura a mensagem, enquanto STDIO ou Streamable HTTP transportam essa mensagem.

## MCP Java SDK

O MCP Java SDK é uma biblioteca que implementa aspectos do protocolo MCP e oferece APIs para criar Clients e Servers, registrar Tools e lidar com comunicação e transporte.

O SDK não substitui a lógica de negócio, autenticação, autorização, validações ou controles de acesso da aplicação. Os exemplos de API apresentados durante a aula foram conceituais e precisam ser conferidos na documentação da versão utilizada antes de serem tratados como código compilável.

## Segurança e responsabilidades do backend

Expor uma funcionalidade como Tool não transfere ao modelo a autoridade para executá-la. O backend deve continuar validando identidade, permissões, argumentos, regras de negócio e acesso aos dados. Credenciais e acesso ao banco permanecem sob controle da aplicação.

Consultas e alterações têm riscos diferentes. Operações de escrita exigem controles adicionais, como autorização explícita, auditoria, validação e proteção contra repetição indevida.

## Trade-offs de desenho das Tools

- Tools granulares oferecem flexibilidade e composição, mas podem exigir mais chamadas.
- Tools orientadas a casos de uso encapsulam fluxos e podem reduzir chamadas, mas são menos flexíveis e precisam evitar contratos excessivamente amplos.

O contrato público da Tool não precisa espelhar a arquitetura interna. Um handler pode orquestrar vários serviços, desde que a semântica exposta permaneça clara e estável.

## Evidência e avaliação atual

O diagnóstico inicial teve cinco questões sobre:

1. Responsabilidades de Host, Client e Server.
2. Fluxo de decisão e execução de Tools.
3. Papel do JSON-RPC 2.0.
4. Diferenças entre Tools, Resources e Prompts.
5. Diferença entre transporte e estrutura da mensagem.

**Resultado informado pelo usuário:** pontuação perfeita, 5/5.

Esse resultado demonstra reconhecimento correto dos conceitos avaliados. Ainda não comprova capacidade de explicar sem alternativas, implementar um Server/Client, tratar falhas ou operar MCP em produção. Por isso, o nível foi registrado como **35%, 🟡 Em aprendizado**, e não como conhecimento consolidado.

## Tópicos ainda não aprofundados

- Ciclo de vida e inicialização da sessão.
- Negociação de capacidades em maior detalhe.
- Notificações e atualização dinâmica de Tools.
- Implementação prática e testes com MCP Java SDK.
- Autenticação, autorização e segurança do transporte em cenários reais.
- Observabilidade, tratamento de erros e operação.

## Próxima etapa

O estudo de MCP foi encerrado nesta etapa, conforme decisão do aprendiz. Uma retomada futura pode começar pelos tópicos ainda não aprofundados, sem presumir que houve implementação prática.
