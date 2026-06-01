# Firecrawl - Comandos e Regras do CLI

## Visão Geral
Firecrawl é uma ferramenta de scraping e busca que agrega resultados de múltiplas fontes de empregos (Indeed, Catho, LinkedIn, Glassdoor, Infojobs).

## Comandos Disponíveis

### Busca
```
firecrawl search "termo de busca" --json
```
- Retorna resultados em formato JSON
- Campos retornados: url, titulo, descricao, cargo

### Scraper
```
firecrawl scrape <url> --format markdown
```
- Extrai conteúdo de uma URL específica
- Retorna markdown limpo com descrição e requisitos

## Regras de Uso

1. **Sempre usar `--json`** na busca para facilitar o parsing
2. **Sempre usar `--format markdown`** no scrape para obter texto limpo
3. **Tratar erros**: se o comando falhar, capturar a mensagem de erro exata
4. **Timeout**: comandos podem expirar; usar fallback quando necessário
5. **Rate limiting**: aguardar entre requisições se necessário

## Variáveis de Ambiente
- `FIRECRAWL_API_KEY` - deve estar definida no ambiente