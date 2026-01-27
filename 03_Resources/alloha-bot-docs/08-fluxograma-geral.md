# Fluxograma Geral do Bot Alloha

## Arquitetura Completa do Bot

```mermaid
flowchart TB
    subgraph ENTRADA["📥 ENTRADA"]
        P[principal]
        FI[fluxo-inicial]
        NOT[notifica]
        NOTN[notifica-neg]
    end

    subgraph AUTH["🔐 AUTENTICAÇÃO"]
        A[auth]
        VD[validacao-dados]
        VT[validacao-token]
    end

    subgraph MENU["📋 MENU PRINCIPAL"]
        MP[menu-principal]
    end

    subgraph FINANCEIRO["💰 FINANCEIRO"]
        FIN[financeiro]
        FINS[financeiro-susp]
        FINE[financeiro-ex]
        FINM[financeiro-mass]
        NEG[negociacao]
        CONT[contestacao-fin]
        TV[troca-venciment]
    end

    subgraph SUPORTE["🔧 SUPORTE TÉCNICO"]
        ST[suporte-tec-v2]
        CR[cons-de-reparo]
        SW[senha-wifi]
        LEN[lentidao]
        FA[fluxo-ativacao]
    end

    subgraph PLANOS["📦 PLANOS E PRODUTOS"]
        TP[troca-de-plano]
        OTT[ott]
        SB[softbundle]
    end

    subgraph ALTERACOES["✏️ ALTERAÇÕES"]
        AC[alt-cadastrais]
        END[endereco]
        EC[end-comercial]
    end

    subgraph CANCELAMENTO["❌ CANCELAMENTO"]
        CAN[cancelamento-tx]
        EX[ex-cliente]
    end

    subgraph ESPECIAIS["⚠️ ESPECIAIS"]
        MAS[massiva]
        SD[suspenso-debito]
        SS[suspenso-solic]
        PF[passivo-fatura]
    end

    subgraph SAIDA["📤 SAÍDA"]
        ENC[encerramento]
        CSAT[csat]
        NPS[nps-fintalk]
        ATD[atendimento]
        FB[fallback]
        GA[gestao-api]
    end

    %% Fluxo de entrada
    P --> FI
    NOT --> FI
    NOTN --> AUTH

    %% Autenticação
    FI --> A
    FI --> VD
    A --> VT
    VD --> AUTH

    %% Menu principal
    AUTH --> MP
    FI --> MP

    %% Do menu para os fluxos
    MP --> ST
    MP --> FIN
    MP --> CAN
    MP --> AC
    MP --> END
    MP --> TP

    %% Financeiro
    FIN --> NEG
    FIN --> TV
    FIN --> CONT
    FINS --> NEG
    FINE --> EX
    FINM --> NEG

    %% Suporte
    ST --> CR
    ST --> SW
    ST --> LEN
    ST --> OTT

    %% Planos
    TP --> SB
    OTT --> SB

    %% Alterações
    AC --> END
    AC --> EC

    %% Cancelamento
    CAN --> EX

    %% Especiais
    FI --> MAS
    FI --> SD
    SD --> FINS
    SS --> EX
    MAS --> FINM
    PF --> FIN

    %% Saídas
    FIN --> CSAT
    ST --> CSAT
    CAN --> CSAT
    AC --> CSAT
    NEG --> CSAT
    CSAT --> ENC
    ENC --> NPS

    %% Atendimento humano
    FB --> ATD
    GA --> ATD
    CAN --> ATD
    NEG --> ATD
    ST --> ATD
```

---

## Fluxo de Identificação do Cliente

```mermaid
flowchart TD
    A[Cliente entra] --> B[Coleta CPF/CNPJ]
    B --> C{API: alloha-contratos}
    C -->|Encontrado| D{Múltiplos contratos?}
    C -->|Não encontrado| E[ex-cliente]

    D -->|Sim| F[Seleciona contrato]
    D -->|Não| G[Contrato único]

    F --> H{Verifica status}
    G --> H

    H -->|Ativo| I{Em massiva?}
    H -->|Suspenso| J[suspenso-debito]
    H -->|Cancelado| K[ex-cliente]

    I -->|Sim| L[massiva]
    I -->|Não| M[auth]

    M --> N[menu-principal]
```

---

## Fluxo de Resolução de Problemas Técnicos

```mermaid
flowchart TD
    A[Suporte Técnico] --> B{Tipo}
    B -->|Internet| C[Verifica status]
    B -->|TV| D[Diagnóstico TV]
    B -->|OTT| E[Fluxo OTT]

    C --> F{API: connection-status}
    F -->|Online| G{Problema?}
    F -->|Offline| H[Reset orientado]

    G -->|Lentidão| I[Diagnóstico lentidão]
    G -->|Intermitência| J[Verifica OS]
    G -->|Outro| K[Atendimento]

    H --> L{Resolveu?}
    L -->|Sim| M[CSAT]
    L -->|Não| N{Abrir OS?}

    N -->|Sim| O[API: cria-service-order]
    N -->|Não| P[Atendimento]

    O --> Q[Confirma agendamento]
    Q --> M
```

---

## Fluxo de Negociação de Débitos

```mermaid
flowchart TD
    A[Início] --> B{API: elegibilidade}
    B -->|Não elegível| C[Atendimento]
    B -->|Elegível| D{API: negociacao}

    D --> E[Exibe propostas]
    E --> F{Cliente escolhe}

    F -->|Ver detalhes| G[Mostra condições]
    F -->|Aceitar| H[Seleciona parcelas]
    F -->|Recusar| I{Outras opções?}

    G --> F
    I -->|Sim| E
    I -->|Não| J[Encerramento]

    H --> K{API: cria-negociacao}
    K -->|Sucesso| L{Pagamento}
    K -->|Erro| C

    L -->|PIX| M[Gera PIX]
    L -->|Boleto| N[Gera Boleto]

    M --> O[CSAT]
    N --> O
```

---

## Fluxo de Cancelamento com Retenção

```mermaid
flowchart TD
    A[Pedido de cancelamento] --> B[Confirma titularidade]
    B --> C{Motivo}

    C -->|Preço| D[Oferta desconto]
    C -->|Mudança| E[Verifica cobertura]
    C -->|Qualidade| F[Oferta VIP]
    C -->|Concorrência| G[Contraproposta]
    C -->|Outros| H[Atendimento]

    D --> I{Aceita?}
    E --> J{Tem cobertura?}
    F --> I
    G --> I

    I -->|Sim| K[Mantém serviço]
    I -->|Não| L{Segunda oferta?}

    J -->|Sim| M[Processo mudança]
    J -->|Não| N[Confirma cancel]

    L -->|Sim| O[Nova proposta]
    L -->|Não| N

    O --> I
    N --> P{Confirmação final}
    P -->|Sim| Q[Efetua cancelamento]
    P -->|Não| R[Menu principal]
```

---

## Fluxo de Mudança de Endereço

```mermaid
flowchart TD
    A[Início] --> B[Coleta CEP]
    B --> C{API: disponibilidade-cep}

    C -->|Sem cobertura| D[Informa indisponibilidade]
    C -->|Com cobertura| E[Coleta endereço completo]

    E --> F{É condomínio?}
    F -->|Sim| G[API: verifica-condominio]
    F -->|Não| H[API: verifica-disp-casa]

    G --> I{Disponível?}
    H --> I

    I -->|Sim| J[API: disponibilidade-endereco]
    I -->|Não| K[Atendimento]

    J --> L[Exibe datas]
    L --> M[Seleciona data/período]
    M --> N{API: mudanca-endereco}

    N -->|Sucesso| O[Confirmação]
    N -->|Erro| K

    O --> P[CSAT]
```

---

## Legenda dos Fluxos

| Símbolo | Significado |
| --- | --- |
| 📥 | Ponto de entrada |
| 🔐 | Autenticação/Validação |
| 📋 | Menu de navegação |
| 💰 | Operações financeiras |
| 🔧 | Suporte técnico |
| 📦 | Planos e produtos |
| ✏️ | Alterações cadastrais |
| ❌ | Cancelamento |
| ⚠️ | Fluxos especiais |
| 📤 | Saída/Encerramento |

---

## Estatísticas do Bot

| Métrica | Valor |
| --- | --- |
| Total de Fluxos | 38 |
| Total de Blocos | ~1.500 |
| APIs Integradas | 52 |
| Filas de Atendimento | 10 |
| Entidades NLU | 4 |

---

## Navegação

- [README.md](./README.md)
- [01-fluxos-principais.md](./01-fluxos-principais.md)
- [05-apis-integracoes.md](./05-apis-integracoes.md)
