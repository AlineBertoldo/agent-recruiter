# Course Search – Busca de Cursos na Alura

## Ferramenta
- `terminal` – executar os comandos CLI do **Firecrawl** (`firecrawl search` e `firecrawl scrape`).

## Entrada
- **habilidades_faltantes**: lista de strings extraída de `data/job-search-results.md` (campo `habilidades_faltantes`).
- **perfil_usuario**: conteúdo de `data/user-profile.md` (nível de experiência, localização, etc.).

## Busca Inicial
Para cada habilidade `h` na lista de habilidades faltantes:
```bash
firecrawl search "curso $h alura" --json
```
- O comando devolve um JSON com `url`, `title` e `description` para cada resultado.
- Limitar a **3** resultados por habilidade para evitar sobrecarga.

## Extração Detalhada
Para cada URL retornada, executar:
```bash
firecrawl scrape <url> --format markdown
```
- Se a extração falhar, usar apenas `title` e `description` retornados pela busca e registrar a falha em `erros`.

## Processamento
1. **Normalização**: converter título e descrição para minúsculas e remover pontuação.
2. **Detecção de habilidade**: confirmar que a descrição contém a habilidade buscada (match case‑insensitive).
3. **Detecção de nível**:
   - Procurar palavras‑chave no título ou no markdown: `iniciante`, `intermediário`, `avançado`.
   - Caso não encontre, inferir a partir da URL (ex.: `/curso/` → iniciante, `/especializacao/` → avançado) ou usar o nível padrão do usuário.
4. **Compatibilidade de nível**:
   - Usuário Júnior → preferir `iniciante`.
   - Usuário Pleno → preferir `intermediário`.
   - Usuário Sênior → preferir `avançado`.
   - Se não houver curso no nível exato, aceitar o nível mais próximo e anotar a diferença.
5. **Construção da recomendação**:
   ```json
   {
     "curso": "<título>",
     "link": "<url>",
     "nivel": "iniciante|intermediário|avançado",
     "habilidade_alvo": "<habilidade>",
     "descricao_resumida": "<primeiras 2‑3 linhas do markdown ou description>"
   }
   ```

## Saída
- Até **5** recomendações de cursos (máximo 1 por habilidade, completando com as melhores opções caso haja mais de 5 habilidades).
- Formato de lista numerada (texto simples, não markdown table):
```
1. curso: <título>
   link: <url>
   nivel: <iniciante|intermediário|avançado>
   habilidade_alvo: <habilidade>
   descricao_resumida: <texto curto>

2. ...
```
- Se nenhuma recomendação for encontrada para uma habilidade, registrar em `erros` a mensagem `"Nenhum curso encontrado para <habilidade>"`.

## Tratamento de Erros
- **Falha no `firecrawl search`** para uma habilidade → registrar erro em `erros` e continuar com as demais habilidades.
- **Falha no `firecrawl scrape`** de uma URL → usar `title`/`description` da busca e anotar a falha.
- **Erro inesperado** → registrar a mensagem e prosseguir.
- O agente deve sempre retornar um envelope JSON com os campos `estado`, `resumo`, `dados` (lista de recomendações) e `erros` (lista de mensagens).
