# Fluxo de Empréstimos do Bot C&A

## Visão Geral

O módulo de empréstimos gerencia o Empréstimo Pessoal C&A, incluindo simulação, contratação e opções de seguro.

```mermaid
flowchart TD
    A[Menu Empréstimo] --> B{Condições}
    B -->|Até 7x com seguro| C[Empr Até 7x c/ Seg]
    B -->|Até 7x sem seguro| D[Empr Até 7x s/ Seg]
    B -->|Mais de 7x com seguro| E[Empr +7x c/ Seg]
    B -->|Mais de 7x sem seguro| F[Empr +7x s/ Seg]

    C --> G[Contratação]
    D --> G
    E --> G
    F --> G

    G --> H{Quer seguro?}
    H -->|Sim| I[Seguro Empréstimo]
    H -->|Não| J[Finaliza]
```

---

## 1. Menu de Empréstimos (`emprestimo`)

Ponto de entrada para solicitação de empréstimo pessoal.

### Fluxograma

```mermaid
flowchart TD
    A[Início] --> B[API: /emprestimos-pessoal]
    B --> C{Elegível?}
    C -->|Não| D[Não elegível]
    C -->|Sim| E{Valor disponível}

    E --> F[Exibe valor máximo]
    F --> G{Quantidade de parcelas?}

    G -->|Até 7x| H{Quer seguro?}
    G -->|Mais de 7x| I{Quer seguro?}

    H -->|Sim| J[Empr Até 7x c/ Seg]
    H -->|Não| K[Empr Até 7x s/ Seg]

    I -->|Sim| L[Empr +7x c/ Seg]
    I -->|Não| M[Empr +7x s/ Seg]
```

### APIs Utilizadas

| API | Método | Descrição |
| --- | --- | --- |
| `/emprestimos-pessoal` | GET | Verifica elegibilidade e condições |
| `/emprestimos-pessoal/compras/${idCompra}` | GET | Detalhes do empréstimo |

### Componentes

| Tipo | Quantidade |
| --- | --- |
| Mensagens | 1 |
| Total | 16 |

---

## 2. Empréstimo Até 7x com Seguro (`empr-ate7-c-seg`)

Contratação de empréstimo em até 7 parcelas com seguro incluso.

### Fluxograma

```mermaid
flowchart TD
    A[Início] --> B[Exibe condições]
    B --> C[Valor solicitado]
    C --> D[Parcelas: 1-7]
    D --> E[Taxa de juros]
    E --> F[Valor do seguro]

    F --> G[Calcula valor parcela]
    G --> H[Exibe resumo]

    H --> I{Confirma?}
    I -->|Sim| J[API: Contrata empréstimo]
    I -->|Não| K[Menu]

    J --> L{Sucesso?}
    L -->|Sim| M[Confirmação]
    L -->|Não| N[Erro]

    M --> O[Crédito em conta]
    O --> P[Posso Ajudar]
```

### Informações Exibidas

| Informação | Descrição |
| --- | --- |
| Valor Solicitado | Valor do empréstimo |
| Quantidade Parcelas | 1 a 7 parcelas |
| Taxa de Juros | % ao mês |
| Valor Seguro | Valor mensal do seguro |
| CET | Custo Efetivo Total |
| Valor Parcela | Valor de cada parcela |
| Valor Total | Valor total a pagar |

### Componentes

| Tipo | Quantidade |
| --- | --- |
| Lógicas | 4 |
| Total | 51 |

---

## 3. Empréstimo Até 7x sem Seguro (`empr-ate7-s-seg`)

Contratação de empréstimo em até 7 parcelas sem seguro.

### Fluxograma

```mermaid
flowchart TD
    A[Início] --> B[Exibe condições]
    B --> C[Valor solicitado]
    C --> D[Parcelas: 1-7]
    D --> E[Taxa de juros]

    E --> F[Calcula valor parcela]
    F --> G[Exibe resumo]

    G --> H{Confirma?}
    H -->|Sim| I[API: Contrata]
    H -->|Não| J[Menu]

    I --> K{Sucesso?}
    K -->|Sim| L[Confirmação]
    K -->|Não| M[Erro]
```

### Diferenças do com Seguro

| Aspecto | Com Seguro | Sem Seguro |
| --- | --- | --- |
| Valor parcela | Maior | Menor |
| Proteção | Sim | Não |
| CET | Maior | Menor |

### Componentes

| Tipo | Quantidade |
| --- | --- |
| Lógicas | 4 |
| Total | 38 |

---

## 4. Empréstimo +7x com Seguro (`empr-mai7-c-seg`)

Contratação de empréstimo acima de 7 parcelas com seguro.

### Fluxograma

```mermaid
flowchart TD
    A[Início] --> B[Exibe condições]
    B --> C[Valor solicitado]
    C --> D[Parcelas: 8-24]
    D --> E[Taxa de juros]
    E --> F[Valor do seguro]

    F --> G[Calcula valor parcela]
    G --> H[Exibe resumo]

    H --> I{Confirma?}
    I -->|Sim| J[Contrata]
    I -->|Não| K[Menu]
```

### Opções de Parcelas

| Parcelas | Disponibilidade |
| --- | --- |
| 8x | Disponível |
| 10x | Disponível |
| 12x | Disponível |
| 18x | Disponível |
| 24x | Disponível |

### Componentes

| Tipo | Quantidade |
| --- | --- |
| Lógicas | 4 |
| Total | 37 |

---

## 5. Empréstimo +7x sem Seguro (`empr-mai7-s-seg`)

Contratação de empréstimo acima de 7 parcelas sem seguro.

### Componentes

| Tipo | Quantidade |
| --- | --- |
| Lógicas | 2 |
| Total | 29 |

---

## 6. Seguro de Empréstimo (`seguro-emprest`)

Informações e contratação do seguro de empréstimo.

### Coberturas do Seguro

| Cobertura | Descrição |
| --- | --- |
| Morte | Quitação do saldo devedor |
| Invalidez | Quitação do saldo devedor |
| Desemprego | Pagamento de parcelas |
| Incapacidade | Pagamento temporário |

### Fluxograma

```mermaid
flowchart TD
    A[Início] --> B{Já tem seguro?}
    B -->|Sim| C[Exibe detalhes]
    B -->|Não| D[Oferece contratação]

    C --> E{Opção}
    E -->|Cancelar| F[Processo cancelamento]
    E -->|Manter| G[Menu]

    D --> H{Quer contratar?}
    H -->|Sim| I[Exibe condições]
    H -->|Não| J[Menu]

    I --> K[Confirma contratação]
    K --> L[Posso Ajudar]
```

### Componentes

| Tipo | Quantidade |
| --- | --- |
| Lógicas | 1 |
| Total | 14 |

---

## Fluxo Completo de Contratação

```mermaid
flowchart TD
    subgraph SIMULACAO["1. Simulação"]
        A[Verifica elegibilidade] --> B[Calcula valor disponível]
        B --> C[Define condições]
    end

    subgraph ESCOLHA["2. Escolha"]
        D[Quantidade parcelas] --> E{Quer seguro?}
        E -->|Sim| F[Adiciona seguro]
        E -->|Não| G[Sem seguro]
    end

    subgraph CONFIRMACAO["3. Confirmação"]
        H[Exibe resumo] --> I[Termos e condições]
        I --> J{Aceita?}
    end

    subgraph EFETIVACAO["4. Efetivação"]
        K[Processa contratação] --> L[Crédito em conta]
        L --> M[Confirmação]
    end

    SIMULACAO --> ESCOLHA
    ESCOLHA --> CONFIRMACAO
    CONFIRMACAO --> EFETIVACAO
```

---

## Variáveis do Módulo Empréstimo

| Variável | Descrição |
| --- | --- |
| `vars.valorEmprestimo` | Valor solicitado |
| `vars.qtdParcelas` | Quantidade de parcelas |
| `vars.taxaJuros` | Taxa de juros mensal |
| `vars.valorParcela` | Valor de cada parcela |
| `vars.valorTotal` | Valor total a pagar |
| `vars.cet` | Custo Efetivo Total |
| `vars.comSeguro` | Se inclui seguro |
| `vars.valorSeguro` | Valor do seguro |
| `vars.idCompraEmprestimo` | ID da operação |

---

## Regras de Negócio

### Elegibilidade

| Critério | Requisito |
| --- | --- |
| Conta ativa | Sim |
| Limite disponível | > R$ 100 |
| Adimplência | Sem atrasos |
| Tempo de conta | > 6 meses |

### Limites

| Parâmetro | Valor |
| --- | --- |
| Valor mínimo | R$ 100,00 |
| Valor máximo | Conforme limite |
| Parcelas mínimas | 1 |
| Parcelas máximas | 24 |

### Taxa de Juros

| Parcelas | Taxa (exemplo) |
| --- | --- |
| 1-7x | X% a.m. |
| 8-12x | Y% a.m. |
| 13-24x | Z% a.m. |

*Taxas sujeitas à aprovação de crédito*

---

## Próximo: [05-apis-integracoes.md](./05-apis-integracoes.md)
