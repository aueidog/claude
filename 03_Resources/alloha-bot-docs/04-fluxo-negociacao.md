# Fluxo de Negociação e Cancelamento do Bot Alloha

## Visão Geral

Este documento detalha os fluxos de negociação de débitos, retenção de clientes e processo de cancelamento.

```mermaid
flowchart TD
    A[Cliente com Débitos] --> B{Elegível para negociação?}
    B -->|Sim| C[Propostas de Acordo]
    B -->|Não| D[Atendimento Humano]

    C --> E{Aceita proposta?}
    E -->|Sim| F[Formaliza Acordo]
    E -->|Não| G{Quer cancelar?}

    G -->|Sim| H[Fluxo Cancelamento]
    G -->|Não| I[Encerramento]

    H --> J[Ofertas de Retenção]
    J --> K{Aceita oferta?}
    K -->|Sim| L[Mantém Serviço]
    K -->|Não| M[Confirma Cancelamento]
```

---

## 1. Negociação de Débitos (`negociacao`)

### Fluxo Completo de Negociação

```mermaid
flowchart TD
    A[Início Negociação] --> B[API: alloha-elegebilidade-negociacao]
    B --> C{Cliente elegível?}
    C -->|Não| D[Mensagem - Não elegível]
    C -->|Sim| E[API: alloha-negociacao]

    D --> F[Atendimento Humano]

    E --> G{Propostas disponíveis?}
    G -->|Não| H[Erro - Atendimento]
    G -->|Sim| I[Exibe Propostas]

    I --> J{Cliente escolhe}
    J -->|Ver detalhes| K[Exibe Condições]
    J -->|Aceitar| L[Seleciona Parcelas]
    J -->|Recusar| M[Oferece Alternativas]

    L --> N[API: alloha-cria-negociacao]
    N --> O{Acordo criado?}
    O -->|Sim| P{Forma de pagamento}
    O -->|Não| Q[Erro - Atendimento]

    P -->|PIX| R[Gera Código PIX]
    P -->|Boleto| S[Gera Boleto]

    R --> T[Exibe Dados Pagamento]
    S --> T
    T --> U[CSAT]
```

### Estrutura da Proposta de Negociação

```json
{
  "id_negotiation": "NEG123456",
  "total_debt": 450.00,
  "proposals": [
    {
      "id_parcel_group": "PARC01",
      "description": "À vista com 30% desconto",
      "parcel_quantity": 1,
      "parcel_value": 315.00,
      "total_value": 315.00,
      "discount_percentage": 30
    },
    {
      "id_parcel_group": "PARC02",
      "description": "3x sem juros",
      "parcel_quantity": 3,
      "parcel_value": 150.00,
      "total_value": 450.00,
      "discount_percentage": 0
    },
    {
      "id_parcel_group": "PARC03",
      "description": "6x com juros",
      "parcel_quantity": 6,
      "parcel_value": 85.00,
      "total_value": 510.00,
      "discount_percentage": -13
    }
  ]
}
```

### APIs Utilizadas na Negociação

| API | Método | Descrição |
| --- | --- | --- |
| `alloha-elegebilidade-negociacao` | POST | Verifica se cliente pode negociar |
| `alloha-negociacao` | POST | Busca propostas disponíveis |
| `alloha-cria-negociacao` | POST | Formaliza o acordo |
| `llm-negocia` | POST | IA para conversação de negociação |
| `llm-negocia-similarity` | POST | Análise de intenção do cliente |

### Parâmetros de Criação de Acordo

```json
{
  "auth": "token_autenticacao",
  "contract_number": "123456",
  "cpf_cnpj": "12345678900",
  "customer_id": "CUS123",
  "phone": "11999999999",
  "protocol": "PROT20240115001",
  "id_negotiation": "NEG123456",
  "id_parcel_group": "PARC01",
  "invoices": ["FAT001", "FAT002", "FAT003"],
  "parcel_quantity": 1,
  "negotiation": {
    "discount": 30,
    "conditions": "À vista"
  }
}
```

---

## 2. Negociação com LLM (`llm-negocia`)

O bot utiliza um modelo de linguagem para negociações mais complexas.

### Fluxo com IA

```mermaid
flowchart TD
    A[Cliente inicia negociação] --> B[Coleta histórico conversa]
    B --> C[API: llm-negocia]
    C --> D{Resposta LLM}
    D --> E[Exibe resposta ao cliente]
    E --> F{Cliente responde}
    F --> G[API: llm-negocia-similarity]
    G --> H{Intenção identificada}
    H -->|Aceitar| I[Formaliza Acordo]
    H -->|Recusar| J[Oferece alternativa]
    H -->|Dúvida| K[Esclarece condições]
    H -->|Cancelar| L[Fluxo Cancelamento]
```

### Parâmetros LLM

```json
{
  "history": [
    {"role": "assistant", "content": "Olá! Vi que você tem débitos..."},
    {"role": "user", "content": "Quero pagar, mas está difícil"}
  ],
  "pergunta": "Posso parcelar em mais vezes?",
  "resp": "sim",
  "buttons": ["Aceitar proposta", "Ver outras opções", "Falar com atendente"]
}
```

---

## 3. Cancelamento (`cancelamento-tx`)

### Fluxo de Cancelamento com Retenção

```mermaid
flowchart TD
    A[Início Cancelamento] --> B[Confirma Titularidade]
    B --> C{É titular?}
    C -->|Não| D[Solicita Titular]
    C -->|Sim| E[Motivo Cancelamento]

    E --> F{Motivo}
    F -->|Preço| G[Oferta Desconto]
    F -->|Mudança| H[Verifica Cobertura]
    F -->|Qualidade| I[Oferta Suporte VIP]
    F -->|Concorrência| J[Oferta Competitiva]
    F -->|Outros| K[Atendimento]

    G --> L{Aceita oferta?}
    H --> M{Tem cobertura?}
    I --> L
    J --> L

    L -->|Sim| N[Mantém Serviço]
    L -->|Não| O[Confirma Cancelamento]

    M -->|Sim| P[Processo Mudança]
    M -->|Não| O

    O --> Q{Confirmação final}
    Q -->|Sim| R[API: Cancela Contrato]
    Q -->|Não| S[Retorna Menu]
```

### Menu de Motivos de Cancelamento

| Opção | Tratamento |
| --- | --- |
| Preço alto | Ofertas de desconto/downgrade |
| Mudança de endereço | Verifica cobertura no novo endereço |
| Qualidade do serviço | Oferece suporte prioritário |
| Oferta da concorrência | Proposta competitiva |
| Não utilizo mais | Planos mais econômicos |
| Outros | Atendimento humano |

### Ofertas de Retenção

```mermaid
flowchart TD
    A[Ofertas Disponíveis] --> B{Tipo de Oferta}
    B -->|Desconto| C[10-30% por 3-6 meses]
    B -->|Downgrade| D[Plano mais barato]
    B -->|Upgrade| E[Mais velocidade mesmo preço]
    B -->|Fidelidade| F[Benefícios exclusivos]

    C --> G[Cliente escolhe]
    D --> G
    E --> G
    F --> G

    G --> H{Aceita?}
    H -->|Sim| I[Aplica Oferta]
    H -->|Não| J[Próxima oferta ou Cancela]
```

---

## 4. Suspensão por Débito (`suspenso-debito`)

### Fluxo para Clientes Suspensos

```mermaid
flowchart TD
    A[Cliente Suspenso] --> B[Informa Situação]
    B --> C[API: alloha-verifica-blacklist]
    C --> D{Na blacklist?}
    D -->|Sim| E[Atendimento Obrigatório]
    D -->|Não| F[Opções]

    F --> G{Escolha}
    G -->|Informar pagamento| H[Desbloqueio Confiança]
    G -->|Emitir fatura| I[Segunda Via]
    G -->|Renegociar| J[Negociação]

    H --> K[API: alloha-desbloqueio-confianca]
    K --> L{Aprovado?}
    L -->|Sim| M[Serviço Liberado]
    L -->|Não| N[Enviar Comprovante]

    N --> O[API: alloha-desbloqueio-comprovante]
    O --> P{Válido?}
    P -->|Sim| M
    P -->|Não| Q[Atendimento]
```

### Desbloqueio por Confiança

Condições:
- Primeira vez nos últimos 6 meses
- Débito inferior a R$ 500,00
- Tempo de cliente superior a 3 meses

### Parâmetros de Desbloqueio

```json
{
  "auth": "token",
  "contract_number": "123456",
  "cpf": "12345678900",
  "customer_id": "CUS123",
  "phone": "11999999999",
  "protocol": "PROT123"
}
```

### Parâmetros de Comprovante

```json
{
  "auth": "token",
  "contract_number": "123456",
  "invoce_code": "FAT123456",
  "file_name": "comprovante.pdf",
  "file_extension": "pdf",
  "file_url": "https://storage.../comprovante.pdf"
}
```

---

## 5. Ex-Cliente (`ex-cliente`)

### Fluxo para Ex-Clientes

```mermaid
flowchart TD
    A[Ex-Cliente Identificado] --> B{Motivo do Contato}
    B -->|Cobrança| C[Financeiro Ex-Cliente]
    B -->|Não devolução| D[Portal Devolução]
    B -->|Multa fidelidade| E[Atendimento]

    C --> F[Lista Faturas Pendentes]
    F --> G{Quer pagar?}
    G -->|Sim| H[Gera Fatura]
    G -->|Não| I[Encerramento]

    D --> J[Link Portal Sydle]
    J --> K[Instruções Devolução]
```

### Opções para Ex-Clientes

| Demanda | Tratamento |
| --- | --- |
| Cobrança indevida | Análise e estorno |
| Devolução de valores | Portal de autoatendimento |
| Multa de fidelidade | Atendimento humano para análise |
| Retorno como cliente | Verificação de disponibilidade |

---

## 6. Fluxo de Notificação de Negociação (`notifica-neg`)

### Notificações Proativas

```mermaid
flowchart TD
    A[Sistema Identifica Débito] --> B[Envia Notificação]
    B --> C{Cliente Responde}
    C -->|Quer negociar| D[Inicia Negociação]
    C -->|Já paguei| E[Verifica Pagamento]
    C -->|Não tenho interesse| F[Agenda Recontato]

    D --> G[Fluxo Negociação]
    E --> H{Pagamento confirmado?}
    H -->|Sim| I[Baixa automática]
    H -->|Não| J[Solicita Comprovante]
```

### Parâmetros de Notificação

```json
{
  "auth": "token",
  "contract_number": "123456",
  "cpf": "12345678900",
  "customer_id": "CUS123",
  "phone": "11999999999",
  "protocol": "PROT123",
  "negotiation_type": "proactive",
  "debt_amount": 350.00,
  "days_overdue": 15
}
```

---

## Filas de Atendimento - Negociação

| Situação | Fila |
| --- | --- |
| Negociação geral | Giga-cobrança-chat |
| Cancelamento | Giga_Retencao_Chat |
| Ex-cliente | GIGA_COBRANÇA_NCC_CHAT |
| Reincidente | Giga-Reincidente-CHAT |

---

## Variáveis do Módulo

| Variável | Descrição |
| --- | --- |
| `vars.elegivel_negociacao` | Se cliente pode negociar |
| `vars.propostas` | Lista de propostas |
| `vars.proposta_selecionada` | Proposta escolhida |
| `vars.acordo_id` | ID do acordo formalizado |
| `vars.motivo_cancelamento` | Razão do cancelamento |
| `vars.oferta_retencao` | Oferta de retenção |
| `vars.blacklist` | Se está na blacklist |

---

## Próximo: [05-apis-integracoes.md](./05-apis-integracoes.md)
