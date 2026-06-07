# 🤖 Recoloca-IA: Sistema Multi-Agente para Busca de Vagas

> Automação inteligente de busca de oportunidades e análise de aderência de perfil utilizando Inteligência Artificial.

## 📋 Visão Geral
O **Recoloca-IA** é um sistema baseado na arquitetura de múltiplos agentes de IA projetado para otimizar a jornada de busca por emprego. O sistema automatiza a varredura de vagas no mercado, analisa os requisitos das oportunidades encontradas e cruza essas informações com as habilidades do candidato, gerando um dashboard visual e estruturado para facilitar a tomada de decisão.

## 🏗️ Arquitetura do Sistema
O projeto utiliza um modelo de delegação de tarefas (Mixture of Experts), separando responsabilidades em agentes específicos:
* **Maestro:** Orquestrador principal responsável pela interação com o usuário e tomada de decisão sobre qual fluxo iniciar.
* **Scout:** Agente de busca integrado ao Firecrawl para mapear vagas ativas, extrair dados relevantes e tratar links dinâmicos, filtrando ruídos (como páginas de erro 404).
* *(Em breve)* **Curator & Coach:** Agentes focados em análise de lacunas de habilidades e simulação de entrevistas.

