---
title: Checklist de Atendimento
date-created: 2026-01-22
type: checklist
tags: [comunicacao, checklist, atendimento]
---

# Checklist de Atendimento

Guia rápido para a equipe seguir ao receber e processar chamados de incidentes.

---

## ✅ AO RECEBER UM CHAMADO

- [ ] **Classificar criticidade**
  - [ ] 🔴 Crítico (sistema fora do ar, perda de dados)
  - [ ] 🟡 Médio (funcionalidade comprometida)
  - [ ] 🟢 Baixo (melhorias, dúvidas, problemas menores)

- [ ] **Gerar ID único do chamado**
  - [ ] Formato: `#YYYY-NNNN` (ex: #2024-0847)
  - [ ] Documentar em log centralizado

- [ ] **Enviar primeiro contato**
  - [ ] Via WhatsApp (sempre)
  - [ ] Crítico: dentro de 15 minutos
  - [ ] Médio: dentro de 1 hora
  - [ ] Baixo: dentro de 4 horas
  - [ ] Incluir ID do chamado
  - [ ] Agradecer pela comunicação

- [ ] **Designar responsável técnico**
  - [ ] Escolher especialista adequado
  - [ ] Notificar via WhatsApp ou e-mail
  - [ ] Incluir no primeiro contato com cliente

---

## ⚙️ DURANTE RESOLUÇÃO (Críticos/Médios)

- [ ] **Manter updates conforme criticidade**
  - [ ] 🔴 Crítico: a cada hora
  - [ ] 🟡 Médio: diário se >24h
  - [ ] Via WhatsApp

- [ ] **Documentar ações tomadas**
  - [ ] O que foi feito
  - [ ] Quando foi feito
  - [ ] Por quem foi feito
  - [ ] Resultado da ação
  - [ ] Manter em log ou ticket

- [ ] **Comunicar workarounds imediatamente**
  - [ ] Se houver solução temporária
  - [ ] Incluir limitações/riscos
  - [ ] Informar quando solução permanente estará pronta
  - [ ] Via WhatsApp urgente

- [ ] **Escalar se necessário**
  - [ ] Detectar se precisa de especialista externo
  - [ ] Notificar gestor responsável
  - [ ] Manter cliente informado sobre escalação

---

## ✔️ PÓS-RESOLUÇÃO

### Para Todos os Chamados

- [ ] **Enviar confirmação de resolução**
  - [ ] Via WhatsApp
  - [ ] Confirmar que problema foi resolvido
  - [ ] Oferecer suporte se houver dúvidas
  - [ ] Usar template apropriado

- [ ] **Validar solução com cliente (se crítico/médio)**
  - [ ] Solicitar confirmação de que está funcionando
  - [ ] Fazer testes básicos
  - [ ] Documentar validação

### Apenas para Críticos

- [ ] **Elaborar e-mail consolidado em 24h**
  - [ ] Incluir resumo executivo
  - [ ] Linha do tempo completa
  - [ ] Análise técnica
  - [ ] Ações preventivas
  - [ ] Usar template de e-mail

- [ ] **Agendar análise pós-incidente**
  - [ ] Discutir o que deu certo
  - [ ] Discutir o que poderia melhorar
  - [ ] Definir ações preventivas
  - [ ] Atribuir responsáveis

### Para Todos os Tipos

- [ ] **Atualizar base de conhecimento**
  - [ ] Documentar problema
  - [ ] Documentar solução
  - [ ] Incluir passos de reprodução
  - [ ] Publicar em wiki/base interna

- [ ] **Fechar ticket**
  - [ ] Marcar como resolvido
  - [ ] Arquivar documentação
  - [ ] Preparar para análise semanal

---

## 📊 Matriz de Templates

| Criticidade | Primeiro Contato | Durante | Resolução | Pós |
|-------------|------------------|---------|-----------|-----|
| 🔴 Crítico | WhatsApp | WhatsApp 1h | WhatsApp | E-mail + Análise |
| 🟡 Médio | WhatsApp | WhatsApp diário | WhatsApp | Base de conhecimento |
| 🟢 Baixo | WhatsApp | Nenhum | WhatsApp | Base de conhecimento |

---

## 📱 Dicas Importantes

### WhatsApp
- ✅ Sempre inclua o ID do chamado (#YYYY-NNNN)
- ✅ Use emojis para destacar status (✅🔴🟡🟢)
- ✅ Seja objetivo e direto
- ✅ Responda com agilidade
- ✅ Confirme recebimento de mensagens do cliente

### E-mail
- ✅ Use após resolução de críticos
- ✅ Inclua análise técnica
- ✅ Sempre proponha ações preventivas
- ✅ Seja profissional e formal
- ✅ Mantenha registro oficial

### Comunicação Geral
- ✅ Sempre que possível, dê notícias positivas (workarounds, progresso)
- ✅ Não deixe cliente sem notícias
- ✅ Se não souber resposta, diga o que está fazendo
- ✅ Ofereça sempre alternativa/próximos passos
- ✅ Mantenha tom profissional e prestativo

---

## 🔗 Links Relacionados

- [[Processo de Escalação]] - Quando usar cada canal
- [[Templates de Comunicação - WhatsApp]] - Templates para mensagens
- [[Templates de Comunicação - E-mail]] - Templates para e-mail
- [[Gestão de Incidentes e Comunicação]] - Visão geral do processo
