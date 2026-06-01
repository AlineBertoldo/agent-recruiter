# Plano Aula 03 – Implementação do Agente Curator

Continuando a sequência estabelecida nos documentos **plano.md** e **plano-aula02.md**, nesta aula vamos criar o terceiro agente do sistema: **Curator**. O Curator será responsável por buscar cursos na plataforma **alura.com.br** que preencham as lacunas de habilidades identificadas pelo agente **Scout** a partir das vagas encontradas e do quiz do usuário.

---

## 1. Objetivo da Aula
- Implementar o agente **Curator** que navega em *alura.com.br* usando a ferramenta **Firecrawl** (mesmo padrão adotado pelo Scout).
- Analisar as habilidades faltantes (listadas em `data/job-search-results.md`) e buscar cursos que as cubram.
- Persistir os resultados em `data/course-recommendations.md`.
- Integrar o fluxo ao Maestro, permitindo que o usuário selecione a opção **B** no menu para disparar o Curator.

---

## 2. Estrutura de Diretórios Atualizada
```
agent-recruiter/
├─ personas/
│   ├─ scout.md               # já existente
│   └─ curator.md             # **NOVO** – descrição da persona Curator
├─ skills/
│   ├─ firecrawl.md           # já existente
│   ├─ job-search.md          # já existente (Scout)
│   ├─ course-search.md       # **NOVO** – lógica de busca de cursos na Alura
│   └─ dispatch.md            # já existente
└─ data/
    ├─ user-profile.md        # já existente
    ├─ job-search-results.md  # já existente (resultado do Scout)
    └─ course-recommendations.md # **NOVO** – armazenamento das recomendações de cursos
```

---

## 3. Skill **course-search.md**
### 3.1 Ferramenta
- `terminal` – executar os comandos CLI do **Firecrawl** para buscar e raspar páginas da Alura.

### 3.2 Entrada
- Lista de habilidades faltantes extraída de `data/job-search-results.md` (campo `habilidades_faltantes`).
- Preferências de nível de experiência e localização já presentes no perfil do usuário (`data/user-profile.md`).

### 3.3 Busca inicial
Para cada habilidade **h** da lista:
```bash
firecrawl search "curso $h alura" --json
```
- O comando retorna um JSON contendo `url`, `title` e `description` para cada resultado.
- Limitar a **3** resultados por habilidade para evitar sobrecarga.

### 3.4 Extração detalhada
Para cada URL retornada:
```bash
firecrawl scrape <url> --format markdown
```
- Caso a extração falhe, usar apenas `title` e `description` do resultado da busca e registrar a falha.

### 3.5 Processamento
1. **Normalizar** o título e a descrição do curso (lower‑case, remoção de pontuação).
2. Verificar se a descrição contém a habilidade buscada (match case‑insensitive).
3. Determinar o **nível** do curso (iniciante, intermediário, avançado) a partir de palavras‑chave no título ou na página (`"iniciante"`, `"intermediário"`, `"avançado"`).
4. Priorizar cursos cujo nível seja compatível com o nível do usuário (Júnior → iniciante, Pleno → intermediário, Sênior → avançado). Se não houver correspondência exata, aceitar o nível mais próximo e anotar a diferença.
5. Construir um objeto de recomendação contendo:
   - `curso`: título do curso
   - `link`: URL
   - `nivel`: nível detectado
   - `habilidade_alvo`: habilidade que o curso cobre
   - `descricao_resumida`: primeiras 2‑3 linhas do markdown extraído (ou do `description` da busca)

### 3.6 Saída
- Até **5** recomendações de cursos (máximo 1 por habilidade, completando com as melhores opções caso haja mais de 5 habilidades).
- Formato de lista numerada:
```
1. curso: <título>
   link: <url>
   nivel: <iniciante|intermediário|avançado>
   habilidade_alvo: <habilidade>
   descricao_resumida: <texto curto>

2. ...
```
- Caso nenhuma recomendação seja encontrada para uma habilidade, registrar no campo `erros` a mensagem "Nenhum curso encontrado para <habilidade>".

### 3.7 Tratamento de Erros
- Falha no `firecrawl search` → registrar erro em `erros` e abortar a iteração da habilidade.
- Falha no `firecrawl scrape` de uma URL → usar dados da busca e anotar a falha.
- Qualquer outro erro inesperado → registrar e continuar com as demais habilidades.

---

## 4. Persona **personas/curator.md**
- **Papel**: Busca de cursos na Alura que supram as lacunas de habilidades identificadas pelo Scout.
- **Ferramentas Disponíveis**: `terminal` (CLI Firecrawl) + fallback `curl`/`wget` (caso Firecrawl falhe de forma recorrente).
- **Referências Obrigatórias**:
  - `skills/course-search.md`
  - `skills/firecrawl.md`
- **Response Envelope** esperado pelo Maestro (estado, resumo, dados, erros).
- **Regras de Erro**:
  - Se a busca falhar para todas as habilidades, retornar envelope com `erros` e `estado: erro`.
  - Se apenas algumas habilidades falharem, incluir recomendações válidas e listar as falhas em `erros`.
  - Nunca gerar cursos fictícios; usar apenas resultados reais retornados pelo Firecrawl ou, em fallback, pelos HTMLs obtidos via `curl`.

---

## 5. Integração no Maestro (spawn_agent) – Opção **B**
1. **Construção do Envelope de Despacho** quando o usuário escolher a opção **B**:
   - Incluir conteúdo completo de `personas/curator.md`.
   - Incluir parâmetros de busca: lista de `habilidades_faltantes` extraída de `data/job-search-results.md`.
   - Incluir perfil do usuário (`data/user-profile.md`).
2. **Chamada**:
```json
spawn_agent({
  "label": "Curator – Busca de Cursos",
  "message": "<envelope de despacho>"
})
```
3. **Recepção da Resposta** do Curator:
   - Salvar em `data/course-recommendations.md` seguindo o esquema abaixo.
   - Exibir ao usuário a lista formatada (máximo 5 cursos).
   - Retornar ao menu principal.
4. **Fluxo de Erro**:
   - Se `spawn_agent` falhar, apresentar mensagem de erro ao usuário e voltar ao menu.
   - Se o Curator retornar `estado: erro`, exibir os detalhes e permitir que o usuário tente novamente ou volte ao menu.

---

## 6. Esquema do Arquivo **data/course-recommendations.md**
```
Data da Busca: [AAAA-MM-DD HH:MM]
Habilidades analisadas:
  - <habilidade 1>
  - <habilidade 2>
  ...

Recomendações de Cursos:
1. curso: <título>
   link: <url>
   nivel: <iniciante|intermediário|avançado>
   habilidade_alvo: <habilidade>
   descricao_resumida: <texto curto>

2. ...

Erros:
- Nenhum curso encontrado para <habilidade X>
- Falha ao raspar <url Y>: <mensagem de erro>
```

---

## 7. Testes da Aula
| Cenário | Passos | Resultado Esperado |
|---|---|---|
| **Cenário Feliz** | Usuário escolhe **B** → Curator executa buscas → 5 cursos são retornados com descrição resumida e nível compatível. | Lista exibida, arquivo `course-recommendations.md` preenchido. |
| **Fallback Firecrawl** | Firecrawl falha em algumas URLs → Curator usa `curl` para obter HTML bruto e extrai título/descrição. | Cursos ainda são listados, falhas anotadas em `Erros`. |
| **Nenhum Curso Encontrado** | Nenhum resultado para uma habilidade específica. | Mensagem de erro para a habilidade, mas recomendações válidas para as demais são apresentadas. |
| **Erro Total** | `firecrawl search` falha para todas as habilidades. | Envelope retornado com `estado: erro` e lista de erros; Maestro volta ao menu. |

---

## 8. Próximos Passos (Aula 04)
- Implementar a skill **interview-simulation.md** (agente **Coach**) que usa as vagas selecionadas e os cursos recomendados para gerar perguntas de entrevista.
- Criar um fluxo de feedback onde o usuário pode marcar cursos concluídos e atualizar seu perfil de habilidades.
- Automatizar testes de integração entre Scout, Curator e Coach.

---

**Conclusão**
Com este plano, o agente **Curator** será integrado ao fluxo multi‑agente, completando o ciclo de identificação de lacunas e recomendação de aprendizado. Ao final da aula, o usuário terá acesso a cursos relevantes da Alura que o ajudarão a se qualificar para as vagas encontradas pelo Scout.
