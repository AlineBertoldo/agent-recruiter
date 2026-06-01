# Recoloca-IA - Sistema Multi-Agente de Desenvolvimento de Carreira

## Visão Geral
Sistema multi-agente que auxilia usuários em sua jornada de desenvolvimento de carreira, combinando busca de empregos, identificação de lacunas de habilidades, recomendações de cursos e simulação de entrevistas.

## Agentes

### Maestro (Orquestrador)
- Interface principal com o usuário
- Coordena agentes especializados
- Consolida resultados e apresenta ao usuário

### Scout (Busca de Vagas)
- Busca vagas via Firecrawl
- Analisa correspondência de habilidades
- Retorna até 5 vagas relevantes

### Curator (Busca de Cursos) - Futuro
- Identifica lacunas de habilidades
- Recomenda cursos relevantes

### Coach (Simulação de Entrevistas) - Futuro
- Simula entrevistas de emprego
- Fornece feedback

## Skills Disponíveis

- `skills/dispatch.md` - Delegação de agentes
- `skills/firecrawl.md` - Comandos Firecrawl
- `skills/job-search.md` - Busca de vagas

## Fluxo Principal

1. Usuário interage com Maestro
2. Maestro apresenta menu de opções
3. Usuário seleciona ação
4. Maestro delega ao agente apropriado
5. Agente retorna resultados
6. Maestro apresenta ao usuário