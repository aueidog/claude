# Fluxos Principais do Bot C&A

## Visão Geral da Jornada do Cliente

```mermaid
flowchart TD
    A[Cliente Inicia Contato] --> B[Principal]
    B --> C[Autenticação]
    C --> D{CPF Válido?}
    D -->|Sim| E[Envia Código SMS]
    D -->|Não| F[Solicita Novamente]
    E --> G{Código Válido?}
    G -->|Sim| H[Menu Principal]
    G -->|Não| I[Reenviar/Tentar Novamente]
    H --> J{Escolha do Cliente}
    J -->|Fatura| K[Segunda Via]
    J -->|Cartões| L[Menu Cartões]
    J -->|Seguros| M[Menu Seguros]
    J -->|Empréstimo| N[Menu Empréstimo]
    J -->|C&A Pay| O[Menu Creliq]
    K --> P[Posso Ajudar?]
    L --> P
    M --> P
    N --> P
    O --> P
    P --> Q[NPS]
    Q --> R[Fim]
```

---

## 1. Fluxo Principal (`principal`)

O ponto de entrada do bot, responsável por direcionar o cliente para autenticação.

### Estrutura

```mermaid
flowchart TD
    A[Cliente envia mensagem] --> B{É cliente preditivo?}
    B -->|Sim| C[Fluxo Preditivo]
    B -->|Não| D[Autenticação]
    D --> E[Menu Principal]
```

### Componentes

| Tipo | Quantidade |
| --- | --- |
| Blocos totais | 8 |

---

## 2. Autenticação (`autenticacao`)

Fluxo mais extenso do bot, responsável pela validação do cliente via CPF e código SMS.

### Fluxograma Detalhado

```mermaid
flowchart TD
    A[Início] --> B[Solicita CPF]
    B --> C{CPF Válido?}
    C -->|Não| D[Mensagem Erro]
    D --> E{Tentativas < 3?}
    E -->|Sim| B
    E -->|Não| F[Encerramento]

    C -->|Sim| G[API: /autorizacao]
    G --> H{Cliente encontrado?}
    H -->|Não| I[Cliente não cadastrado]
    H -->|Sim| J[Envia Código SMS]

    J --> K[Solicita Código]
    K --> L{Código Válido?}
    L -->|Não| M{Tentativas < 3?}
    M -->|Sim| N[Reenviar código?]
    M -->|Não| O[Encerramento]
    N -->|Sim| J
    N -->|Não| K

    L -->|Sim| P[API: /autorizacao/validar-codigo]
    P --> Q{Validado?}
    Q -->|Sim| R[Carrega dados cliente]
    Q -->|Não| M
    R --> S[Menu Principal]
```

### APIs Utilizadas

| API | Método | Descrição |
| --- | --- | --- |
| `/autorizacao` | POST | Inicia autenticação e envia SMS |
| `/autorizacao/validar-codigo` | POST | Valida código informado |
| `/cliente` | GET | Carrega dados do cliente |
| `/cliente/creliq` | GET | Verifica se é cliente Creliq |

### Componentes

| Tipo | Quantidade |
| --- | --- |
| Perguntas | 4 |
| Lógicas | 26 |
| Total | 121 |

### Variáveis de Sessão

| Variável | Descrição |
| --- | --- |
| `vars.cpf` | CPF do cliente |
| `vars.token` | Token de autenticação |
| `vars.nome` | Nome do cliente |
| `vars.celular` | Celular cadastrado |
| `vars.isCreliq` | Se é cliente C&A Pay |

---

## 3. Menu Principal (`menu`)

Hub central de navegação do bot.

### Estrutura

```mermaid
flowchart TD
    A[Menu Principal] --> B{Análise de Similaridade}
    B -->|Limites| C[Lim. Disponível]
    B -->|Vencimento| D[Data Vencimento]
    B -->|Segunda Via| E[Segunda Via]
    B -->|Não identificado| F[Fallback Inteligente]

    F --> G{Opção reconhecida?}
    G -->|Sim| H[Fluxo correspondente]
    G -->|Não| I[Posso Ajudar]
```

### Opções Reconhecidas por Similaridade

| Intenção | Fluxo Destino |
| --- | --- |
| limites | `lim-disponivel` |
| data vencimento | `data-vencimento` |
| segunda via | `segunda-via` |
| cartões | `cartoes` |
| seguros | `seguros` |
| empréstimo | `emprestimo` |
| c&a pay | `menu-pre-creliq` |

### Componentes

| Tipo | Quantidade |
| --- | --- |
| Múltipla Escolha | 2 |
| Lógicas | 14 |
| Total | 40 |

---

## 4. Autenticação C&A Pay (`aut-pre-creliq`)

Autenticação específica para clientes do C&A Pay (Creliq).

### Fluxograma

```mermaid
flowchart TD
    A[Início Creliq] --> B[Solicita CPF]
    B --> C[API: /api/clientes]
    C --> D{Cliente Creliq?}
    D -->|Não| E[Não é cliente Creliq]
    D -->|Sim| F[Exibe dados]
    F --> G{Dados corretos?}
    G -->|Sim| H[Menu Creliq]
    G -->|Quero corrigir| I[Solicita correção]
```

### APIs Utilizadas

| API | Método | Descrição |
| --- | --- | --- |
| `/oauth/token` | POST | Autenticação Creliq |
| `/api/clientes` | GET | Busca cliente Creliq |

### Componentes

| Tipo | Quantidade |
| --- | --- |
| Mensagens | 2 |
| Perguntas | 8 |
| Múltipla Escolha | 1 |
| Lógicas | 7 |
| Total | 54 |

---

## 5. Fallback Inteligente (`fb-inteligente`)

Tratamento de mensagens não reconhecidas com análise de similaridade.

### Fluxograma

```mermaid
flowchart TD
    A[Mensagem não reconhecida] --> B[API: KB Questions]
    B --> C{Intenção identificada?}
    C -->|Limites| D[Lim. Disponível]
    C -->|Vencimento| E[Data Vencimento]
    C -->|Segunda via| F[Segunda Via]
    C -->|Não identificada| G{Tentativas < 2?}
    G -->|Sim| H[Solicita reformulação]
    G -->|Não| I[Posso Ajudar]
```

### APIs Utilizadas

| API | Descrição |
| --- | --- |
| `https://kb-prd.fintalk.io/questions` | Knowledge Base para análise de intenção |

### Componentes

| Tipo | Quantidade |
| --- | --- |
| Mensagens | 4 |
| Perguntas | 1 |
| Múltipla Escolha | 1 |
| Lógicas | 20 |
| Total | 35 |

---

## 6. Posso Ajudar (`posso-ajudar`)

Fluxo de retorno após conclusão de atendimento.

### Fluxograma

```mermaid
flowchart TD
    A[Posso ajudar em algo mais?] --> B{Resposta}
    B -->|Sim| C[Menu/Fallback]
    B -->|Não| D[Oferta Produtos]
    B -->|Segunda via| E[Segunda Via]
    D --> F[NPS]
```

### Opções de Resposta

| Resposta | Ação |
| --- | --- |
| Sim | Retorna ao menu |
| Não | Oferta de produtos e NPS |
| Segunda via | Direciona para emissão |

### Componentes

| Tipo | Quantidade |
| --- | --- |
| Mensagens | 2 |
| Múltipla Escolha | 3 |
| Lógicas | 10 |
| Total | 23 |

---

## 7. NPS (`nps`)

Pesquisa de satisfação ao final do atendimento.

### Fluxograma

```mermaid
flowchart TD
    A[Início NPS] --> B[Pergunta satisfação]
    B --> C{Resposta}
    C -->|Positiva| D[Agradecimento]
    C -->|Negativa| E[Motivo dificuldade]
    E --> F{Qual motivo?}
    F -->|Não achei info| G[Registra feedback]
    F -->|Erro| G
    F -->|Demorado| G
    G --> H[Encerramento]
```

### Perguntas do NPS

1. **Facilidade de uso**: Fácil / Difícil / Outros
2. **Motivo dificuldade** (se difícil):
  - Não achei informação
  - Deu mensagem de erro
  - Muito demorado

### Componentes

| Tipo | Quantidade |
| --- | --- |
| Mensagens | 3 |
| Perguntas | 2 |
| Múltipla Escolha | 3 |
| Lógicas | 15 |
| Total | 28 |

---

## 8. Preditivo (`preditivo`)

Atendimento preditivo para clientes identificados com demandas específicas.

### Fluxograma

```mermaid
flowchart TD
    A[Cliente identificado] --> B{Tipo de demanda}
    B -->|Fatura em atraso| C[Oferta segunda via]
    B -->|Limite disponível| D[Informa limite]
    B -->|Outro| E[Menu normal]

    C --> F{Aceita?}
    F -->|Sim| G[Segunda Via]
    F -->|Não| H[Menu]
```

### Componentes

| Tipo | Quantidade |
| --- | --- |
| Mensagens | 10 |
| Múltipla Escolha | 2 |
| Lógicas | 20 |
| Total | 42 |

---

## 9. FAQ (`faq`)

Central de perguntas frequentes sobre diversos assuntos.

### Categorias

| Categoria | Fluxo Destino |
| --- | --- |
| Assistência Odontológica | `assist-odonto` |
| Assistência Saúde | `assist-saude` |
| Bolsa Protegida + Assistência | `bol-prot-assist` |
| Bolsa Protegida | `bolsa-protegida` |
| Garantia Estendida | `garantia-estend` |
| Parcela Premiada | `parcela-premiad` |
| Proteção Premiada | `protecao-premia` |
| Seguro Celular | `seguro-celular` |

### Componentes

| Tipo | Quantidade |
| --- | --- |
| Lógicas | 1 |
| Total | 58 |

---

## 10. LGPD (`lgpd`)

Informações sobre proteção de dados.

### Conteúdo

- Informações sobre coleta de dados
- Direitos do titular
- Como exercer direitos
- Contato do DPO

---

## Variáveis de Contexto Principais

| Variável | Descrição | Escopo |
| --- | --- | --- |
| `vars.cpf` | CPF do cliente | Global |
| `vars.token` | Token de autenticação | Sessão |
| `vars.nome` | Nome do cliente | Global |
| `vars.celular` | Telefone do cliente | Global |
| `vars.isCreliq` | Se é cliente C&A Pay | Sessão |
| `vars.limiteDisponivel` | Limite disponível | Sessão |
| `vars.dataVencimento` | Data de vencimento | Sessão |
| `vars.faturaAberta` | Se há fatura em aberto | Sessão |

---

## Próximo: [02-fluxo-financeiro-cartao.md](./02-fluxo-financeiro-cartao.md)
