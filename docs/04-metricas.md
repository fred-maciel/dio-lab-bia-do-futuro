# Avaliação e Métricas

## Como Avaliar seu Agente

A avaliação pode ser feita de duas formas complementares:

1. **Testes estruturados:** Você define perguntas e respostas esperadas;
2. **Feedback real:** Pessoas testam o agente e dão notas.

---

## Métricas de Qualidade

| Métrica | O que avalia | Exemplo de teste |
|---------|--------------|------------------|
| **Assertividade** | O agente respondeu o que foi perguntado? | Perguntar o saldo e receber o valor correto |
| **Segurança** | O agente evitou inventar informações? | Perguntar algo fora do contexto e ele admitir que não sabe |
| **Coerência** | A resposta faz sentido para o perfil do cliente? | Sugerir investimento conservador para cliente conservador |

> [!TIP]
> Peça para 3-5 pessoas (amigos, família, colegas) testarem seu agente e avaliarem cada métrica com notas de 1 a 5. Isso torna suas métricas mais confiáveis! Caso use os arquivos da pasta `data`, lembre-se de contextualizar os participantes sobre o **cliente fictício** representado nesses dados.

---

## Exemplos de Cenários de Teste

Crie testes simples para validar seu agente:

### Teste 1: Consulta e categorização de gastos
- **Pergunta:** "Quanto gastei com licenças de software e infraestrutura mês passado?"
- **Resposta esperada:** O agente deve filtrar o `transacoes.csv`, somar apenas as categorias operacionais (PJ) e retornar o valor exato sem misturar com despesas pessoais.
- **Resultado:** [x] Correto  [ ] Incorreto

### Teste 2: Recomendação de produto com restrição
- **Pergunta:** "Qual investimento de curto prazo você recomenda para alocar o caixa excedente dos meus serviços este mês?"
- **Resposta esperada:** O agente consulta o perfil_investidor.json (ex: Moderado) e sugere um ativo do produtos_financeiros.json que possua alta liquidez e baixo risco (como um CDB Diário), justificando a escolha.
- **Resultado:** [x] Correto  [ ] Incorreto

### Teste 3: Pergunta fora do escopo técnico
- **Pergunta:** "FINFAST, como eu configuro um servidor Nginx para hospedar minha aplicação web?"
- **Resposta esperada:** O agente identifica que a pergunta é sobre infraestrutura de TI e informa educadamente que seu escopo de atuação é estritamente financeiro.
- **Resultado:** [x] Correto  [ ] Incorreto

### Teste 4: Informação inexistente / Prevenção de Alucinação
- **Pergunta:** "Quanto estão rendendo as ações da Petrobras hoje?"
- **Resposta esperada:** O agente verifica o produtos_financeiros.json, não encontra o ativo "Petrobras", admite não ter a informação na base homologada e recusa a recomendação.
- **Resultado:** [x] Correto  [ ] Incorreto

---

## Resultados

Após os testes, registre suas conclusões:

**O que funcionou bem:**
- [A separação rigorosa entre despesas de pessoa física e jurídica (PF/PJ) nas análises de fluxo de caixa funcionou perfeitamente graças à categorização prévia dos dados.]
- -[As tags XML no System Prompt garantiram que o agente não alucinasse em nenhum momento ao recomendar produtos de investimento. O bloqueio contra respostas fora do escopo financeiro foi 100% eficaz.]

**O que pode melhorar:**
- [A formatação das tabelas na saída das respostas pode ser padronizada (ex: forçar o agente a sempre devolver resumos em formato Markdown limpo) para facilitar a futura extração desses dados para um dashboard no Looker Studio ou Power BI.]
- [A latência (tempo de resposta) apresentou um leve aumento quando o agente precisou somar muitas linhas do arquivo CSV. Uma etapa de pré-processamento via script antes de enviar ao LLM pode otimizar isso.]

---

## Métricas Avançadas (Opcional)

Para quem quer explorar mais, algumas métricas técnicas de observabilidade também podem fazer parte da sua solução, como:

- Latência e tempo de resposta: Fundamental para garantir que a interface do usuário não trave durante a leitura dos CSVs;
- Consumo de tokens e custos: Monitorar o tamanho do "Contexto" gerado pelos arquivos estáticos para não estourar o limite de tokens da API a cada pergunta do usuário;
- Logs e taxa de erros: Registrar falhas na conversão dos formatos (ex: datas lidas incorretamente do CSV pelo modelo) para garantir a precisão matemática da análise.

Ferramentas especializadas em LLMs, como [LangWatch](https://langwatch.ai/) e [LangFuse](https://langfuse.com/), são exemplos que podem ajudar nesse monitoramento. Entretanto, fique à vontade para usar qualquer outra que você já conheça!
