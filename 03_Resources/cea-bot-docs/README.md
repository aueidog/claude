# Bot de Atendimento C&A - Documentação Completa

## Visão Geral

O **cea-prd** é um bot de atendimento automatizado da C&A Brasil, desenvolvido na plataforma Fintalk. O bot gerencia interações com clientes através do WhatsApp, oferecendo autoatendimento para serviços financeiros do Cartão C&A e C&A Pay (Creliq).

### Informações do Bot

| Propriedade | Valor |
| --- | --- |
| Nome | cea-prd |
| Ambiente | Produção |
| Plataforma | Fintalk |
| Canal Principal | WhatsApp |
| Total de Fluxos | 52 grupos |
| Total de Blocos | ~1.700 |

---

## Arquitetura Geral

```
┌─────────────────────────────────────────────────────────────────────┐
│                          BOT CEA-PRD                                 │
├─────────────────────────────────────────────────────────────────────┤
│  ENTRADA                                                             │
│  ┌──────────┐    ┌────────────────┐    ┌──────────────────┐        │
│  │ Principal │───▶│ Autenticação   │───▶│ Menu Principal   │        │
│  └──────────┘    └────────────────┘    └──────────────────┘        │
│                                                │                    │
│         ┌──────────────────────────────────────┼──────────────────┐ │
│         ▼                  ▼                   ▼                  ▼ │
│  ┌────────────┐    ┌────────────┐    ┌────────────┐    ┌────────┐ │
│  │ Segunda Via│    │  Cartões   │    │  Seguros   │    │  FAQ   │ │
│  │  Fatura    │    │  C&A/Pay   │    │            │    │        │ │
│  └────────────┘    └────────────┘    └────────────┘    └────────┘ │
│         │                  │                   │                    │
│         ▼                  ▼                   ▼                    │
│  ┌─────────────────────────────────────────────────────────────┐   │
│  │                    PRODUTOS FINANCEIROS                      │   │
│  │  • Empréstimo Pessoal    • Parcelamento                     │   │
│  │  • Antecipação           • Limite Disponível                │   │
│  │  • Negociação Dívidas    • Termo de Quitação                │   │
│  └─────────────────────────────────────────────────────────────┘   │
│                              │                                      │
│                        ┌─────┴─────┐                               │
│                        │    NPS    │                               │
│                        └───────────┘                               │
└─────────────────────────────────────────────────────────────────────┘
```

---

## Índice de Documentação

1. [01-fluxos-principais.md](./01-fluxos-principais.md)
2. [02-fluxo-financeiro-cartao.md](./02-fluxo-financeiro-cartao.md)
3. [03-fluxo-cea-pay.md](./03-fluxo-cea-pay.md)
4. [04-fluxo-emprestimos.md](./04-fluxo-emprestimos.md)
5. [05-apis-integracoes.md](./05-apis-integracoes.md)
6. [06-entidades-variaveis.md](./06-entidades-variaveis.md)
7. [07-fluxograma-geral.md](./07-fluxograma-geral.md)

---

## Resumo dos Fluxos

### Fluxos de Entrada e Navegação

| Fluxo | Descrição | Blocos |
| --- | --- | --- |
| `principal` | Ponto de entrada do bot | 8 |
| `autenticacao` | Validação CPF e código SMS | 121 |
| `menu` | Menu principal de navegação | 40 |
| `posso-ajudar` | Retorno pós-atendimento | 23 |
| `fb-inteligente` | Fallback com IA | 35 |

### Fluxos Financeiros - Cartão C&A

| Fluxo | Descrição | Blocos |
| --- | --- | --- |
| `segunda-via` | Emissão de segunda via de fatura | 52 |
| `data-vencimento` | Consulta/alteração vencimento | 47 |
| `alterar-vencime` | Efetua alteração de vencimento | 20 |
| `transacao-alt` | Alteração de transações | 42 |
| `escolhe-parcela` | Seleção de parcelas | 22 |
| `antecipa-parcel` | Antecipação de parcelas | 19 |
| `lim-disponivel` | Consulta limite disponível | 41 |
| `termo-quitacao` | Termo de quitação anual | 17 |

### Fluxos C&A Pay (Creliq)

| Fluxo | Descrição | Blocos |
| --- | --- | --- |
| `aut-pre-creliq` | Autenticação Creliq | 54 |
| `menu-pre-creliq` | Menu do C&A Pay | 43 |
| `pag-pre-creliq` | Pagamentos Creliq | 104 |
| `2-via-pre-creli` | Segunda via Creliq | 33 |
| `valores-creliq` | Consulta valores | 43 |
| `prob-pag-creliq` | Problemas com pagamento | 14 |
| `indevido-creliq` | Cobrança indevida | 10 |
| `seguro-creliq` | Seguros Creliq | 13 |
| `prob-fat-creliq` | Problemas com fatura | 8 |
| `inf-pag-creliq` | Informar pagamento | 12 |
| `par-saldo-total` | Parcelamento saldo total | 64 |

### Fluxos de Empréstimo

| Fluxo | Descrição | Blocos |
| --- | --- | --- |
| `emprestimo` | Menu de empréstimos | 16 |
| `empr-ate7-c-seg` | Empréstimo até 7x com seguro | 51 |
| `empr-ate7-s-seg` | Empréstimo até 7x sem seguro | 38 |
| `empr-mai7-c-seg` | Empréstimo +7x com seguro | 37 |
| `empr-mai7-s-seg` | Empréstimo +7x sem seguro | 29 |
| `seguro-emprest` | Seguro de empréstimo | 14 |

### Fluxos de Seguros

| Fluxo | Descrição | Blocos |
| --- | --- | --- |
| `seguros` | Menu geral de seguros | 55 |
| `seguro-celular` | Seguro celular | 14 |
| `bolsa-protegida` | Seguro bolsa protegida | 15 |
| `garantia-estend` | Garantia estendida | 13 |
| `protecao-premia` | Proteção premiada | 12 |
| `parcela-premiad` | Parcela premiada | 11 |
| `parc-prem-facil` | Parcela premiada fácil | 10 |
| `assist-saude` | Assistência saúde | 17 |
| `assist-odonto` | Assistência odontológica | 16 |
| `bol-prot-assist` | Bolsa protegida + assistência | 14 |

### Fluxos de Cartões

| Fluxo | Descrição | Blocos |
| --- | --- | --- |
| `cartoes` | Menu de cartões | 69 |
| `cobranca-indevi` | Contestação de cobrança | 20 |
| `pedid-devoluc` | Pedido de devolução | 6 |

### Fluxos Auxiliares

| Fluxo | Descrição | Blocos |
| --- | --- | --- |
| `faq` | Perguntas frequentes | 58 |
| `faq-fatura` | FAQ sobre fatura | 48 |
| `nps` | Pesquisa de satisfação | 28 |
| `nps-fintalk` | NPS Fintalk | 29 |
| `lgpd` | Informações LGPD | 5 |
| `preditivo` | Atendimento preditivo | 42 |
| `oft-prod-finan` | Oferta produtos financeiros | 21 |

---

## Entidades de Reconhecimento

O bot utiliza as seguintes entidades para reconhecimento de linguagem natural:

| Entidade | Descrição | Exemplos |
| --- | --- | --- |
| `base-positiva` | Respostas afirmativas | sim, ok, pode, quero |
| `base-negativa` | Respostas negativas | não, nunca, negativo |
| `cep-padrao` | Padrão de CEP | 12345-678, 12345678 |
| `ola` | Saudações | oi, olá |
| `bomdia` | Bom dia | bom dia |
| `boatarde` | Boa tarde | boa tarde |
| `boanoite` | Boa noite | boa noite |
| `despedida` | Despedidas | tchau, até mais |
| `xingamentos` | Palavras ofensivas | (filtro de moderação) |

---

## Integrações Principais

### APIs Backend C&A

| Base URL | Descrição |
| --- | --- |
| `[[api-hub]]` | API principal WhatsApp BFF |
| `[[api-hub-creliq]]` | API C&A Pay (Cobransaas) |
| `[[av-api]]` | Proxy CEA URA |

### Serviços Externos

| Serviço | URL |
| --- | --- |
| Knowledge Base | `https://kb-prd.fintalk.io/questions` |
| n8n Webhook | `https://n8n-prd-webhook.fintalk.io/webhook/fintalk/async` |

---

## Produtos e Serviços Atendidos

### Cartão C&A
- Segunda via de fatura
- Alteração de vencimento
- Consulta de limite
- Parcelamento de compras
- Antecipação de parcelas
- Contestação de cobranças

### C&A Pay (Creliq)
- Consulta de valores
- Segunda via de boleto
- Negociação de dívidas
- Parcelamento de saldo
- Seguros associados

### Empréstimo Pessoal
- Simulação
- Contratação
- Acompanhamento
- Seguro opcional

### Seguros
- Seguro Celular
- Bolsa Protegida
- Garantia Estendida
- Proteção Premiada
- Parcela Premiada
- Assistência Saúde
- Assistência Odontológica

---

## Tecnologias e Plataformas

- **Plataforma Bot**: Fintalk
- **Canal**: WhatsApp Business API
- **Backend**: API Hub C&A
- **Automação**: n8n
- **Knowledge Base**: Fintalk KB
- **IA/NLP**: Fallback inteligente com similaridade

---

## Próximos Passos

Para informações detalhadas sobre cada fluxo, consulte os documentos específicos listados no índice acima.
