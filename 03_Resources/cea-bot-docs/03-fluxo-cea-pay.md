# Fluxo C&A Pay (Creliq)

## Visão Geral

O C&A Pay (anteriormente conhecido como Creliq) é a solução de crédito e parcelamento da C&A. Este módulo gerencia todas as operações relacionadas a este produto financeiro.

```mermaid
flowchart TD
    A[Menu C&A Pay] --> B{Opção}
    B -->|Segunda Via| C[2 Via Creliq]
    B -->|Valores| D[Consulta Valores]
    B -->|Pagamento| E[Pagamento Creliq]
    B -->|Problemas| F[Problemas Pagamento]
    B -->|Seguros| G[Seguros Creliq]
    B -->|Parcelamento| H[Saldo Total]

    C --> I[Menu Creliq]
    D --> I
    E --> I
    F --> I
    G --> I
    H --> I
```

---

## 1. Autenticação C&A Pay (`aut-pre-creliq`)

Autenticação específica para clientes do C&A Pay.

### Fluxograma

```mermaid
flowchart TD
    A[Início] --> B[Solicita CPF]
    B --> C[API: /oauth/token]
    C --> D[API: /api/clientes]
    D --> E{Cliente encontrado?}

    E -->|Não| F[Não é cliente C&A Pay]
    E -->|Sim| G[Exibe dados cadastro]

    G --> H{Dados corretos?}
    H -->|Sim, está| I[Menu C&A Pay]
    H -->|Quero corrigir| J[Solicita correção]

    J --> K[Atualiza dados]
    K --> I
```

### APIs Utilizadas

| API | Método | Descrição |
| --- | --- | --- |
| `/oauth/token` | POST | Autenticação Creliq |
| `/api/clientes` | GET | Busca dados do cliente |
| `/api/clientes/${idCliente}` | GET | Detalhes do cliente |

### Componentes

| Tipo | Quantidade |
| --- | --- |
| Mensagens | 2 |
| Perguntas | 8 |
| Múltipla Escolha | 1 |
| Lógicas | 7 |
| Total | 54 |

---

## 2. Menu C&A Pay (`menu-pre-creliq`)

Menu principal do C&A Pay.

### Opções Disponíveis

| Opção | Fluxo Destino | Descrição |
| --- | --- | --- |
| Segunda Via | `2-via-pre-creli` | Emissão de boleto |
| Valores | `valores-creliq` | Consulta de valores |
| Pagamento | `pag-pre-creliq` | Informar/realizar pagamento |
| Problemas | `prob-pag-creliq` | Problemas com pagamento |
| Seguros | `seguro-creliq` | Seguros associados |
| Parcelamento | `par-saldo-total` | Parcelamento do saldo |

### Componentes

| Tipo | Quantidade |
| --- | --- |
| Lógicas | 1 |
| Total | 43 |

---

## 3. Segunda Via C&A Pay (`2-via-pre-creli`)

Emissão de segunda via do boleto C&A Pay.

### Fluxograma

```mermaid
flowchart TD
    A[Início] --> B[API: /api/contratos]
    B --> C{Contrato encontrado?}
    C -->|Não| D[Sem débitos]
    C -->|Sim| E[API: /api/contratos/${id}/boletos]

    E --> F{Boleto disponível?}
    F -->|Sim| G[Exibe dados boleto]
    F -->|Não| H[Gera novo boleto]

    G --> I[Envia código de barras]
    H --> I
    I --> J[Posso Ajudar]
```

### APIs Utilizadas

| API | Método | Descrição |
| --- | --- | --- |
| `/api/contratos` | GET | Lista contratos |
| `/api/contratos/boletos/${idParcela}` | GET | Busca/gera boleto |

### Componentes

| Tipo | Quantidade |
| --- | --- |
| Lógicas | 7 |
| Total | 33 |

---

## 4. Valores C&A Pay (`valores-creliq`)

Consulta de valores devidos no C&A Pay.

### Fluxograma

```mermaid
flowchart TD
    A[Início] --> B[API: /api/contratos]
    B --> C{Contrato encontrado?}
    C -->|Não| D[Sem débitos]
    C -->|Sim| E[Exibe valores]

    E --> F[Valor total]
    F --> G[Parcelas em aberto]
    G --> H[Próximo vencimento]

    H --> I{Opção}
    I -->|Segunda Via| J[2 Via Creliq]
    I -->|Pagamento| K[Pagamento]
    I -->|Problemas| L[Problemas]
    I -->|Menu| M[Menu Creliq]
```

### Informações Exibidas

| Informação | Descrição |
| --- | --- |
| Valor Total | Soma de todas as parcelas |
| Parcelas em Aberto | Quantidade de parcelas |
| Valor Parcela | Valor de cada parcela |
| Próximo Vencimento | Data da próxima parcela |

### Componentes

| Tipo | Quantidade |
| --- | --- |
| Total | 43 |

---

## 5. Pagamento C&A Pay (`pag-pre-creliq`)

Gerenciamento de pagamentos do C&A Pay.

### Fluxograma

```mermaid
flowchart TD
    A[Início] --> B{Tipo de ação}
    B -->|Informar pagamento| C[Registra pagamento]
    B -->|Gerar boleto| D[API: /api/contratos/boletos]
    B -->|Negociar| E[API: /api/acordos/simular]

    C --> F[Confirmação]

    D --> G[Exibe código de barras]
    G --> H[Envia boleto]

    E --> I{Simulação OK?}
    I -->|Sim| J[Exibe opções]
    I -->|Não| K[Não elegível]

    J --> L{Aceita acordo?}
    L -->|Sim| M[API: /api/acordos/efetivar]
    L -->|Não| N[Menu]

    M --> O{Sucesso?}
    O -->|Sim| P[Gera boleto acordo]
    O -->|Não| Q[Erro]
```

### APIs Utilizadas

| API | Método | Descrição |
| --- | --- | --- |
| `/api/contratos` | GET | Lista contratos |
| `/api/contratos/boletos/${idParcela}` | GET | Gera boleto |
| `/api/acordos/simular` | POST | Simula acordo |
| `/api/acordos/efetivar` | POST | Efetiva acordo |
| `/api/acordos` | GET | Lista acordos |

### Componentes

| Tipo | Quantidade |
| --- | --- |
| Lógicas | 6 |
| Total | 104 |

---

## 6. Problemas com Pagamento (`prob-pag-creliq`)

Tratamento de problemas com pagamentos do C&A Pay.

### Fluxograma

```mermaid
flowchart TD
    A[Início] --> B{Tipo de problema}
    B -->|Cobrança indevida| C[Indevido Creliq]
    B -->|Problema fatura| D[Prob. Fatura]
    B -->|Seguro| E[Seguro Creliq]
    B -->|Valores| F[Valores Creliq]
```

### Opções de Problemas

| Problema | Fluxo | Descrição |
| --- | --- | --- |
| Cobrança indevida | `indevido-creliq` | Contestação |
| Problema na fatura | `prob-fat-creliq` | Erros na fatura |
| Seguro | `seguro-creliq` | Problemas com seguro |
| Valores incorretos | `valores-creliq` | Verificar valores |

### Componentes

| Tipo | Quantidade |
| --- | --- |
| Total | 14 |

---

## 7. Cobrança Indevida C&A Pay (`indevido-creliq`)

Contestação de cobranças no C&A Pay.

### Fluxograma

```mermaid
flowchart TD
    A[Início] --> B[Registra contestação]
    B --> C[API: /api/tarefas/manual]
    C --> D{Tarefa criada?}
    D -->|Sim| E[Protocolo gerado]
    D -->|Não| F[Erro]

    E --> G[Informa prazo análise]
    G --> H[Menu Creliq]
```

### APIs Utilizadas

| API | Método | Descrição |
| --- | --- | --- |
| `/api/tarefas/manual` | POST | Cria tarefa de contestação |

### Componentes

| Tipo | Quantidade |
| --- | --- |
| Total | 10 |

---

## 8. Seguro C&A Pay (`seguro-creliq`)

Gerenciamento de seguros associados ao C&A Pay.

### Fluxograma

```mermaid
flowchart TD
    A[Início] --> B[Consulta seguros]
    B --> C{Tem seguro?}
    C -->|Sim| D[Exibe seguros ativos]
    C -->|Não| E[Oferece contratação]

    D --> F{Opção}
    F -->|Cancelar| G[Processa cancelamento]
    F -->|Usar| H[Sinistro]
    F -->|Menu| I[Menu Creliq]
```

### Componentes

| Tipo | Quantidade |
| --- | --- |
| Total | 13 |

---

## 9. Problema na Fatura (`prob-fat-creliq`)

Tratamento de problemas específicos com fatura.

### Componentes

| Tipo | Quantidade |
| --- | --- |
| Total | 8 |

---

## 10. Informar Pagamento (`inf-pag-creliq`)

Registro de pagamento realizado.

### Fluxograma

```mermaid
flowchart TD
    A[Início] --> B[Solicita comprovante]
    B --> C{Comprovante válido?}
    C -->|Sim| D[Registra pagamento]
    C -->|Não| E[Solicita novamente]

    D --> F[Confirmação]
    F --> G[Prazo baixa: 48h]
    G --> H[Menu Creliq]
```

### Componentes

| Tipo | Quantidade |
| --- | --- |
| Total | 12 |

---

## 11. Parcelamento Saldo Total (`par-saldo-total`)

Negociação e parcelamento do saldo devedor total.

### Fluxograma Detalhado

```mermaid
flowchart TD
    A[Início] --> B[API: /api/negociacoes]
    B --> C{Elegível?}
    C -->|Não| D[Não elegível para acordo]
    C -->|Sim| E[API: /api/acordos/simular]

    E --> F[Exibe opções]
    F --> G{Escolha parcelas}
    G --> H[Exibe valor parcela]
    H --> I{Confirma?}

    I -->|Confirmar| J[API: /api/acordos/efetivar]
    I -->|Recusar| K[Outras opções]

    J --> L{Sucesso?}
    L -->|Sim| M[Gera boleto]
    L -->|Não| N[Erro]

    M --> O[Exibe código barras]
    O --> P[Posso Ajudar]
```

### APIs Utilizadas

| API | Método | Descrição |
| --- | --- | --- |
| `/api/negociacoes` | GET | Verifica elegibilidade |
| `/api/acordos/simular` | POST | Simula condições |
| `/api/acordos/efetivar` | POST | Formaliza acordo |
| `/api/contratos/boletos/${idParcela}` | GET | Gera boleto |

### Opções de Parcelamento

| Parcelas | Condições |
| --- | --- |
| À vista | Desconto máximo |
| 2x | Desconto reduzido |
| 3x | Sem desconto |
| 4x+ | Juros aplicados |

### Componentes

| Tipo | Quantidade |
| --- | --- |
| Mensagens | 7 |
| Perguntas | 5 |
| Múltipla Escolha | 3 |
| Lógicas | 26 |
| Total | 64 |

---

## Variáveis do Módulo C&A Pay

| Variável | Descrição |
| --- | --- |
| `vars.idCliente` | ID do cliente Creliq |
| `vars.idContrato` | ID do contrato |
| `vars.idParcela` | ID da parcela |
| `vars.valorTotal` | Valor total devido |
| `vars.qtdParcelas` | Quantidade de parcelas |
| `vars.valorParcela` | Valor de cada parcela |
| `vars.codigoBarras` | Código de barras do boleto |
| `vars.acordoSimulado` | Dados do acordo simulado |
| `vars.acordoEfetivado` | Dados do acordo efetivado |
| `vars.tokenCreliq` | Token de autenticação Creliq |

---

## Base URLs

| Variável | URL |
| --- | --- |
| `[[api-hub-creliq]]` | `https://api-hub.fintalk.io/ceapay/cobransaas` |

---

## Próximo: [04-fluxo-emprestimos.md](./04-fluxo-emprestimos.md)
