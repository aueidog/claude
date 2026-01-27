# Fluxo Financeiro do Bot Alloha

## Visão Geral

O módulo financeiro gerencia todas as operações relacionadas a faturas, pagamentos, negociações e contestações.

```mermaid
flowchart TD
    A[Menu Financeiro] --> B{Tipo de Cliente}
    B -->|Ativo| C[Financeiro]
    B -->|Suspenso| D[Financeiro Susp]
    B -->|Ex-Cliente| E[Financeiro Ex]
    B -->|Em Massiva| F[Financeiro Mass]

    C --> G{Opção}
    G -->|Alterar vencimento| H[Troca Vencimento]
    G -->|Segunda via| I[Lista Faturas]
    G -->|Negociar| J[Negociação]
    G -->|Contestar| K[Contestação]

    I --> L[Gera Fatura]
    J --> M[Proposta Acordo]
    K --> N[Atendimento Humano]
```

---

## 1. Financeiro (`financeiro`)

Fluxo principal para clientes ativos com pendências financeiras.

### Menu de Opções

| Opção | Descrição | Destino |
| --- | --- | --- |
| Alterar vencimento | Troca data de vencimento | `troca-venciment` |
| Forma de pagamento | Opções de pagamento | Menu interno |
| Segunda via de fatura | Emissão de boleto/PIX | Geração fatura |
| Voltar ao menu | Retorna ao menu principal | `menu-principal` |

### Fluxograma Detalhado

```mermaid
flowchart TD
    A[Entrada Financeiro] --> B[API: alloha-lista-faturas]
    B --> C{Faturas encontradas?}
    C -->|Sim| D[Exibe Faturas]
    C -->|Não| E[Sem pendências]
    D --> F{Ação desejada}
    F -->|Segunda via| G[Seleciona Fatura]
    F -->|Negociar| H[Verifica Elegibilidade]
    F -->|Alterar vencimento| I[Troca Vencimento]

    G --> J{Forma de Pagamento}
    J -->|PIX| K[Gera PIX]
    J -->|Boleto| L[Gera Boleto]

    H --> M{Elegível?}
    M -->|Sim| N[Fluxo Negociação]
    M -->|Não| O[Atendimento]

    K --> P[Exibe Código PIX]
    L --> Q[Envia Link Boleto]
    P --> R[CSAT]
    Q --> R
```

### APIs Utilizadas

| API | Método | Descrição |
| --- | --- | --- |
| `alloha-lista-faturas` | POST | Lista faturas do contrato |
| `alloha-elegebilidade-negociacao` | POST | Verifica elegibilidade para acordo |
| `alloha-gera-protocolo` | POST | Gera protocolo de atendimento |

### Componentes

| Tipo | Quantidade |
| --- | --- |
| Mensagens | 15 |
| Perguntas | 1 |
| Múltipla Escolha | 11 |
| Lógicas | 42 |

---

## 2. Financeiro Suspenso (`financeiro-susp`)

Fluxo para clientes com serviço suspenso por inadimplência.

### Fluxograma

```mermaid
flowchart TD
    A[Cliente Suspenso] --> B[Informa Situação]
    B --> C[API: alloha-lista-faturas]
    C --> D{Possui faturas?}
    D -->|Sim| E[Opções]
    D -->|Não| F[Erro - Atendimento]

    E --> G{Escolha}
    G -->|Emitir fatura| H[Seleciona Fatura]
    G -->|Informar pagamento| I[Desbloqueio Confiança]
    G -->|Renegociar| J[Negociação]

    H --> K[Gera PIX/Boleto]
    I --> L[API: alloha-desbloqueio-confianca]
    L --> M{Aprovado?}
    M -->|Sim| N[Serviço Liberado]
    M -->|Não| O[Atendimento]
```

### Opções do Menu

- **Informar pagamento** - Desbloqueio por confiança
- **Emitir segunda via** - Gera nova fatura
- **Renegociar débitos** - Inicia negociação

### APIs Utilizadas

| API | Descrição |
| --- | --- |
| `alloha-lista-faturas` | Lista faturas pendentes |
| `alloha-desbloqueio-confianca` | Solicita desbloqueio |
| `alloha-desbloqueio-comprovante` | Envia comprovante de pagamento |

---

## 3. Financeiro Ex-Cliente (`financeiro-ex`)

Fluxo para ex-clientes com pendências.

### Fluxograma

```mermaid
flowchart TD
    A[Ex-Cliente] --> B{Tipo de Demanda}
    B -->|Cobrança| C[Lista Faturas]
    B -->|Não devolução| D[Portal Devolução]
    B -->|Multa fidelidade| E[Atendimento]

    C --> F[API: alloha-lista-faturas]
    F --> G{Escolha pagamento}
    G -->|PIX| H[Gera PIX]
    G -->|Boleto| I[Gera Boleto]

    D --> J[Link Portal Sydle]
```

### Opções do Menu

- **Cobrança** - Faturas em aberto
- **Não devolução** - Devolução de valores
- **Multa por fidelidade** - Contestação

### Portal de Devolução

Link: `https://portal-gigamaisfibra.sydle.com/devolucao-de-credito`

---

## 4. Financeiro Massiva (`financeiro-mass`)

Fluxo financeiro durante incidentes massivos.

### Fluxograma

```mermaid
flowchart TD
    A[Cliente em Massiva] --> B[Informa Incidente]
    B --> C{Quer resolver financeiro?}
    C -->|Sim| D[Menu Financeiro Mass]
    C -->|Não| E[Retorna Massiva]

    D --> F{Opção}
    F -->|Alterar vencimento| G[Troca Vencimento]
    F -->|Forma pagamento| H[Opções Pagamento]
    F -->|Segunda via| I[Lista Faturas]
```

---

## 5. Negociação (`negociacao`)

Fluxo de renegociação de débitos.

### Fluxograma Detalhado

```mermaid
flowchart TD
    A[Início Negociação] --> B[API: alloha-elegebilidade-negociacao]
    B --> C{Elegível?}
    C -->|Não| D[Atendimento Humano]
    C -->|Sim| E[API: alloha-negociacao]

    E --> F[Exibe Propostas]
    F --> G{Aceita proposta?}
    G -->|Sim| H[Seleciona Parcelas]
    G -->|Não| I[Encerramento]

    H --> J[API: alloha-cria-negociacao]
    J --> K{Acordo criado?}
    K -->|Sim| L{Forma pagamento}
    K -->|Não| M[Erro - Atendimento]

    L -->|PIX| N[Gera PIX Acordo]
    L -->|Boleto| O[Gera Boleto Acordo]

    N --> P[CSAT]
    O --> P
```

### APIs Utilizadas

| API | Descrição |
| --- | --- |
| `alloha-elegebilidade-negociacao` | Verifica se pode negociar |
| `alloha-negociacao` | Busca propostas disponíveis |
| `alloha-cria-negociacao` | Formaliza acordo |
| `llm-negocia` | LLM para negociação inteligente |
| `llm-negocia-similarity` | Análise de intenção |

### Parâmetros da Negociação

```json
{
  "auth": "token",
  "contract_number": "123456",
  "cpf_cnpj": "12345678900",
  "customer_id": "CUS123",
  "phone": "11999999999",
  "protocol": "PROT123",
  "id_negotiation": "NEG001",
  "id_parcel_group": "PARC01",
  "invoices": ["FAT001", "FAT002"],
  "parcel_quantity": 3
}
```

---

## 6. Troca de Vencimento (`troca-venciment`)

Alteração da data de vencimento das faturas.

### Fluxograma

```mermaid
flowchart TD
    A[Início] --> B[API: alloha-troca-vencimento]
    B --> C{Datas disponíveis?}
    C -->|Sim| D[Exibe Opções]
    C -->|Não| E[Não disponível]

    D --> F{Cliente escolhe}
    F --> G[API: alloha-troca-data-vencimento]
    G --> H{Sucesso?}
    H -->|Sim| I[Confirma Alteração]
    H -->|Não| J[Erro]

    I --> K{Fatura em aberto?}
    K -->|Sim| L[Oferece Segunda Via]
    K -->|Não| M[CSAT]
```

### APIs Utilizadas

| API | Descrição |
| --- | --- |
| `alloha-troca-vencimento` | Lista datas disponíveis |
| `alloha-troca-data-vencimento` | Efetua a troca |

---

## 7. Contestação Financeira (`contestacao-fin`)

Fluxo para contestação de cobranças.

### Fluxograma

```mermaid
flowchart TD
    A[Início Contestação] --> B[API: contestacao-contrato]
    B --> C{Dados carregados?}
    C -->|Sim| D[Exibe Informações]
    C -->|Não| E[Erro - Atendimento]

    D --> F{Ver mensagens?}
    F -->|Sim| G[Exibe Histórico]
    F -->|Não| H[Continua]

    G --> I{Posso ajudar?}
    H --> I
    I -->|Sim| J[Menu Principal]
    I -->|Não| K[Encerramento]
```

---

## Filas de Atendimento Financeiro

| Situação | Fila |
| --- | --- |
| Cobrança geral | Giga-cobrança-chat |
| Cobrança ATEX | Giga_Atex_Cobrança_Chat |
| Cobrança NCC | GIGA_COBRANÇA_NCC_CHAT |
| Fatura SAC | Giga_sac_fatura_chat |
| Reajuste | Giga_sac_reajuste_chat |

---

## Variáveis do Módulo Financeiro

| Variável | Descrição |
| --- | --- |
| `vars.faturas` | Lista de faturas |
| `vars.fatura_selecionada` | Fatura escolhida |
| `vars.valor_total` | Valor total devido |
| `vars.proposta_aceita` | Proposta de negociação |
| `vars.parcelas` | Número de parcelas |
| `vars.pix_code` | Código PIX gerado |
| `vars.boleto_url` | URL do boleto |

---

## Próximo: [03-fluxo-suporte-tecnico.md](./03-fluxo-suporte-tecnico.md)
