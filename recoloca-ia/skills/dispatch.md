# Dispatch - Habilidade de Delegação de Agentes

## Visão Geral
Esta skill permite ao Maestro delegar tarefas a agentes especializados usando a ferramenta `spawn_agent` do Zed.

## Ferramenta Zed
- `spawn_agent` — despachar agentes com persona e contexto específicos

## Protocolo de Despacho

### Envelope de Despacho
Estrutura padrão para enviar tarefas a agentes:

```
## DESPACHO: [NOME_DO_AGENTE]
### referencia_persona
[Conteúdo completo do arquivo personas/[nome].md]

### tarefa
[Descrição clara da tarefa]

### perfil_usuario
[Conteúdo de data/user-profile.md]

### contexto
[Detalhes específicos para a tarefa]

### saida_esperada
[Formato esperado de resposta]
```

## Regras de Delegação

1. **Sempre incluir a persona completa** no campo `referencia_persona`
2. **Ser específico na tarefa** - detalhar exatamente o que deve ser feito
3. **Passar contexto relevante** - área, localização, habilidades, nível
4. **Definir formato de saída** - deixar claro o formato esperado
5. **Tratar falhas** - se `spawn_agent` falhar, reportar ao usuário

## Agentes Disponíveis

- **Scout** - Busca de vagas de emprego
- **Curator** - Busca de cursos (futuro)
- **Coach** - Simulação de entrevistas (futuro)

## Fluxo de Trabalho
1. Construir envelope de despacho
2. Chamar `spawn_agent` com o prompt
3. Aguardar resposta
4. Processar resposta
5. Salvar resultados em `data/`
6. Apresentar ao usuário