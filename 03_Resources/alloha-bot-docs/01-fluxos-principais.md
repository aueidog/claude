# Fluxos Principais do Bot Alloha

## Visão Geral da Jornada do Cliente

```mermaid
flowchart TD
    A[Cliente Inicia Contato] --> B[Principal]
    B --> C{É Cliente?}
    C -->|Sim| D[Fluxo Inicial]
    C -->|Não| E[Ex-Cliente]
    D --> F[Validação de Dados]
    F --> G{Dados Válidos?}
    G -->|Sim| H[Auth - Token]
    G -->|Não| I[Solicita Correção]
    I --> F
    H --> J[Menu Principal]
    J --> K{Escolha do Cliente}
    K -->|Suporte| L[Suporte Técnico V2]
    K -->|Financeiro| M[Financeiro]
    K -->|Cancelamento| N[Cancelamento TX]
    K -->|Alterações| O[Alt. Cadastrais]
    K -->|Mudança Endereço| P[Endereço]
    L --> Q[Encerramento]
    M --> Q
    N --> Q
    O --> Q
    P --> Q
    Q --> R[CSAT]
    R --> S[Fim]
```

---

## 1. Fluxo Principal (`principal`)

O ponto de entrada do bot, responsável por identificar a intenção inicial do cliente.

### Estrutura

```mermaid
flowchart TD
    A[Boas-vindas] --> B{Cliente quer cancelar?}
    B -->|Quero| C[Fluxo Inicial - Cancelamento]
    B -->|Não| D[Fluxo Inicial - Normal]
    B -->|Outros| E[Fallback]
```

### Componentes

| Tipo | Quantidade |
| --- | --- |
| Mensagens | 2 |
| Múltipla Escolha | 1 |
| Lógicas | 6 |

### Opções do Menu Inicial

- **Quero!** - Direciona para cancelamento
- **Não** - Continua atendimento normal
- **Outros** - Fallback

---

## 2. Fluxo Inicial (`fluxo-inicial`)

Responsável pela identificação e validação inicial do cliente.

### Estrutura

```mermaid
flowchart TD
    A[Início] --> B[Pergunta CPF/CNPJ]
    B --> C[API: alloha-contratos]
    C --> D{Cliente encontrado?}
    D -->|Sim| E{Múltiplos contratos?}
    D -->|Não| F[Ex-Cliente]
    E -->|Sim| G[Seleção de Contrato]
    E -->|Não| H[Verifica Massiva]
    G --> H
    H --> I{Em massiva?}
    I -->|Sim| J[Fluxo Massiva]
    I -->|Não| K[Verifica Status]
    K --> L{Suspenso?}
    L -->|Sim| M[Suspenso Débito]
    L -->|Não| N[Auth]
```

### Componentes

| Tipo | Quantidade |
| --- | --- |
| Mensagens | 10 |
| Perguntas | 4 |
| Múltipla Escolha | 3 |
| Lógicas | 33 |

### APIs Utilizadas

- `alloha-contratos` - Busca contratos do cliente
- `alloha-massiva-v2` - Verifica se há incidente massivo
- `alloha-id` - Busca ID do cliente

---

## 3. Autenticação (`auth`)

Gera e valida token de acesso para operações sensíveis.

### Estrutura

```mermaid
flowchart TD
    A[Recebe Dados] --> B[API: alloha-token]
    B --> C{Token Gerado?}
    C -->|Sim| D[Retorna ao Fluxo]
    C -->|Não| E[Gestão de API - Erro]
```

### APIs Utilizadas

- `alloha-token` - Gera token de autenticação

---

## 4. Menu Principal (`menu-principal`)

Hub central de navegação do bot.

### Estrutura

```mermaid
flowchart TD
    A[Menu Principal] --> B{Opção selecionada}
    B -->|Suporte técnico| C[Suporte Tec V2]
    B -->|Segunda via de fatura| D[Financeiro]
    B -->|Mudança de endereço| E[Endereço]
    B -->|Cancelamento| F[Cancelamento TX]
    B -->|Alterações cadastrais| G[Alt. Cadastrais]
    B -->|Troca de plano| H[Troca de Plano]
    B -->|Encerrar| I[Encerramento]
```

### Opções Disponíveis

| Opção | Fluxo Destino |
| --- | --- |
| Suporte técnico | `suporte-tec-v2` |
| Segunda via de fatura | `financeiro` |
| Mudança de endereço | `endereco` |
| Cancelamento | `cancelamento-tx` |
| Alterações cadastrais | `alt-cadastrais` |
| Troca de plano | `troca-de-plano` |

---

## 5. Validação de Dados (`validacao-dados`)

Valida e confirma dados do cliente antes de operações sensíveis.

### Estrutura

```mermaid
flowchart TD
    A[Recebe Dados] --> B[Exibe Dados ao Cliente]
    B --> C{Dados corretos?}
    C -->|Sim| D[Continua Fluxo]
    C -->|Não| E[Solicita Correção]
    C -->|Outros| F[Atendimento Humano]
```

### Componentes

| Tipo | Quantidade |
| --- | --- |
| Mensagens | 3 |
| Perguntas | 2 |
| Múltipla Escolha | 1 |
| Lógicas | 6 |

---

## 6. Validação de Token (`validacao-token`)

Valida token SMS enviado ao cliente para operações seguras.

### Estrutura

```mermaid
flowchart TD
    A[Confirma Telefone] --> B{Telefone correto?}
    B -->|Sim| C[API: envia-token]
    B -->|Não| D[Solicita Telefone]
    C --> E[Pergunta Token]
    E --> F[API: confirma-token]
    F --> G{Token válido?}
    G -->|Sim| H[Continua Operação]
    G -->|Não| I{Tentativas < 3?}
    I -->|Sim| J[Solicita Novamente]
    I -->|Não| K[Atendimento Humano]
```

### APIs Utilizadas

- `envia-token` - Envia SMS com token
- `confirma-token` - Valida token informado
- `verifica-validacao` - Verifica status da validação

---

## 7. Encerramento (`encerramento`)

Finaliza o atendimento e limpa variáveis de sessão.

### Estrutura

```mermaid
flowchart TD
    A[Encerramento] --> B[Reseta Variáveis]
    B --> C{CSAT Respondido?}
    C -->|Não| D[Direciona CSAT]
    C -->|Sim| E[Mensagem Final]
    E --> F[Fim]
```

### Componentes

| Tipo | Quantidade |
| --- | --- |
| Lógicas | 8 |

---

## 8. CSAT (`csat`)

Pesquisa de satisfação do cliente.

### Estrutura

```mermaid
flowchart TD
    A[Verifica se já Respondeu] --> B{Já respondeu?}
    B -->|Sim| C[Encerramento]
    B -->|Não| D{Atendido por humano?}
    D -->|Sim| E[Pergunta Satisfação Humano]
    D -->|Não| F[Pergunta Satisfação Bot]
    E --> G[Registra Resposta]
    F --> G
    G --> H[Encerramento]
```

### Opções de Resposta

- **Sim** - Satisfeito
- **Não** - Insatisfeito
- **Outros** - Comentário livre

---

## 9. Fallback (`fallback`)

Tratamento de mensagens não reconhecidas.

### Estrutura

```mermaid
flowchart TD
    A[Mensagem não reconhecida] --> B[API: alloha-llm]
    B --> C{Intenção identificada?}
    C -->|Sim| D[Direciona para Fluxo]
    C -->|Não| E{Tentativas < 3?}
    E -->|Sim| F[Solicita Nova Mensagem]
    E -->|Não| G[Atendimento Humano]
```

### APIs Utilizadas

- `alloha-llm` - Análise de linguagem natural com LLM

---

## 10. Gestão de API (`gestao-api`)

Tratamento centralizado de erros de API.

### Estrutura

```mermaid
flowchart TD
    A[Erro de API] --> B[Registra Erro]
    B --> C[Mensagem de Erro]
    C --> D[Oferece Opções]
    D --> E{Escolha}
    E -->|Tentar novamente| F[Retry]
    E -->|Atendimento| G[Humano]
```

---

## Variáveis de Contexto Principais

| Variável | Descrição | Escopo |
| --- | --- | --- |
| `vars.cpf` | CPF do cliente | Global |
| `vars.customer_id` | ID do cliente | Global |
| `vars.contract_number` | Número do contrato | Global |
| `vars.token` | Token de autenticação | Sessão |
| `vars.protocol` | Protocolo de atendimento | Sessão |
| `vars.phone` | Telefone do cliente | Global |
| `vars.csat_respondido` | Flag de CSAT respondido | Sessão |

---

## Próximo: [02-fluxo-financeiro.md](./02-fluxo-financeiro.md)
