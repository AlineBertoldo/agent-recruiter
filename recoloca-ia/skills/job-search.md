# Job Search - Capacidades de Busca de Vagas

## Ferramenta Zed
- `terminal` — executar comandos CLI do `firecrawl`

## Fluxo de Busca

### 1. Descoberta de Vagas
Comando:
```
firecrawl search "vagas [area_de_interesse] [localizacao]" --json
```

Retorna JSON com:
- url: link direto para a vaga
- titulo: título da vaga
- descricao: descrição resumida
- cargo: nome do cargo

### 2. Extração de Detalhes
Comando:
```
firecrawl scrape <url> --format markdown
```

Usar em URLs individuais para obter:
- Descrição completa
- Requisitos detalhados
- Habilidades requeridas

**Fallback**: Se scrape falhar, usar título/descrição do resultado da busca.

### 3. Extração de Dados da Vaga
Para cada vaga encontrada, extrair:
1. título: do campo `titulo` ou do scrape
2. empresa: inferir da URL ou título
3. localizacao: do scrape ou campo `descricao`
4. link: do campo `url`
5. habilidades_requeridas: da descrição detalhada

### 4. Correspondência de Habilidades
- Comparar habilidades requeridas com habilidades do usuário
- Usar correspondência sem distinção de maiúsculas/minúsculas
- Contar quantas habilidades correspondem
- Listar habilidades em falta

### 5. Filtragem por Nível
- Identificar nível mencionado na vaga (Júnior, Pleno, Sênior)
- Priorizar vagas que correspondam ao nível do usuário
- Se não houver vagas do nível correspondente, incluir vagas adjacentes anotando a discrepância

### 6. Formatação de Resposta
Retornar até 5 vagas no formato:
```
1. titulo: [título da vaga]
   empresa: [nome da empresa]
   localizacao: [cidade ou Remoto]
   link: [URL]
   habilidades_correspondentes: [habilidade1, habilidade2]
   habilidades_faltantes: [habilidade3, habilidade4]
   contagem_correspondencia: [X de Y habilidades correspondem]
```

## Tratamento de Erros

- Se `firecrawl search` falhar: reportar erro exato e sugerir ampliar termos de busca
- Se `firecrawl scrape` falhar em URL específica: usar dados da busca e anotar falha
- Se nenhum resultado: informar ao usuário e sugerir novos termos

## Limites

- Máximo de 5 vagas por busca
- Timeout de 30 segundos por operação