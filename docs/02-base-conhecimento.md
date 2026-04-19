# Base de Conhecimento

## Dados Utilizados

Descreva se usou os arquivos da pasta `data`, por exemplo:

| Arquivo | Formato | Utilização no Agente |
|---------|---------|---------------------|
| `historico_atendimento.csv` | CSV | Contextualizar interações anteriores |
| `perfil_investidor.json` | JSON | Personalizar recomendações |
| `produtos_financeiros.json` | JSON | Sugerir produtos adequados ao perfil |
| `transacoes.csv` | CSV | Analisar padrão de gastos do cliente |

> [!TIP]
> **Quer um dataset mais robusto?** Você pode utilizar datasets públicos do [Hugging Face](https://huggingface.co/datasets) relacionados a finanças, desde que sejam adequados ao contexto do desafio.

---

## Adaptações nos Dados

[Para elevar a complexidade do desafio e tornar o agente mais aderente a cenários reais (como a gestão financeira mista de profissionais independentes e consultores), os dados mockados originais sofreram as seguintes expansões estruturais:

Enriquecimento do transacoes.csv:

Categorização Granular: Adição da coluna categoria (ex: Moradia, Alimentação, Infraestrutura de TI, Impostos).

Flag de Natureza: Inclusão da coluna natureza_transacao (PF ou PJ). Isso permite que o agente atue como um auditor, identificando e alertando o usuário caso ele esteja misturando despesas pessoais com custos operacionais da sua prestação de serviços.

Expansão do perfil_investidor.json:

Métricas de Segurança: Criação dos campos reserva_emergencia_alvo e caixa_operacional_minimo. Com isso, o agente ganha parâmetros matemáticos claros para calcular quanto do saldo atual está ocioso e pode ser destinado a investimentos de menor liquidez.

Objetivos: Adição de um array metas_curto_prazo (ex: "Fundo para impostos anuais", "Aquisição de equipamentos").

Normalização em produtos_financeiros.json:

Atributos Parametrizados: Inclusão de liquidez_dias e risco_score (numérico de 1 a 5) em cada produto do catálogo. Essa modificação técnica força o LLM a realizar um match lógico e exato com a tolerância do cliente, reduzindo drasticamente a margem para alucinações nas recomendações.]

---

## Estratégia de Integração

### Como os dados são carregados?
> [A aplicação utiliza a biblioteca Pandas para carregar os arquivos CSV e a biblioteca nativa json para os arquivos de configuração. O carregamento ocorre no startup da aplicação (session state do Streamlit), garantindo performance. Os dados são processados para gerar resumos estatísticos (ex: soma de gastos por categoria) antes de serem injetados no prompt, economizando tokens e focando no que é relevante.]

### Como os dados são usados no prompt?
> Os dados vão no system prompt? São consultados dinamicamente?

[O script lê as bases.

Gera-se um resumo em texto estruturado (Markdown).

Esse resumo é inserido no campo Contexto do prompt enviado ao LLM, delimitado por tags como <contexto_financeiro>.]

---

## Exemplo de Contexto Montado

> Mostre um exemplo de como os dados são formatados para o agente.

```
Dados do Cliente:

Nome: Fred Maciel

Perfil: Moderado (Foco em equilíbrio entre segurança e rentabilidade)

Status: Profissional de TI / Consultoria PJ

Reserva de Emergência Atual: R$ 12.000,00 (Meta: R$ 30.000,00)

Análise de Transações (Últimos 30 dias):

Receita Total: R$ 15.000,00

Gastos Essenciais: R$ 6.000,00 (Aluguel, Nuvem, Impostos)

Gastos Variáveis: R$ 2.500,00 (Aumento de 12% em relação ao mês anterior em 'Alimentação')

Capacidade de Investimento Identificada: R$ 6.500,00

Produtos Disponíveis Compatíveis:

CDB Pós-Fixado Liquidez Diária (Para composição de reserva)

LCI/LCA 90 dias (Para excedente de caixa de curto prazo)
```
