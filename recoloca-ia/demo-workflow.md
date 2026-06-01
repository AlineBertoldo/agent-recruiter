# Demonstração do Fluxo de Trabalho

## Exemplo de Interação

### 1. Usuário inicia conversa com Maestro

**Maestro**: Olá! Bem-vindo ao Recoloca-IA, seu assistente de desenvolvimento de carreira. Vou ajudar você a encontrar vagas de QA e testes de software, identificar lacunas de habilidades e preparar você para entrevistas.

### 2. Verificação do Quiz

**Maestro**: Verifiquei que você ainda não respondeu nosso quiz de habilidades. Para personalizar melhor sua experiência, preciso conhecer seu perfil.

### 3. Perguntas do Quiz

**Maestro**: Por favor, responda estas 6 perguntas:

1. Qual é a sua experiência em QA (em anos)?
2. Em quais tipos de testes você tem mais experiência? (ex.: unitário, integração, aceitação, performance)
3. Prefere trabalhar em empresas de grande porte ou startups?
4. Qual é a sua expectativa salarial mensal (em reais)?
5. Está aberto a oportunidades remotas ou prefere trabalho presencial?
6. Quais tecnologias ou ferramentas de teste você domina? (ex.: Selenium, Cypress, JUnit, TestNG, Playwright)

### 4. Menu de Opções

**Maestro**: Agora escolha uma opção:
- a) Responder o quiz
- b) Buscar vagas de emprego

### 5. Usuário seleciona "b"

**Maestro**: Ótimo! Vou buscar vagas de QA e testes de software para você.

### 6. Envelope de Despacho (interno)

O Maestro constrói este envelope e envia ao Scout via `spawn_agent`:

```
## DESPACHO: SCOUT
### referencia_persona
# Scout - Agente de Busca de Vagas
...

### tarefa
Buscar vagas de emprego para QA e testes de software

### perfil_usuario
# Perfil do Usuário
...

### contexto
Area: QA e Testes de Software
Localizacao: Brasil
Nivel: Não especificado
Habilidades: [a ser preenchido após quiz]

### saida_esperada
Envelope de resposta com estado, resumo, dados (lista de vagas) e erros se houver
```

### 7. Scout executa busca

**Scout** executa: `firecrawl search "vagas QA testes de software Brasil" --json`

### 8. Scout retorna resultados

```
estado: sucesso
resumo: Encontrei 5 vagas de QA com correspondência de habilidades
dados:
  1. titulo: QA Engineer
     empresa: Tech Solutions
     localizacao: São Paulo - Remoto
     link: https://exemplo.com/vaga1
     habilidades_correspondentes: Selenium, Testes Automatizados
     habilidades_faltantes: Cypress, API Testing
     contagem_correspondencia: 2 de 4 habilidades correspondem
```

### 9. Maestro apresenta resultados

**Maestro**: Encontrei 5 vagas para você! Aqui estão as melhores oportunidades:

1. titulo: QA Engineer
   empresa: Tech Solutions
   localizacao: São Paulo - Remoto
   link: https://exemplo.com/vaga1
   habilidades_correspondentes: Selenium, Testes Automatizados
   habilidades_faltantes: Cypress, API Testing
   contagem_correspondencia: 2 de 4 habilidades correspondem

### 10. Retorno ao Menu

**Maestro**: Deseja fazer outra busca ou responder o quiz?