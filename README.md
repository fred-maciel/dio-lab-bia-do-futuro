# FINFAST - Agente Financeiro Inteligente 🚀

O **FINFAST** é um assistente financeiro proativo desenvolvido para o desafio de projeto da DIO. Ele utiliza IA Generativa para analisar dados transacionais, histórico de atendimento e perfis de investimento, oferecendo insights personalizados com foco em segurança (anti-alucinação) e separação de contas (PF/PJ).

## 🛠️ Tecnologias Utilizadas

- **Linguagem:** Python 3.10+
- **Interface:** [Streamlit](https://streamlit.io/)
- **Processamento de Dados:** Pandas
- **Orquestração de IA:** Ollama (Modelo: gpt-oss / Llama 3)
- **Documentação:** Mermaid (Fluxogramas)

## 📁 Estrutura do Repositório

- `data/`: Contém os arquivos JSON e CSV (mockados) que alimentam o agente.
- `docs/`: Documentação detalhada (Caso de uso, Prompts, Métricas e Pitch).
- `src/`: Código-fonte da aplicação (`app.py`).
- `assets/`: Imagens da interface e diagramas.

## 🚀 Como Executar o Projeto

1. **Clone o repositório:**
   Bash
   `git clone https://github.com/fred-maciel/dio-lab-bia-do-futuro.git
   cd dio-lab-bia-do-futuro'`
   
🚀 Instruções para Rodar o Projeto

Siga os passos abaixo para configurar o ambiente e executar o FINFAST na sua máquina local:

1. Requisitos Prévios
Certifique-se de ter o Python instalado e o Ollama rodando em sua máquina.

Download Ollama

No terminal, baixe o modelo necessário:

Bash
ollama run llama3
2. Clonar o Repositório
Bash
git clone https://github.com/fred-maciel/dio-lab-bia-do-futuro.git
cd dio-lab-bia-do-futuro
3. Instalar Dependências
Recomenda-se o uso de um ambiente virtual (venv), mas você pode instalar as bibliotecas diretamente:

Bash
pip install streamlit pandas requests
4. Executar a Aplicação
Com o Ollama aberto em segundo plano, execute o comando:

Bash
streamlit run src/app.py
Após o comando, o Streamlit abrirá automaticamente uma aba no seu navegador (geralmente em http://localhost:8501) com a interface do FINFAST.

💡 Principais Funcionalidades Demonstradas
Análise Proativa: O agente identifica se você está misturando contas pessoais e profissionais.

Filtro de Investimentos: Recomendações baseadas estritamente no catálogo produtos_financeiros.json.

Segurança (Anti-alucinação): O agente se recusa a responder sobre temas fora do escopo financeiro ou produtos inexistentes na base.
