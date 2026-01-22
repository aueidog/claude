---
title: "Incidente #2026-0002 - Mensagem Oi Indevida"
date-created: 2026-01-22T00:00:00.000Z
type: incident
status: resolvido
criticidade: critico
tags:
  - incidente
  - critico
  - resolvido
  - mensagem
  - bot
---
# 🔴 Incidente #2026-0002 - Mensagem Oi Indevida

## Resumo

| Campo | Valor |
| --- | --- |
| **ID** | #2026-0002 |
| **Criticidade** | 🔴 Crítico |
| **Status** | ✅ Resolvido |
| **Data Identificação** | 2026-01-22 |
| **Data Resolução** | 2026-01-22 |

## Descrição do Problema

Clientes que finalizavam a conversa estavam sendo redirecionados para o início do bot com uma mensagem de "oi" enviada indevidamente.

## Impacto

- **Volume**: ~500 casos por dia
- **Efeito**: Experiência confusa para o usuário ao receber mensagem não solicitada após encerrar conversa

## Causa Raiz

O fluxo estava direcionando os usuários para o bloco errado após finalização da conversa.

## Solução Aplicada

1. **Identificação**: Mapeado o bloco incorreto que estava sendo acionado
2. **Correção**: Alterado o redirecionamento para o bloco de logout correto
3. **Resultado**: Usuários agora são enviados para o bloco de logout sem envio de mensagem

## Ações Preventivas

- [ ] Revisar outros fluxos de finalização de conversa
- [ ] Adicionar testes automatizados para fluxos de logout

## Comunicação

- [ ] WhatsApp enviado
- [ ] E-mail consolidado enviado (24h)

## Links Relacionados

- [[Gestão de Incidentes e Comunicação]]
