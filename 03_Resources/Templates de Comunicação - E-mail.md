---
title: Templates de Comunicação - E-mail
date-created: 2026-01-22
type: template
tags: [comunicacao, email, templates]
---

# Templates de Comunicação - E-mail

Templates para documentação formal e consolidação de incidentes críticos via e-mail.

---

## 📧 Template para Incidente Crítico

Use para documentação formal após resolução de incidentes críticos.

### Estrutura

**Assunto:** `[CRÍTICO] #ID - Título do Problema - STATUS`

---

```
Prezado [Nome],

Segue relatório consolidado do incidente ocorrido:

━━━━━━━━━━━━━━━━━━━━━━━━━━
RESUMO EXECUTIVO
━━━━━━━━━━━━━━━━━━━━━━━━━━

Incidente: [Descrição curta]
Impacto: [O que foi afetado e quantos usuários/processos]
Duração: [Início às XX:XX até XX:XX - Total: X horas]
Status: RESOLVIDO

━━━━━━━━━━━━━━━━━━━━━━━━━━
LINHA DO TEMPO
━━━━━━━━━━━━━━━━━━━━━━━━━━

[HH:MM] - Incidente detectado/reportado
[HH:MM] - Equipe iniciou investigação
[HH:MM] - Causa raiz identificada
[HH:MM] - Correção aplicada
[HH:MM] - Validação e confirmação
[HH:MM] - Sistema normalizado

━━━━━━━━━━━━━━━━━━━━━━━━━━
ANÁLISE TÉCNICA
━━━━━━━━━━━━━━━━━━━━━━━━━━

Causa Raiz:
[Explicação técnica mas compreensível]

Solução Aplicada:
[O que foi feito para resolver]

━━━━━━━━━━━━━━━━━━━━━━━━━━
AÇÕES PREVENTIVAS
━━━━━━━━━━━━━━━━━━━━━━━━━━

Para evitar recorrência, implementaremos:
1. [Ação preventiva 1] - Prazo: [data]
2. [Ação preventiva 2] - Prazo: [data]

━━━━━━━━━━━━━━━━━━━━━━━━━━

Responsável técnico: [Nome]
Estamos à disposição para esclarecimentos.

Atenciosamente,
[Sua equipe]
```

---

## 📋 Exemplo Completo

### Assunto
`[CRÍTICO] #2024-0849 - Indisponibilidade da Aplicação - RESOLVIDO`

### Corpo do E-mail

```
Prezado Fernando,

Segue relatório consolidado do incidente ocorrido em 22 de janeiro de 2026:

━━━━━━━━━━━━━━━━━━━━━━━━━━
RESUMO EXECUTIVO
━━━━━━━━━━━━━━━━━━━━━━━━━━

Incidente: Falha de conectividade com banco de dados após atualização de segurança
Impacto: 150 usuários sem acesso à aplicação principal
Duração: 14:30 até 16:15 - Total: 1h45min
Status: RESOLVIDO

━━━━━━━━━━━━━━━━━━━━━━━━━━
LINHA DO TEMPO
━━━━━━━━━━━━━━━━━━━━━━━━━━

14:30 - Incidente detectado automaticamente por monitoramento
14:35 - Equipe iniciou investigação
14:50 - Causa raiz identificada: incompatibilidade do driver de BD
15:20 - Rollback da atualização de segurança aplicado
15:45 - Validação de conectividade realizada com sucesso
16:15 - Sistema normalizado e confirmado em produção

━━━━━━━━━━━━━━━━━━━━━━━━━━
ANÁLISE TÉCNICA
━━━━━━━━━━━━━━━━━━━━━━━━━━

Causa Raiz:
A atualização de segurança aplicada em 22/01 às 14:00 incluía uma nova versão
do driver de conexão PostgreSQL que tinha incompatibilidade com a versão do BD
em produção (v11.x). O driver novo esperava v13.x ou superior.

Solução Aplicada:
Realizamos rollback da atualização de segurança para a versão anterior (que é
compatível). Em paralelo, iniciamos planejamento para atualizar a instância do
banco de dados para versão compatível com o novo driver.

━━━━━━━━━━━━━━━━━━━━━━━━━━
AÇÕES PREVENTIVAS
━━━━━━━━━━━━━━━━━━━━━━━━━━

Para evitar recorrência, implementaremos:
1. Teste de compatibilidade em ambiente staging antes de aplicar atualizações - Prazo: 29/01
2. Atualização do PostgreSQL para versão compatível - Prazo: 05/02
3. Automação de testes de conectividade pós-deployment - Prazo: 12/02

━━━━━━━━━━━━━━━━━━━━━━━━━━

Responsável técnico: João Silva (Infraestrutura)
Estamos à disposição para esclarecimentos adicionais.

Atenciosamente,
Equipe de Infraestrutura
```

---

## 📝 Dicas para E-mail

- ✅ Use sempre para documentação de críticos
- ✅ Envie em até 24h após resolução
- ✅ Seja técnico mas compreensível
- ✅ Sempre inclua ações preventivas
- ✅ Use formatação clara com separadores
- ✅ Assunto deve incluir ID, título e status
- ✅ Sempre mencione responsável técnico

---

## 🔗 Links Relacionados

- [[Templates de Comunicação - WhatsApp]] - Para atualizações ágeis
- [[Templates de Comunicação - Call Semanal]] - Para report semanal
- [[Processo de Escalação]] - Quando usar cada canal
