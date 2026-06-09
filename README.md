# 🤖 Agent Recruiter (Recoloca-IA)

Um sistema estruturado de **Engenharia de Prompt** e **Conceitos Multi-Agente** projetado para automatizar, filtrar e otimizar a prospecção de oportunidades de carreira no setor de tecnologia. 

O projeto utiliza o potencial de LLMs integradas ao editor de código e ferramentas de extração de dados para criar assistentes especializados em busca de vagas, análise de fit cultural e preparação técnica.

---

## 📌 Sobre o Projeto

Em vez de depender de buscas manuais e repetitivas em plataformas de emprego, o **Agent Recruiter** organiza o conhecimento em módulos separados (Personas e Competências). Isso permite que a Inteligência Artificial atue de forma autônoma e especializada, interpretando o mercado de trabalho através de critérios rigorosos de validação de dados e qualidade.

## 🏗️ Arquitetura e Engenharia de Prompt

O ecossistema foi desenhado seguindo o padrão de separação de responsabilidades, dividindo o escopo em agentes especialistas:

* **Scout Agent:** Responsável pela varredura inicial, mapeamento e extração de dados brutos de vagas utilizando inteligência de rastreamento.
* **Recoloca-IA Agent:** Focado em análise de perfil, refinamento de correspondência (match de competências) e estratégias de abordagem para recrutadores.

---

## 📁 Estrutura do Repositório

A organização dos arquivos reflete a arquitetura do sistema de prompts, facilitando a manutenção e a escalabilidade do contexto:

```text
├── .firecrawl/          # Configurações de extração de dados e web scraping
├── data/                # Dados históricos, registros de vagas coletadas e logs
├── personas/            # Definição de identidade, tom de voz e escopo dos agentes (AGENTS.md)
├── skills/              # Módulos de competências técnicas e regras de negócio injetadas na IA
├── plano.md             # Planejamento estratégico do projeto
└── README.md            # Documentação principal do ecossistema
