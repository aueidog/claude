# Bot de Atendimento Alloha - Documentação Completa

## Visão Geral

O **alloha-prd** é um bot de atendimento automatizado da empresa Alloha Fibra (Giga+ Fibra), desenvolvido na plataforma Fintalk. O bot gerencia interações com clientes através de múltiplos canais, oferecendo autoatendimento para diversas demandas.

### Informações do Bot

| Propriedade | Valor |
| --- | --- |
| Nome | alloha-prd |
| Ambiente | Produção |
| Plataforma | Fintalk |
| Integrações | n8n (52 endpoints) |
| Total de Fluxos | 38 grupos |

---

## Arquitetura Geral

```
┌─────────────────────────────────────────────────────────────────────┐
│                         BOT ALLOHA-PRD                               │
├─────────────────────────────────────────────────────────────────────┤
│  ENTRADA                                                             │
│  ┌──────────┐    ┌────────────────┐    ┌──────────────────┐        │
│  │ Principal │───▶│ Fluxo Inicial  │───▶│ Auth (Token)     │        │
│  └──────────┘    └────────────────┘    └──────────────────┘        │
│                          │                      │                   │
│                          ▼                      ▼                   │
│  ┌────────────────────────────────────────────────────────────┐    │
│  │                    MENU PRINCIPAL                           │    │
│  │  • Suporte Técnico    • Segunda Via Fatura                 │    │
│  │  • Mudança de Endereço • Cancelamento                      │    │
│  │  • Alterações Cadastrais • Outros                          │    │
│  └────────────────────────────────────────────────────────────┘    │
│                          │                                          │
│         ┌────────────────┼────────────────┐                        │
│         ▼                ▼                ▼                        │
│  ┌────────────┐   ┌────────────┐   ┌────────────┐                  │
│  │ Suporte    │   │ Financeiro │   │ Cancelam.  │                  │
│  │ Técnico V2 │   │            │   │ TX         │                  │
│  └────────────┘   └────────────┘   └────────────┘                  │
│         │                │                │                        │
│         ▼                ▼                ▼                        │
│  ┌─────────────────────────────────────────────────────────────┐   │
│  │                      ENCERRAMENTO                            │   │
│  │                           │                                  │   │
│  │                      ┌────┴────┐                            │   │
│  │                      │  CSAT   │                            │   │
│  │                      └─────────┘                            │   │
│  └─────────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────────┘
```

---

## Índice de Documentação

1. [01-fluxos-principais.md](./01-fluxos-principais.md)
2. [02-fluxo-financeiro.md](./02-fluxo-financeiro.md)
3. [03-fluxo-suporte-tecnico.md](./03-fluxo-suporte-tecnico.md)
4. [04-fluxo-negociacao.md](./04-fluxo-negociacao.md)
5. [05-apis-integracoes.md](./05-apis-integracoes.md)
6. [06-entidades-variaveis.md](./06-entidades-variaveis.md)
7. [07-filas-atendimento.md](./07-filas-atendimento.md)

---

## Resumo dos Fluxos

### Fluxos de Entrada e Navegação

| Fluxo | Descrição | Blocos |
| --- | --- | --- |
| `principal` | Ponto de entrada do bot | 10 |
| `fluxo-inicial` | Identificação e validação do cliente | 60 |
| `auth` | Autenticação e geração de token | 6 |
| `menu-principal` | Menu de opções principal | 27 |
| `encerramento` | Finalização do atendimento | 8 |
| `csat` | Pesquisa de satisfação | 19 |

### Fluxos Financeiros

| Fluxo | Descrição | Blocos |
| --- | --- | --- |
| `financeiro` | Consulta e gestão de faturas | 77 |
| `financeiro-susp` | Financeiro para clientes suspensos | 36 |
| `financeiro-ex` | Financeiro para ex-clientes | 46 |
| `financeiro-mass` | Financeiro em massiva | 51 |
| `negociacao` | Renegociação de débitos | 54 |
| `contestacao-fin` | Contestação financeira | 14 |

### Fluxos de Suporte Técnico

| Fluxo | Descrição | Blocos |
| --- | --- | --- |
| `suporte-tec-v2` | Suporte técnico principal | 116 |
| `cons-de-reparo` | Consulta de ordens de serviço | 62 |
| `senha-wifi` | Alteração de senha WiFi | 21 |
| `lentidao` | Diagnóstico de lentidão | 28 |

### Fluxos de Alteração

| Fluxo | Descrição | Blocos |
| --- | --- | --- |
| `troca-de-plano` | Upgrade/downgrade de planos | 46 |
| `troca-venciment` | Alteração de data de vencimento | 43 |
| `endereco` | Mudança de endereço residencial | 74 |
| `end-comercial` | Mudança de endereço comercial | 41 |
| `alt-cadastrais` | Alterações cadastrais | 56 |

### Fluxos Especiais

| Fluxo | Descrição | Blocos |
| --- | --- | --- |
| `cancelamento-tx` | Processo de cancelamento | 62 |
| `ex-cliente` | Atendimento a ex-clientes | 35 |
| `suspenso-debito` | Clientes suspensos por débito | 82 |
| `suspenso-solic` | Clientes suspensos por solicitação | 7 |
| `fluxo-ativacao` | Ativação de novos serviços | 58 |
| `massiva` | Tratamento de incidentes massivos | 38 |

### Fluxos de Produtos Adicionais

| Fluxo | Descrição | Blocos |
| --- | --- | --- |
| `ott` | Serviços de streaming (Giga+TV, Globoplay, Max) | 49 |
| `softbundle` | Pacotes combinados | 44 |

### Fluxos de Notificação

| Fluxo | Descrição | Blocos |
| --- | --- | --- |
| `notifica` | Notificações proativas | 24 |
| `notifica-neg` | Notificações de negociação | 48 |
| `nps-fintalk` | Pesquisa NPS | 29 |

### Fluxos Auxiliares

| Fluxo | Descrição | Blocos |
| --- | --- | --- |
| `validacao-dados` | Validação de dados do cliente | 18 |
| `validacao-token` | Validação de token SMS | 34 |
| `gestao-api` | Gestão de erros de API | 5 |
| `fallback` | Tratamento de mensagens não reconhecidas | 14 |
| `passivo-fatura` | Fluxo passivo de fatura | 5 |

---

## Entidades de Reconhecimento

O bot utiliza as seguintes entidades para reconhecimento de linguagem natural:

| Entidade | Descrição | Exemplos |
| --- | --- | --- |
| `base-positiva` | Respostas afirmativas | sim, ok, pode, quero, certo |
| `base-negativa` | Respostas negativas | não, nunca, negativo, nem |
| `cep-padrao` | Padrão de CEP | 12345-678, 12345678 |
| `sys_session` | Controle de sessão | - |

---

## Filas de Atendimento Humano

O bot pode transferir para atendimento humano através das seguintes filas:

| Fila | Identificador |
| --- | --- |
| Cobrança | Giga-cobrança-chat |
| Cobrança ATEX | Giga_Atex_Cobrança_Chat |
| Cobrança NCC | GIGA_COBRANÇA_NCC_CHAT |
| Fatura SAC | Giga_sac_fatura_chat |
| OTT | Giga_Sac_OTT_CHAT |
| OTT ATEX | Giga_Atex_OTT_Chat |
| Reajuste | Giga_sac_reajuste_chat |
| Reincidente | Giga-Reincidente-CHAT |
| Reincidente ATEX | Giga_Atex_Reincidente_Chat |

---

## Tecnologias e Integrações

- **Plataforma**: Fintalk
- **Automação**: n8n (webhooks)
- **Backend**: API Hub Fintalk
- **Portal Cliente**: portal-gigamaisfibra.sydle.com

---

## Próximos Passos

Para informações detalhadas sobre cada fluxo, consulte os documentos específicos listados no índice acima.
