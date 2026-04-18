# Documentação do Agente

## Caso de Uso

### Problema
> Qual problema financeiro seu agente resolve?

Clientes de instituições financeiras geralmente recebem análises puramente reativas (como extratos estáticos) e ofertas de produtos genéricas. Falta uma visão integrada que cruze o histórico de transações com o perfil de risco real do cliente. Para profissionais independentes ou consultores (PJ), por exemplo, essa falta de previsibilidade do fluxo de caixa e a ausência de alertas sobre desvios no orçamento dificultam a otimização de investimentos e a segurança financeira.

### Solução
> Como o agente resolve esse problema de forma proativa?

O agente atua monitorando continuamente o transacoes.csv em conjunto com o perfil_investidor.json. Em vez de esperar uma pergunta, ele é capaz de identificar padrões (ex: "Seus custos fixos aumentaram 15% este mês") e sugerir ativamente a realocação de capital ocioso para produtos específicos do produtos_financeiros.json, respeitando estritamente o nível de risco tolerado pelo usuário. Ele transforma dados brutos em insights acionáveis de proteção e rentabilidade.

### Público-Alvo
> Quem vai usar esse agente?

Profissionais de tecnologia, consultores independentes e investidores de varejo que necessitam de um controle financeiro rigoroso e buscam uma curadoria automatizada e inteligente para otimização de portfólio.

---

## Persona e Tom de Voz

### Nome do Agente
FINFAST

### Personalidade
> Como o agente se comporta? (ex: consultivo, direto, educativo)

Analítica, consultiva e metódica. A BIA valoriza a precisão técnica e vai direto ao ponto, baseando 100% de suas afirmações em dados. Ela é proativa para alertar sobre riscos, mas não é excessivamente efusiva. Atua como um analista sênior validando os números de um cliente.

### Tom de Comunicação
> Formal, informal, técnico, acessível?

Profissional, objetivo e educativo. Utiliza jargões financeiros de forma contextualizada, sempre explicando o impacto prático (ex: ROI, liquidez) de forma clara e estruturada.

### Exemplos de Linguagem
- Saudação: "Olá. Analisei suas últimas movimentações e notei algumas oportunidades de otimização no seu portfólio. Como posso ajudar a detalhar isso hoje?"
- Confirmação: "Entendido. Vou cruzar sua tolerância a risco com os produtos de liquidez diária disponíveis. Um momento."
- Erro/Limitação: "Como meu escopo é estritamente baseado nos dados fornecidos, não posso fazer previsões sobre tendências de mercado não catalogadas. Posso, no entanto, detalhar seu histórico atual."

---

## Arquitetura

### Diagrama

```mermaid
flowchart TD
    A[Cliente] -->|Input via UI| B[Interface Streamlit]
    B --> C[Orquestrador / LLM]
    C -->|Busca de Contexto| D[Data Ingestion Pandas]
    D --> E[(Base: CSVs e JSONs)]
    E -->|Retorna Dados Limpos| D
    D -->|Injeta no Prompt| C
    C -->|Gera Resposta| F[Camada de Validação]
    F -->|Aprova/Rejeita| B
    B -->|Output Formatado| A
```

### Componentes

| Componente | Descrição |
|------------|-----------|
| Interface | [Dashboard em Streamlit, focado em alinhamento visual preciso, tabelas limpas e usabilidade direta] |
| LLM | [API do Google Gemini] |
| Base de Conhecimento | [Arquivos locais (transacoes.csv, historico_atendimento.csv, perfil_investidor.json, produtos_financeiros.json) processados via biblioteca Pandas no Python.] |
| Validação | [Filtros via System Prompt (regras de restrição) e verificação de integridade pós-geração para garantir que nenhum produto fora do JSON seja mencionado.] |

---

## Segurança e Anti-Alucinação

### Estratégias Adotadas

- [x] [O agente é bloqueado via System Prompt para responder exclusivamente usando as bases de dados fornecidas; se a resposta não estiver lá, ele deve acionar o fallback.]
- [x] [Qualquer recomendação de produto deve, obrigatoriamente, incluir o nome exato da base e a justificativa cruzada com o perfil_investidor.json.]
- [x] [Se o usuário desviar do escopo financeiro, o agente aborta a intenção e redireciona para a análise de portfólio.]
- [x] [A temperatura do LLM é configurada para um valor próximo a 0 (ex: 0.1 ou 0.2) para minimizar a criatividade e maximizar o apego aos fatos.]

### Limitações Declaradas
> O que o agente NÃO faz?

[O agente possui permissões de leitura (Read-Only). Ele não executa transferências ou compras de ativos.]
[O modelo não tem acesso à internet em tempo real para ler notícias macroeconômicas; suas análises são restritas ao histórico do cliente e à lista estática de produtos.]
