---
title: Processo de Escalação
date-created: 2026-01-22T00:00:00.000Z
type: process
tags:
  - comunicacao
  - escalacao
  - fluxo
---
# Processo de Escalação

Guia de quando usar cada canal de comunicação e frequência de atualizações por nível de criticidade.

---

## 📱 Quando Usar Cada Canal

### WhatsApp
**Para:** Comunicação ágil, atualizações rápidas, confirmações

**Quando:**
- ✅ Primeira comunicação com cliente/stakeholder
- ✅ Atualizações de status (críticos e médios)
- ✅ Confirmações de resolução
- ✅ Alertas de urgência
- ✅ Workarounds temporários

**Vantagens:**
- Feedback imediato
- Comunicação em tempo real
- Confirmação de recebimento

---

### E-mail
**Para:** Documentação formal de críticos, relatórios consolidados, registro

**Quando:**
- ✅ Documentação pós-resolução de incidente crítico
- ✅ Relatórios técnicos detalhados
- ✅ Comunicação com múltiplos stakeholders
- ✅ Informações que precisam ficar documentadas
- ✅ Ações preventivas e planos

**Vantagens:**
- Registro formal e rastreável
- Documentação completa
- Alcance múltiplos destinatários
- Permite incluir análises técnicas detalhadas

---

### Call Semanal
**Para:** Análise de tendências, discussão de preventivas, alinhamento estratégico

**Quando:**
- ✅ Report consolidado da semana
- ✅ Discussão de padrões e tendências
- ✅ Alinhamento de ações preventivas
- ✅ Planejamento estratégico
- ✅ Decisões sobre prioridades

**Vantagens:**
- Discussão bidirecional
- Alinhamento de expectativas
- Oportunidade para questões complexas
- Construção de relacionamento

---

## ⏱️ Frequência de Updates por Criticidade

### 🔴 CRÍTICO
**Update via WhatsApp:** A cada hora até resolução

**Cronograma:**
- Primeiro contato: <15 minutos
- Updates: A cada 1 hora
- Resolução: WhatsApp imediato
- E-mail consolidado: Dentro de 24h

**Exemplo:**
```
14:30 - Detecção do problema
14:45 - Primeiro contato WhatsApp
15:45 - Update de andamento WhatsApp
16:45 - Update de andamento WhatsApp
17:15 - Resolução confirmada WhatsApp
18/01 - E-mail com relatório consolidado
```

---

### 🟡 MÉDIO
**Update via WhatsApp:** Diário se não resolver em 24h

**Cronograma:**
- Primeiro contato: <1 hora
- Atualizações: Diárias se em andamento
- Resolução: WhatsApp
- Documentação: Nota interna (sem e-mail)

**Exemplo:**
```
09:00 - Recebimento do chamado
10:00 - Primeiro contato WhatsApp
17:00 - End of day update (se ainda em andamento)
Próximo dia 09:00 - Status update diário
```

---

### 🟢 BAIXO
**Update via WhatsApp:** Apenas na resolução

**Cronograma:**
- Primeiro contato: <4 horas
- Atualizações: Nenhuma até resolução
- Resolução: WhatsApp
- Documentação: Apenas para base de conhecimento

**Exemplo:**
```
14:00 - Recebimento do chamado
14:30 - Análise e resolução
14:45 - Confirmação de resolução WhatsApp
```

---

## 📋 Matriz de Decisão Rápida

| Situação | Canal | Urgência | Update |
| --- | --- | --- | --- |
| Problema resolvido em <30min | WhatsApp | Baixa | Confirmação final |
| Problema em análise | WhatsApp | Média/Alta | Cada hora ou diária |
| Pós-resolução crítico | E-mail | Alta | Dentro de 24h |
| Report da semana | E-mail/Call | Baixa | Semanal |
| Escalação urgente | WhatsApp | Crítica | Imediato |

---

## 🔄 Fluxo de Escalação

```
CHAMADO RECEBIDO
      ↓
┌─────────────────┐
│ Classificar?    │
├─ Crítico (🔴)   │
├─ Médio (🟡)     │
└─ Baixo (🟢)     │
      ↓
┌──────────────────┐
│ 1º Contato       │
│ via WhatsApp     │
│ <15min crítico   │
│ <1h médio        │
│ <4h baixo        │
└──────────────────┘
      ↓
     [ANÁLISE]
      ↓
┌──────────────────────────┐
│ Crítico? → Updates cada hora
│ Médio?   → Updates diários
│ Baixo?   → Sem updates
└──────────────────────────┘
      ↓
    [RESOLUÇÃO]
      ↓
┌──────────────────────────┐
│ WhatsApp: Confirmação    │
│ Crítico?  → E-mail em 24h│
│ Médio?    → Documentado  │
│ Baixo?    → Base de know │
└──────────────────────────┘
```

---

## 📝 Checklist de Escalação

- [ ] Criticidade classificada?
- [ ] ID único gerado?
- [ ] Responsável designado?
- [ ] Primeiro contato enviado no tempo?
- [ ] Updates sendo enviados conforme criticidade?
- [ ] Documentação sendo atualizada?
- [ ] Pós-resolução: confirmação + documentação apropriada?

---

## 🔗 Links Relacionados

- [[Templates de Comunicação - WhatsApp]] - Templates para WhatsApp
- [[Templates de Comunicação - E-mail]] - Templates para E-mail
- [[Templates de Comunicação - Call Semanal]] - Template para report
- [[Checklist de Atendimento]] - Checklist rápido para equipe
