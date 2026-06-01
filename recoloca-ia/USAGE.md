# Como Usar o Recoloca-IA

## Início Rápido

1. Configure a variável de ambiente `FIRECRAWL_API_KEY`
2. Interaja com o Maestro carregando `personas/maestro.md`
3. Siga as instruções do playbook

## Configuração do Firecrawl

```bash
# Windows (PowerShell)
$env:FIRECRAWL_API_KEY="sua_api_key_aqui"

# Linux/Mac
export FIRECRAWL_API_KEY="sua_api_key_aqui"
```

## Interagindo com o Maestro

Para usar o sistema, você deve:

1. Carregar a persona do Maestro: `personas/maestro.md`
2. O Maestro seguirá o playbook automaticamente:
   - Saudação inicial
   - Verificação do quiz
   - Apresentação das perguntas (se necessário)
   - Menu de opções
   - Delegação ao Scout (quando selecionado)

## Estrutura de Arquivos

### Para o Maestro
- `personas/maestro.md` - Persona do orquestrador
- `skills/dispatch.md` - Habilidade de delegar tarefas

### Para o Scout
- `personas/scout.md` - Persona do buscador
- `skills/job-search.md` - Capacidades de busca
- `skills/firecrawl.md` - Comandos Firecrawl

### Dados
- `data/user-profile.md` - Perfil do usuário (preencha após o quiz)
- `data/job-search-results.md` - Resultados das buscas
- `data/personality-quiz.md` - Perguntas do quiz

## Fluxo de Trabalho

```
Usuário → Maestro → Scout → Resultados → Usuário
```

O Maestro é o único ponto de entrada. Ele delega automaticamente ao Scout quando você seleciona a opção de busca de vagas.