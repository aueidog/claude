# Fluxo de Suporte Técnico do Bot Alloha

## Visão Geral

O módulo de suporte técnico é o mais extenso do bot, com 116 blocos, oferecendo diagnóstico e resolução de problemas de conexão, TV e serviços adicionais.

```mermaid
flowchart TD
    A[Suporte Técnico V2] --> B{Tipo de Problema}
    B -->|Internet| C[Diagnóstico Internet]
    B -->|TV| D[Diagnóstico TV]
    B -->|Serviços Adicionais| E[OTT/Softbundle]

    C --> F{Problema resolvido?}
    D --> F
    F -->|Sim| G[CSAT]
    F -->|Não| H{Abrir OS?}
    H -->|Sim| I[Cria Ordem Serviço]
    H -->|Não| J[Atendimento Humano]
```

---

## 1. Suporte Técnico V2 (`suporte-tec-v2`)

### Menu Principal de Suporte

| Opção | Ícone | Descrição |
| --- | --- | --- |
| Internet | 🌐 | Problemas de conexão |
| TV | 📺 | Problemas com TV |
| Serviços adicionais | ➕ | OTT, streaming |

### Fluxograma de Diagnóstico

```mermaid
flowchart TD
    A[Início Suporte] --> B[API: alloha-connection-status]
    B --> C{Status da conexão}
    C -->|Online| D[Verifica Qualidade]
    C -->|Offline| E[Diagnóstico Offline]

    D --> F{Lentidão?}
    F -->|Sim| G[Fluxo Lentidão]
    F -->|Não| H[Outros problemas]

    E --> I[Procedimentos Reset]
    I --> J{Voltou?}
    J -->|Sim| K[CSAT]
    J -->|Não| L[Verifica OS Existente]

    L --> M[API: alloha-lista-service-orders]
    M --> N{OS Aberta?}
    N -->|Sim| O[Exibe Detalhes OS]
    N -->|Não| P{Abrir nova OS?}
    P -->|Sim| Q[Cria OS]
    P -->|Não| R[Atendimento]
```

### Procedimentos de Reset

```mermaid
flowchart TD
    A[Orientação Reset] --> B[Desligar equipamento]
    B --> C[Aguardar 30 segundos]
    C --> D[Religar equipamento]
    D --> E[Aguardar 2 minutos]
    E --> F{Funcionou?}
    F -->|Voltou a funcionar| G[Sucesso]
    F -->|Ainda não funcionou| H[Próximo passo]
    F -->|Outros| I[Atendimento]
```

### APIs Utilizadas

| API | Descrição |
| --- | --- |
| `alloha-connection-status` | Verifica status da conexão |
| `alloha-lista-service-orders` | Lista ordens de serviço |
| `alloha-cria-service-order` | Abre nova ordem de serviço |
| `alloha-cancel-os` | Cancela ordem de serviço |
| `alloha-remarca-os` | Remarca visita técnica |
| `alloha-disponibilidade-service-order` | Verifica disponibilidade de agenda |

### Componentes

| Tipo | Quantidade |
| --- | --- |
| Mensagens | 18 |
| Perguntas | 1 |
| Múltipla Escolha | 10 |
| Lógicas | 56 |

---

## 2. Consulta de Reparo (`cons-de-reparo`)

Consulta e acompanhamento de ordens de serviço existentes.

### Fluxograma

```mermaid
flowchart TD
    A[Início] --> B[API: alloha-lista-service-orders]
    B --> C{OS encontradas?}
    C -->|Sim| D[Exibe Lista OS]
    C -->|Não| E[Nenhuma OS ativa]

    D --> F{Ação desejada}
    F -->|Ver detalhes| G[Detalhes OS]
    F -->|Remarcar| H[API: alloha-disponibilidade-service-order]
    F -->|Cancelar| I[API: alloha-cancel-os]

    H --> J[Exibe Datas]
    J --> K[Seleciona Data/Período]
    K --> L[API: alloha-remarca-os]
    L --> M{Sucesso?}
    M -->|Sim| N[Confirmação]
    M -->|Não| O[Erro - Atendimento]
```

### Parâmetros de Ordem de Serviço

```json
{
  "auth": "token",
  "contract_number": "123456",
  "cpf": "12345678900",
  "customer_id": "CUS123",
  "phone": "11999999999",
  "protocol": "PROT123",
  "id_motive": "MOT01",
  "id_service": "SVC01",
  "day": "2024-01-15",
  "period": "MANHA",
  "observation": "Cliente relatou sem conexão"
}
```

---

## 3. Senha WiFi (`senha-wifi`)

Alteração de nome e senha da rede WiFi.

### Fluxograma

```mermaid
flowchart TD
    A[Início] --> B[API: alloha-senha-wifi]
    B --> C{Consulta OK?}
    C -->|Sim| D[Exibe Dados Atuais]
    C -->|Não| E[Erro - Atendimento]

    D --> F{Opção}
    F -->|Alterar nome| G[Solicita Novo Nome]
    F -->|Alterar senha| H[Solicita Nova Senha]
    F -->|Encerrar| I[CSAT]

    G --> J{Confirma nome?}
    J -->|Sim| K[API: alloha-wifi-update]
    J -->|Outro nome| G

    H --> L{Confirma senha?}
    L -->|Sim| K
    L -->|Outra senha| H

    K --> M{Sucesso?}
    M -->|Sim| N[Exibe Novos Dados]
    M -->|Não| O[Erro - Atendimento]
```

### APIs Utilizadas

| API | Descrição |
| --- | --- |
| `alloha-senha-wifi` | Consulta dados WiFi atuais |
| `alloha-wifi-update` | Atualiza nome/senha WiFi |

### Parâmetros de Atualização WiFi

```json
{
  "auth": "token",
  "contract_number": "123456",
  "cpf": "12345678900",
  "customer_id": "CUS123",
  "phone": "11999999999",
  "protocol": "PROT123",
  "id": "WIFI001",
  "nome": "MinhaRedeWiFi",
  "senha": "senhanova123",
  "serial": "SN123456",
  "type": "2.4GHz"
}
```

---

## 4. Lentidão (`lentidao`)

Diagnóstico específico para problemas de velocidade.

### Fluxograma

```mermaid
flowchart TD
    A[Início Lentidão] --> B{Problema confirmado?}
    B -->|Sim| C{Opção}
    B -->|Não| D[Encerramento]

    C -->|FAQ| E[Exibe Perguntas Frequentes]
    C -->|Diagnóstico| F[Inicia Diagnóstico]
    C -->|Outros| G[Atendimento]

    F --> H[Verifica Dispositivo]
    H --> I[Verifica Distância]
    I --> J[Verifica Interferência]
    J --> K{Problema identificado?}
    K -->|Sim| L[Solução Orientada]
    K -->|Não| M[Abre OS]
```

### Menu de Opções

| Opção | Ícone | Descrição |
| --- | --- | --- |
| FAQ | 🔍 | Perguntas frequentes |
| Diagnóstico | 📝 | Diagnóstico guiado |
| Outros | - | Atendimento humano |

---

## 5. Fluxo de Ativação (`fluxo-ativacao`)

Acompanhamento de instalação para novos clientes.

### Fluxograma

```mermaid
flowchart TD
    A[Cliente em Ativação] --> B{Status}
    B -->|Aguardando| C[Opções Instalação]
    B -->|Agendada| D[Detalhes Agendamento]
    B -->|Instalada| E[Menu Principal]

    C --> F{Escolha}
    F -->|Data da instalação| G[Exibe Datas Disponíveis]
    F -->|Info do plano| H[Exibe Plano Contratado]
    F -->|Trocar plano| I[Troca de Plano]

    G --> J[Seleciona Data]
    J --> K[API: alloha-cria-service-order]
    K --> L{Sucesso?}
    L -->|Sim| M[Confirmação]
    L -->|Não| N[Atendimento]

    D --> O{Ação}
    O -->|Reagendamento| P[Remarca OS]
    O -->|Falar atendimento| Q[Atendimento Humano]
    O -->|Voltar ao menu| R[Menu Principal]
```

### APIs Utilizadas

| API | Descrição |
| --- | --- |
| `alloha-disponibilidade-service-order` | Agenda disponível |
| `alloha-cria-service-order` | Agenda instalação |
| `alloha-remarca-os` | Remarca instalação |

---

## 6. OTT - Serviços de Streaming (`ott`)

Suporte para serviços de streaming adicionais.

### Serviços Disponíveis

| Serviço | Opções de Suporte |
| --- | --- |
| Giga+ TV | Manual, Pacotes, Como assistir |
| Globoplay | Ativação, Problemas, Cancelamento |
| Max | Ativação, Problemas, Cancelamento |

### Fluxograma

```mermaid
flowchart TD
    A[Início OTT] --> B{Serviço}
    B -->|Giga+ TV| C[Menu Giga+]
    B -->|Globoplay| D[Menu Globoplay]
    B -->|Max| E[Menu Max]

    C --> F{Opção}
    F -->|Manual do aplicativo| G[Link Manual]
    F -->|Pacotes e canais| H[Info Pacotes]
    F -->|Como assistir na TV| I[Instruções]

    D --> J{Ação}
    J -->|Ativar| K[API: alloha-ativacao-produto]
    J -->|Problema| L[Diagnóstico]
    J -->|Cancelar| M[Atendimento]
```

### APIs Utilizadas

| API | Descrição |
| --- | --- |
| `alloha-ativacao-produto` | Ativa produto OTT |
| `recomendacao-ott` | Recomenda serviços |
| `confirma-recomendacao-ott` | Confirma recomendação |
| `lista-produtos` | Lista produtos disponíveis |

---

## 7. Softbundle (`softbundle`)

Pacotes combinados de serviços.

### Fluxograma

```mermaid
flowchart TD
    A[Início Softbundle] --> B[API: lista-produtos]
    B --> C{Produtos disponíveis?}
    C -->|Sim| D[Exibe Opções]
    C -->|Não| E[Sem ofertas]

    D --> F{Interesse?}
    F -->|Sim| G[Mais de um produto?]
    F -->|Não| H[Encerramento]

    G -->|Sim| I[Seleciona Produtos]
    G -->|Não| J[Produto único]

    I --> K[API: alloha-ativacao-produto]
    J --> K
    K --> L{Sucesso?}
    L -->|Sim| M[Confirmação]
    L -->|Não| N[Atendimento]
```

---

## Filas de Atendimento Técnico

| Situação | Fila |
| --- | --- |
| Suporte Geral | Giga_Sac_Suporte_Chat |
| OTT | Giga_Sac_OTT_CHAT |
| OTT ATEX | Giga_Atex_OTT_Chat |
| Reincidente | Giga-Reincidente-CHAT |
| Reincidente ATEX | Giga_Atex_Reincidente_Chat |

---

## Motivos de Ordem de Serviço

| ID | Motivo |
| --- | --- |
| MOT01 | Sem conexão |
| MOT02 | Lentidão |
| MOT03 | Intermitência |
| MOT04 | Problema equipamento |
| MOT05 | Mudança de cômodo |

---

## Variáveis do Módulo Suporte

| Variável | Descrição |
| --- | --- |
| `vars.connection_status` | Status da conexão |
| `vars.os_list` | Lista de OS |
| `vars.os_selecionada` | OS escolhida |
| `vars.data_agendamento` | Data selecionada |
| `vars.periodo` | Período (manhã/tarde) |
| `vars.wifi_nome` | Nome da rede WiFi |
| `vars.wifi_senha` | Senha WiFi |

---

## Próximo: [04-fluxo-negociacao.md](./04-fluxo-negociacao.md)
