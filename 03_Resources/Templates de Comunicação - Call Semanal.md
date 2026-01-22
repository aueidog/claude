---
title: Templates de Comunicação - Call Semanal
date-created: 2026-01-22
type: template
tags: [comunicacao, call, templates, weekly-report]
---

# Templates de Comunicação - Call Semanal

Template para relatório semanal consolidado, estruturado para apresentação em call com stakeholders.

---

## 📊 Template Principal

Documento a ser compartilhado ANTES da reunião semanal.

```
REPORT SEMANAL - [Data Início] a [Data Fim]

═══════════════════════════════════
📊 RESUMO EXECUTIVO
═══════════════════════════════════

Total de chamados: XX
├─ Críticos: X (resolvidos: X, em andamento: X)
├─ Médios: X (resolvidos: X, em andamento: X)
└─ Baixos: X (resolvidos: X, em andamento: X)

Tempo médio de resolução:
├─ Críticos: X horas
├─ Médios: X horas
└─ Baixos: X minutos

SLA: XX% de conformidade

═══════════════════════════════════
🔴 INCIDENTES CRÍTICOS DA SEMANA
═══════════════════════════════════

#ID - [Título]
├─ Impacto: [descrição]
├─ Duração: [tempo]
├─ Status: [Resolvido/Em andamento]
├─ Causa: [resumo]
└─ Ação preventiva: [o que faremos]

[Repetir para cada crítico]

═══════════════════════════════════
📈 TENDÊNCIAS E OBSERVAÇÕES
═══════════════════════════════════

[Padrões identificados, problemas recorrentes, melhorias sugeridas]

═══════════════════════════════════
🎯 PRÓXIMOS PASSOS
═══════════════════════════════════

1. [Ação pendente 1] - Responsável - Prazo
2. [Ação pendente 2] - Responsável - Prazo
```

---

## 📋 Exemplo Completo

```
REPORT SEMANAL - 15 a 21 de janeiro de 2026

═══════════════════════════════════
📊 RESUMO EXECUTIVO
═══════════════════════════════════

Total de chamados: 47
├─ Críticos: 2 (resolvidos: 2, em andamento: 0)
├─ Médios: 12 (resolvidos: 11, em andamento: 1)
└─ Baixos: 33 (resolvidos: 33, em andamento: 0)

Tempo médio de resolução:
├─ Críticos: 2h30min
├─ Médios: 8h45min
└─ Baixos: 45 minutos

SLA: 98% de conformidade

═══════════════════════════════════
🔴 INCIDENTES CRÍTICOS DA SEMANA
═══════════════════════════════════

#2024-0845 - Perda de dados em backup
├─ Impacto: 5GB de dados perdidos em sistema de backup
├─ Duração: 2h20min (15/01 14:30 - 16:50)
├─ Status: Resolvido
├─ Causa: Falha de script de limpeza que deletou arquivos ativos ao invés de antigos
└─ Ação preventiva: Implementar validação de integridade em scripts de backup (Prazo: 22/01)

#2024-0848 - Sistema de pagamento indisponível
├─ Impacto: 85 transações pendentes, clientes sem acesso
├─ Duração: 1h45min (19/01 11:15 - 13:00)
├─ Status: Resolvido
├─ Causa: Cache expirado sem sincronização com servidor principal
└─ Ação preventiva: Implementar redundância de cache (Prazo: 29/01)

═══════════════════════════════════
📈 TENDÊNCIAS E OBSERVAÇÕES
═══════════════════════════════════

1. **Padrão Identificado**: 60% dos problemas críticos relacionados a infraestrutura
   - Cache e sincronização são pontos fracos
   - Sugestão: Revisar arquitetura de cache

2. **Melhoria de Performance**: Tempo médio de resolução caiu 15% vs semana anterior
   - Novos logs centralizados ajudaram na investigação
   - Equipe está mais familiarizada com novas ferramentas

3. **Taxa de SLA**: Mantemos 98% de conformidade
   - Um incidente médio extrapolou SLA por 2h
   - Causa: Falta de especialista em horário disponível

═══════════════════════════════════
🎯 PRÓXIMOS PASSOS
═══════════════════════════════════

1. Validação de integridade em scripts de backup - João Silva - 22/01
2. Implementar redundância de cache - Maria Santos - 29/01
3. Revisar arquitetura de cache em arquitetura semanal - Tech Lead - 23/01
4. Contratar/escalar especialista para horários críticos - RH - 05/02
```

---

## 📝 Dicas para Call Semanal

- ✅ Compartilhe o documento 24h antes da reunião
- ✅ Use métricas claras (números, percentuais, tempos)
- ✅ Destaque apenas críticos na reunião (médios para discussão se necessário)
- ✅ Sempre conclua com próximos passos e responsáveis
- ✅ Use formatação visual para fácil leitura
- ✅ Identifique padrões e tendências
- ✅ Relate ações preventivas já iniciadas

---

## 📊 Métricas Úteis para Incluir

### Qualitativas
- Padrões de problemas recorrentes
- Tendências de melhoria/piora
- Gaps identificados
- Sugestões de preventivas

### Quantitativas
- Total de chamados por criticidade
- Taxa de resolução por criticidade
- Tempo médio de resolução
- Conformidade com SLA
- Problemas em andamento vs resolvidos

---

## 🔗 Links Relacionados

- [[Templates de Comunicação - WhatsApp]] - Para atualizações ágeis
- [[Templates de Comunicação - E-mail]] - Para documentação formal
- [[Processo de Escalação]] - Quando usar cada canal
- [[Gestão de Incidentes e Comunicação]] - Visão geral do processo
