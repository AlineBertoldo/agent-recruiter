# Plano Aula 02 – Implementação do Agente Scout

Baseado no **plano.md**, este documento detalha o planejamento da segunda aula, focada na criação do agente **Scout**, responsável por buscar vagas de emprego utilizando a ferramenta **Firecrawl** (CLI) ou, em caso de falha, a ferramenta de acesso web nativa.

---

## 1. Objetivo da Aula
- Implementar o agente **Scout** que busca vagas em sites como **InfoJobs**, **Vagas.com**, **Indeed** e **Gupy**.
- Utilizar o comando `firecrawl` já instalado no ambiente.
- Garantir fallback para a ferramenta web nativa caso o Firecrawl não funcione.
- Estruturar todo o fluxo dentro da pasta **skills** para que o Maestro possa despachar o agente.

---

## 2. Estrutura de Diretórios a Ser Atualizada
```
agent-recruiter/
├─ personas/
│   └─ scout.md               # (novo) descrição da persona Scout
├─ skills/
│   ├─ firecrawl.md           # (já existente)
│   ├─ job-search.md          # (novo) lógica de busca de vagas
│   └─ dispatch.md            # (já existente)
└─ data/
    ├─ user-profile.md        # (já existente)
    └─ job-search-results.md  # (novo) armazenamento dos resultados
```

---

## 3. Conteúdo da Skill **job-search.md**
1. **Ferramenta**: `terminal` – executar `firecrawl search` e `firecrawl scrape`.
2. **Busca inicial**:
   ```bash
   firecrawl search "vagas <área> <localização>" --json
   ```
   - Retorna JSON com `url`, `title`, `description`.
3. **Extração detalhada** para cada URL retornada:
   ```bash
   firecrawl scrape <url> --format markdown
   ```
   - Caso falhe, usar apenas `title` e `description` da busca.
4. **Processamento**:
   - Ler `data/user-profile.md` para obter habilidades, nível e localização.
   - Extrair habilidades da descrição da vaga (palavras‑chave simples, case‑insensitive).
   - Comparar com as habilidades do usuário, gerar listas **correspondentes** e **faltantes**.
   - Filtrar por nível (Júnior/Pleno/Sênior). Se não houver vagas do nível exato, incluir vagas adjacentes anotando a diferença.
5. **Saída** – até 5 vagas, formato de lista numerada:
   ```text
   1. titulo: ...
      empresa: ...
      localizacao: ...
      link: ...
      habilidades_correspondentes: [...]
      habilidades_faltantes: [...]
      contagem_correspondencia: X de Y
   ```
6. **Tratamento de erros**:
   - Falha no `firecrawl search` → registrar erro em campo `erros` e abortar.
   - Falha no `firecrawl scrape` de uma URL → usar dados da busca e anotar a falha.
   - Qualquer outro erro → reportar e encerrar.

---

## 4. Conteúdo da Persona **personas/scout.md**
- **Papel**: Busca de vagas, filtragem e apresentação ao Maestro.
- **Ferramentas**: `terminal` (CLI Firecrawl) + fallback `curl`/`wget` (web nativo).
- **Referências obrigatórias**:
  - `skills/job-search.md`
  - `skills/firecrawl.md`
- **Response Envelope** esperado pelo Maestro (estado, resumo, dados, erros).
- **Regras de erro** conforme descritas no plano original.

---

## 5. Integração no Maestro (spawn_agent)
1. Quando o usuário escolher a opção **A** no menu, o Maestro deve montar o envelope de despacho:
   - Incluir conteúdo completo de `personas/scout.md`.
   - Incluir parâmetros de busca (área, localização, nível, habilidades) extraídos de `data/user-profile.md`.
2. Chamar `spawn_agent` com o label “Scout – Busca de Vagas” e o prompt contendo o envelope.
3. Receber a resposta do Scout, salvar em `data/job-search-results.md` seguindo o esquema definido no plano.
4. Exibir ao usuário a lista formatada (máximo 5 vagas) e retornar ao menu principal.
5. Em caso de erro, apresentar a mensagem de erro ao usuário e voltar ao menu.

---

## 6. Testes da Aula
- **Cenário feliz**: Firecrawl funciona, 5 vagas são retornadas, habilidades são comparadas e exibidas.
- **Fallback**: Firecrawl falha; o agente usa `curl`/`wget` para obter HTML bruto e ainda entrega vagas (possivelmente menos detalhadas).
- **Erro de extração**: Uma ou mais URLs falham no `scrape`; o agente usa título/descrição da busca e indica a falha.
- **Erro total**: `firecrawl search` falha; o agente reporta erro e volta ao menu.

---

## 7. Próximos Passos (para a aula seguinte)
- Implementar a skill **course-analysis.md** que cruza vagas com cursos recomendados.
- Expandir o fluxo de feedback entre Scout, Curator e Coach.
- Automatizar testes unitários para a skill `job-search.md`.

---

*Este plano deve ser seguido na segunda aula para garantir que o agente Scout esteja pronto para ser despachado pelo Maestro e entregue resultados de busca de vagas de forma robusta e resiliente.*