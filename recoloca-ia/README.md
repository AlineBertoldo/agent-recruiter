# Recoloca-IA

Sistema multi-agente para auxiliar na busca de empregos na área de TI, com foco em QA e testes de software.

## Como Usar

1. **Interaja com o Maestro** - O Maestro é o agente principal que você deve usar para interagir com o sistema.

2. **Responda o Quiz** - Ao iniciar, o Maestro verificará se você já respondeu o quiz de habilidades. Se não respondeu, serão feitas 6 perguntas para entender seu perfil.

3. **Selecione uma Opção** - O menu oferece:
   - a) Responder o quiz
   - b) Buscar vagas de emprego

4. **Veja os Resultados** - Ao selecionar a busca de vagas, o Scout buscará oportunidades e mostrará até 5 vagas com análise de habilidades.

## Pré-requisitos

- Firecrawl instalado e configurado (`FIRECRAWL_API_KEY` definida)

## Estrutura

```
recoloca-ia/
├── AGENTS.md           # Visão geral do sistema
├── personas/
│   ├── maestro.md      # Agente orquestrador
│   └── scout.md        # Agente de busca de vagas
├── skills/
│   ├── dispatch.md      # Habilidade de delegação
│   ├── firecrawl.md     # Comandos Firecrawl
│   └── job-search.md    # Capacidades de busca
└── data/
    ├── personality-quiz.md   # Perguntas e respostas
    ├── user-profile.md       # Perfil do usuário
    └── job-search-results.md # Resultados das buscas
```

## Agentes

- **Maestro**: Interface principal, apresenta menu e delega tarefas
- **Scout**: Busca vagas via Firecrawl e analisa correspondência de habilidades

## Funcionalidades Futuras

- Curator: Busca de cursos para lacunas de habilidades
- Coach: Simulação de entrevistas