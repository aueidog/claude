---
title: Templates de Comunicação - WhatsApp
date-created: 2026-01-22
type: resource
tags: [comunicacao, whatsapp, guia]
---

# Guia de Comunicação - WhatsApp

Guia de referência com exemplos práticos para comunicação via WhatsApp.

**Templates puros:** Use os templates em [[06_Metadata/Templates/]] para copiar e usar diretamente.

---

## ✅ Problema Simples (Baixo) - Resolução Imediata

Use quando o problema for resolvido em minutos.

```
✅ [RESOLVIDO] - #ID_CHAMADO

Problema: [descrição breve]
Solução aplicada: [o que foi feito]
Tempo de resolução: [X minutos]

Qualquer dúvida, estamos à disposição!
```

### Exemplo Prático

```
✅ [RESOLVIDO] - #2024-0847

Problema: Reset de senha não funcionando
Solução aplicada: Limpamos cache e resetamos servidor de autenticação
Tempo de resolução: 5 minutos

Qualquer dúvida, estamos à disposição!
```

---

## 🟡 Problema Médio - Atualizações de Andamento

Use para problemas que levam tempo para resolver, com impacto funcional.

```
🟡 [EM ANDAMENTO] - #ID_CHAMADO

Situação: [descrição do problema]
Status atual: [o que está sendo feito]
Previsão: [quando será resolvido]
Próxima atualização: [quando daremos retorno]

Impacto: [o que está afetado]
Workaround: [se houver alternativa temporária]
```

### Exemplo Prático

```
🟡 [EM ANDAMENTO] - #2024-0848

Situação: Relatórios levando mais de 5 minutos para gerar
Status atual: Analisando índices de banco de dados e otimizando queries
Previsão: 2 horas
Próxima atualização: em 1 hora

Impacto: Relatórios mais lentos que o normal
Workaround: Use período menor (semana ao invés de mês) para processar mais rapidamente
```

---

## 🔴 Incidente Crítico - Comunicação Estruturada

Use para problemas que afetam sistema inteiro ou causam perda de dados.

```
🔴 [INCIDENTE CRÍTICO] - #ID_CHAMADO

⏰ Início: [data/hora]
📍 Status: [Identificado/Em análise/Em resolução]

🎯 IMPACTO:
[Descrição clara do que está afetado]

🔍 CAUSA IDENTIFICADA:
[Se já souber, caso contrário: "Em investigação"]

⚙️ AÇÕES EM CURSO:
- [Ação 1]
- [Ação 2]

⏱️ PREVISÃO: [estimativa ou "a definir"]

Responsável: [nome do técnico]
Próximo update: [prazo máximo]
```

### Exemplo Prático

```
🔴 [INCIDENTE CRÍTICO] - #2024-0849

⏰ Início: 14:30 (22/01/2026)
📍 Status: Em resolução

🎯 IMPACTO:
Aplicação indisponível para todos os usuários. Nenhum acesso possível desde 14:30.

🔍 CAUSA IDENTIFICADA:
Falha de conexão com banco de dados principal após atualização de segurança.

⚙️ AÇÕES EM CURSO:
- Rollback da atualização de segurança
- Verificação de logs de erro
- Testes de conectividade

⏱️ PREVISÃO: 16:00 (aproximadamente 1h30)

Responsável: João Silva (Infraestrutura)
Próximo update: 15:30
```

---

## 📝 Dicas para WhatsApp

- ✅ Seja objetivo e direto
- ✅ Use emojis para destacar status
- ✅ Sempre inclua ID do chamado para rastreabilidade
- ✅ Se crítico: update a cada hora até resolver
- ✅ Se médio: update diário se não resolver em 24h
- ✅ Se baixo: comunique apenas na resolução

---

## 🔗 Links Relacionados

- [[Templates de Comunicação - E-mail]] - Para documentação formal
- [[Templates de Comunicação - Call Semanal]] - Para report semanal
- [[Processo de Escalação]] - Quando usar cada canal
