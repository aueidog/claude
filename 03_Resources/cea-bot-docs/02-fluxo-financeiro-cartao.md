# Fluxo Financeiro - Cartão C&A

## Visão Geral

O módulo financeiro do Cartão C&A gerencia todas as operações relacionadas a faturas, limites, parcelas e cobranças.

```mermaid
flowchart TD
    A[Menu Financeiro] --> B{Opção}
    B -->|Segunda Via| C[Segunda Via Fatura]
    B -->|Vencimento| D[Data Vencimento]
    B -->|Limite| E[Limite Disponível]
    B -->|Parcelas| F[Transação/Parcelas]
    B -->|Antecipação| G[Antecipa Parcelas]
    B -->|Quitação| H[Termo Quitação]

    C --> I[Posso Ajudar]
    D --> I
    E --> I
    F --> I
    G --> I
    H --> I
```

---

## 1. Segunda Via de Fatura (`segunda-via`)

Emissão de segunda via da fatura do Cartão C&A.

### Fluxograma Detalhado

```mermaid
flowchart TD
    A[Início] --> B[API: /faturas/ultima]
    B --> C{Fatura encontrada?}
    C -->|Não| D[Sem fatura em aberto]
    C -->|Sim| E{Fatura paga?}

    E -->|Sim| F[Informar pagamento]
    E -->|Não| G{Ação desejada}

    F --> H{Opção}
    H -->|Informar pagamento| I[Registra informação]
    H -->|Voltar ao menu| J[Menu]
    H -->|Encerrar| K[Encerramento]

    G --> L{Escolha}
    L -->|Pedir segunda via| M[Gera Segunda Via]
    L -->|Voltar ao menu| J
    L -->|Encerrar| K

    M --> N[API: /faturas/ultima-pdf]
    N --> O[Envia PDF/Link]
    O --> P[Posso Ajudar]
```

### Opções do Menu

| Opção | Descrição |
| --- | --- |
| Informar pagamento | Cliente já pagou |
| Pedir segunda via | Gera nova fatura |
| Voltar ao menu | Retorna ao menu principal |
| Encerrar | Finaliza atendimento |

### APIs Utilizadas

| API | Método | Descrição |
| --- | --- | --- |
| `/faturas/ultima` | GET | Consulta última fatura |
| `/faturas/ultima-pdf` | GET | Gera PDF da fatura |
| `/faturas/contas/` | GET | Lista faturas por conta |

### Componentes

| Tipo | Quantidade |
| --- | --- |
| Mensagens | 7 |
| Múltipla Escolha | 3 |
| Lógicas | 7 |
| Total | 52 |

---

## 2. Data de Vencimento (`data-vencimento`)

Consulta e alteração da data de vencimento da fatura.

### Fluxograma

```mermaid
flowchart TD
    A[Início] --> B[API: /faturas/dias/vencimento]
    B --> C{Consulta OK?}
    C -->|Não| D[Erro - Menu]
    C -->|Sim| E[Exibe vencimento atual]

    E --> F{Quer alterar?}
    F -->|Sim| G[Alterar Vencimento]
    F -->|Não| H[Posso Ajudar]

    G --> I[Exibe opções de data]
    I --> J[Cliente escolhe]
    J --> K[API: Altera vencimento]
    K --> L{Sucesso?}
    L -->|Sim| M[Confirmação]
    L -->|Não| N[Erro]
```

### APIs Utilizadas

| API | Método | Descrição |
| --- | --- | --- |
| `/faturas/dias/vencimento` | GET | Lista datas disponíveis |

### Componentes

| Tipo | Quantidade |
| --- | --- |
| Lógicas | 2 |
| Total | 47 |

---

## 3. Alteração de Vencimento (`alterar-vencime`)

Efetua a alteração da data de vencimento.

### Fluxograma

```mermaid
flowchart TD
    A[Recebe nova data] --> B[API: Altera vencimento]
    B --> C{Sucesso?}
    C -->|Sim| D[Confirma alteração]
    C -->|Não| E[Mensagem erro]

    D --> F[Posso Ajudar]
    E --> G[Menu]
```

### Componentes

| Tipo | Quantidade |
| --- | --- |
| Lógicas | 2 |
| Total | 20 |

---

## 4. Limite Disponível (`lim-disponivel`)

Consulta do limite disponível no cartão.

### Fluxograma

```mermaid
flowchart TD
    A[Início] --> B[API: /cliente/conta]
    B --> C{Conta encontrada?}
    C -->|Não| D[Erro]
    C -->|Sim| E[Exibe limite total]

    E --> F[Exibe limite disponível]
    F --> G[Exibe limite utilizado]
    G --> H[Posso Ajudar]
```

### APIs Utilizadas

| API | Método | Descrição |
| --- | --- | --- |
| `/cliente/conta` | GET | Consulta dados da conta |

### Informações Exibidas

| Informação | Descrição |
| --- | --- |
| Limite Total | Valor total do limite |
| Limite Disponível | Valor disponível para uso |
| Limite Utilizado | Valor já utilizado |

### Componentes

| Tipo | Quantidade |
| --- | --- |
| Lógicas | 9 |
| Total | 41 |

---

## 5. Transação e Alteração (`transacao-alt`)

Gerenciamento de transações e parcelamentos.

### Fluxograma

```mermaid
flowchart TD
    A[Início] --> B[API: /compras/meses-anteriores]
    B --> C{Compras encontradas?}
    C -->|Não| D[Sem compras para alterar]
    C -->|Sim| E[Lista compras]

    E --> F[Cliente seleciona]
    F --> G[Escolhe Parcela]
    G --> H[API: /compras/planos-pagamentos]
    H --> I[Exibe opções]
    I --> J[Confirma alteração]
```

### APIs Utilizadas

| API | Método | Descrição |
| --- | --- | --- |
| `/compras/meses-anteriores` | GET | Lista compras anteriores |
| `/compras/planos-pagamentos` | GET | Opções de parcelamento |
| `/v2/compras/planos-pagamentos` | GET | Opções v2 |
| `/compras/${idCompra}` | GET | Detalhes da compra |
| `/v2/compras/${idCompra}` | PUT | Altera parcelamento |

### Componentes

| Tipo | Quantidade |
| --- | --- |
| Lógicas | 4 |
| Total | 42 |

---

## 6. Escolha de Parcela (`escolhe-parcela`)

Seleção de parcelas para alteração.

### Fluxograma

```mermaid
flowchart TD
    A[Recebe compra selecionada] --> B[API: Busca planos]
    B --> C[Exibe opções]
    C --> D{Escolha}
    D -->|Parcela X| E[Confirma]
    D -->|Voltar| F[Transação Alt]
    E --> G[Processa alteração]
    G --> H[Posso Ajudar]
```

### Componentes

| Tipo | Quantidade |
| --- | --- |
| Total | 22 |

---

## 7. Escolha de Parcela V2 (`v2-escolhe-parc`)

Versão atualizada do fluxo de escolha de parcelas.

### Fluxograma

```mermaid
flowchart TD
    A[Início] --> B{Motivo da alteração}
    B -->|Opção 1| C[Registra motivo 1]
    B -->|Opção 2| D[Registra motivo 2]
    B -->|Opção 3| E[Registra motivo 3]

    C --> F[Solicita quantidade]
    D --> F
    E --> F

    F --> G[API: Altera parcelas]
    G --> H{Sucesso?}
    H -->|Sim| I[Confirmação]
    H -->|Não| J[Erro]
```

### Componentes

| Tipo | Quantidade |
| --- | --- |
| Mensagens | 1 |
| Perguntas | 2 |
| Múltipla Escolha | 1 |
| Lógicas | 11 |
| Total | 40 |

---

## 8. Transação Não Alterável (`trans-n-alterav`)

Tratamento de compras que não podem ser alteradas.

### Fluxograma

```mermaid
flowchart TD
    A[Compra não alterável] --> B[Informa motivo]
    B --> C{Opção}
    C -->|Tentar outra| D[Transação Alt]
    C -->|Menu| E[Menu]
```

### Componentes

| Tipo | Quantidade |
| --- | --- |
| Lógicas | 3 |
| Total | 15 |

---

## 9. Antecipação de Parcelas (`antecipa-parcel`)

Antecipação de parcelas futuras.

### Fluxograma

```mermaid
flowchart TD
    A[Início] --> B[API: /antecipacao-parcelas/compras/antecipacao]
    B --> C{Parcelas disponíveis?}
    C -->|Não| D[Sem parcelas para antecipar]
    C -->|Sim| E[Lista parcelas]

    E --> F[Cliente seleciona]
    F --> G[Exibe valor e desconto]
    G --> H{Confirma?}
    H -->|Sim| I[Processa antecipação]
    H -->|Não| J[Menu]

    I --> K{Sucesso?}
    K -->|Sim| L[Confirmação]
    K -->|Não| M[Erro]
```

### APIs Utilizadas

| API | Método | Descrição |
| --- | --- | --- |
| `/antecipacao-parcelas/compras/antecipacao` | GET | Lista parcelas para antecipação |
| `/antecipacao-parcelas/compras/${idCompra}` | POST | Efetua antecipação |

### Componentes

| Tipo | Quantidade |
| --- | --- |
| Lógicas | 1 |
| Total | 19 |

---

## 10. Termo de Quitação (`termo-quitacao`)

Solicitação de termo de quitação anual.

### Fluxograma

```mermaid
flowchart TD
    A[Início] --> B{Qual ano?}
    B -->|2021| C[2021]
    B -->|2022| D[2022]
    B -->|2023| E[2023]

    C --> F[API: /email/termo-quitacao/]
    D --> F
    E --> F

    F --> G{Sucesso?}
    G -->|Sim| H[Termo enviado por email]
    G -->|Não| I[Erro]
```

### APIs Utilizadas

| API | Método | Descrição |
| --- | --- | --- |
| `/email/termo-quitacao/` | GET | Envia termo por email |

### Componentes

| Tipo | Quantidade |
| --- | --- |
| Múltipla Escolha | 1 |
| Lógicas | 2 |
| Total | 17 |

---

## 11. Cobrança Indevida (`cobranca-indevi`)

Contestação de cobranças não reconhecidas.

### Fluxograma

```mermaid
flowchart TD
    A[Início] --> B{Reconhece o débito?}
    B -->|Sim| C[Prossegue com pagamento]
    B -->|Não| D[Registra contestação]

    D --> E{Quer segunda via?}
    E -->|Sim| F[Segunda Via]
    E -->|Voltar ao menu| G[Menu]
    E -->|Encerrar| H[Encerramento]
```

### Opções de Resposta

| Opção | Ação |
| --- | --- |
| Sim, reconheço | Prossegue com pagamento |
| Não reconheço | Registra contestação |

### Componentes

| Tipo | Quantidade |
| --- | --- |
| Mensagens | 3 |
| Múltipla Escolha | 4 |
| Lógicas | 4 |
| Total | 20 |

---

## Variáveis do Módulo Financeiro

| Variável | Descrição |
| --- | --- |
| `vars.faturaAtual` | Dados da fatura atual |
| `vars.valorFatura` | Valor da fatura |
| `vars.dataVencimento` | Data de vencimento |
| `vars.limiteTotal` | Limite total do cartão |
| `vars.limiteDisponivel` | Limite disponível |
| `vars.compras` | Lista de compras |
| `vars.idCompra` | ID da compra selecionada |
| `vars.parcelas` | Opções de parcelamento |
| `vars.parcelasSelecionadas` | Parcelas escolhidas |

---

## Próximo: [03-fluxo-cea-pay.md](./03-fluxo-cea-pay.md)
