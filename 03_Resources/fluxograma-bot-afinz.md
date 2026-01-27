# Documentação do Bot Afinz - Assistente Virtual

## Visão Geral

O **Bot Afinz** (agent0059-prd) é um assistente virtual de atendimento ao cliente da empresa Afinz, uma fintech de serviços financeiros. O bot opera via WhatsApp e oferece autoatendimento para diversas funcionalidades relacionadas a cartão de crédito e empréstimo pessoal.

### Informações Técnicas
- **Nome do Bot**: agent0059-prd
- **Plataforma**: Fintalk
- **Canal Principal**: WhatsApp
- **Total de Grupos**: 21
- **Total de Blocos**: ~550
- **Entidades**: 14

---

## Arquitetura do Bot

### Tipos de Blocos

| Tipo | Nome | Descrição | Quantidade |
| --- | --- | --- | --- |
| 1 | Intent Diversa | Intents para reconhecimento de padrões diversos | 2 |
| 2 | Intent Resposta | Respostas automáticas a padrões reconhecidos | 9 |
| 7 | Enviar para Grupo | Redireciona para outro grupo de fluxo | 65 |
| 8 | Boas-vindas | Intent de início de conversa | 1 |
| 9 | Não Entendi (Fallback) | Tratamento quando não reconhece a mensagem | 1 |
| 10 | Cancelar | Intent de cancelamento de conversa | 1 |
| 100 | Mensagem | Envio de mensagem de texto | 190 |
| 110 | Pergunta/Input | Captura de dados do usuário | 15 |
| 120 | Chamada API/Script | Execução de lógica e chamadas externas | 201 |
| 130 | Menu/Carousel | Apresentação de opções ao usuário | 26 |
| 303 | Broadcast/Pesquisa | Envio de pesquisas e broadcasts | 26 |

### Entidades de Reconhecimento

| Entidade | Descrição | Exemplos |
| --- | --- | --- |
| `base-positiva` | Confirmações positivas | "sim" |
| `base-negativa` | Negações | "nao" |
| `cep-padrao` | Padrão de CEP | Regex para 8 dígitos |
| `xingamentos` | Palavras ofensivas | (filtro de moderação) |
| `bomdia`, `boatarde`, `boanoite` | Saudações | "bom dia", "boa tarde" |
| `ola` | Cumprimentos | "oi" |
| `segundavia` | Solicitação de 2ª via | "segunda-via" |
| `limite` | Consulta de limite | "limite" |
| `parcelamento` | Parcelamento de fatura | "parcelar" |
| `creditopessoal` | Empréstimo pessoal | "credito pessoal" |
| `desbloqueiocartao` | Desbloqueio | "desbloquear cartao" |

---

## Fluxo Geral do Bot

```mermaid
flowchart TB
    subgraph Entrada
        BV[Boas-vindas]
    end

    subgraph Autenticação
        Login[Login via CPF]
        ValidaCPF[Validação CPF]
        EscolheCartao[Seleção de Cartão]
    end

    subgraph MenuPrincipal
        Menu[Menu de Opções]
    end

    subgraph Serviços
        SegundaVia[2ª Via Fatura]
        Limite[Consulta Limite]
        Parcelamento[Parcelamento]
        CP[Crédito Pessoal]
        Desbloqueio[Desbloqueio]
        Recupera[Negociação]
        SaqueJa[Saque Já]
    end

    subgraph Finalização
        NPS[Pesquisa NPS]
        Fim[Fim da Conversa]
    end

    BV --> Login
    Login --> ValidaCPF
    ValidaCPF --> EscolheCartao
    EscolheCartao --> Menu

    Menu --> SegundaVia
    Menu --> Limite
    Menu --> Parcelamento
    Menu --> CP
    Menu --> Desbloqueio

    BV -->|Dívida +90 dias| Recupera
    BV -->|Campanha Saque Já| SaqueJa

    SegundaVia --> NPS
    Limite --> NPS
    Parcelamento --> NPS
    CP --> Fim
    Desbloqueio --> NPS
    Recupera --> Fim
    SaqueJa --> Fim

    NPS --> Fim
```
---

## Grupos e Fluxos

### 1. Principal (`principal`)
**Ponto de entrada do bot**

```mermaid
flowchart TD
    Start([Início]) --> BV[boas-vindas<br/>tipo: 8]
    BV --> VerificaMS[verifica-ms-cp<br/>API tipo: 120]

    VerificaMS -->|com-cp| BVCP[boas-vindas-cp]
    VerificaMS -->|saque| BVSJ[boas-vindas-saqueja]
    VerificaMS -->|sem-cp| MsgBV[msg-boas-vindas]

    BVCP --> VerificaMSG[verifica-mensageio]
    BVSJ --> SaqueJa[Grupo: saqueja]
    MsgBV --> VerificaMSG

    VerificaMSG -->|Meta/Outro| Login[Grupo: login]

    NaoEntendi([nao-entendi<br/>tipo: 9]) --> VerificaSessao
    VerificaSessao --> MsgNaoEntendi[msg-nao-entendi]

    Cancelar([cancelar<br/>tipo: 10]) --> VerificaSessao2
    VerificaSessao2 --> MsgCancelar[msg-cancelar]
    MsgCancelar --> MenuMudo[Grupo: menu-mudo]

    style BV fill:#90EE90
    style NaoEntendi fill:#FFD700
    style Cancelar fill:#FF6B6B
```

**Mensagens Principais:**
- **Boas-vindas**: "Olá! Sou o *Assistente Virtual da Afinz* e estou aqui para te ajudar. 😊"
- **Não entendi**: "Desculpa, não consegui entender. Pode repetir com outras palavras?"
- **Cancelar**: "Ok! Vou finalizar a nossa conversa por aqui, mas pode contar comigo sempre que precisar!"

---

### 2. Login (`login`)
**Autenticação do usuário via CPF**

```mermaid
flowchart TD
    Start([Entrada]) --> CPF[cpf<br/>Captura CPF<br/>tipo: 110]

    CPF -->|Saída| VerificaSessao[verifica-sessao-ccp2]
    CPF -->|Outros| VerificaSessao2[verifica-sessao-ccp2-copy-3]

    VerificaSessao --> ValidaCPF[valida-cpf-func<br/>tipo: 120]

    ValidaCPF -->|Segue| RefreshSessao[refresh-sessao-cpf]
    ValidaCPF -->|CPF inválido| MsgCPFInvalido[msg-cpfinvalido]
    ValidaCPF -->|CPF digitos dif| MsgCPFDigitos[msg-cpfdigitos]

    RefreshSessao --> TentativasAPI[tentativas-api]
    TentativasAPI --> APILogin[api-login<br/>tipo: 120]

    APILogin -->|OK| Saudacao[saudacao<br/>Oi, {{user.name}}!]
    APILogin -->|NúmeroDiferente| MsgProvisoria[msg-provisoria]
    APILogin -->|Timeout| TimeoutHandler[timeout]
    APILogin -->|Erro| ErrorHandler[error]

    Saudacao --> TratativaSaqueJa[tratativa-saqueja]
    TratativaSaqueJa -->|saqueJa| EnviaSaqueJa[Grupo: saqueja]
    TratativaSaqueJa -->|Saída| ValidaCPF2[valida-cpf]

    ValidaCPF2 -->|segue| VerificaCPDireto[verifica-cpdireto]
    ValidaCPF2 -->|escolhe-cartao| EscolherCartao[escolher-cartao]
    ValidaCPF2 -->|nenhum-cartao| ContasBloqueadas[contas-bloqueadas]

    VerificaCPDireto -->|cp-direto-sucesso| CPDireto[Grupo: cp-direto]
    VerificaCPDireto -->|segue| EscolheuCartao[escolheu-cartao]

    EscolheuCartao -->|com-cp| Menu[Grupo: menu]
    EscolheuCartao -->|sem-cp| MenuSemCP[Grupo: menu-semcp]
    EscolheuCartao -->|bloqueado| Bloqueio[Grupo: desbloqueio]

    MsgCPFInvalido --> CPF
    MsgCPFDigitos --> CPF

    TimeoutHandler -->|Tentar| TimeoutLogin[timeout-login]
    TimeoutHandler -->|Timeout| MsgErro[msg-errocpf]

    TimeoutLogin --> APILogin
    MsgErro --> ContadorFallback[contador-fallback]
    ContadorFallback -->|Não pergunta| MsgFinal[msg-final]
    ContadorFallback -->|Pergunta Mais| CPF

    style CPF fill:#87CEEB
    style APILogin fill:#FFD700
    style ValidaCPF fill:#FFD700
```

**Variáveis Capturadas:**
- `user.cpf` - CPF do usuário
- `user.name` - Nome do cliente
- `vars.firstName` - Primeiro nome
- `vars.cartao` - Final do cartão selecionado

---

### 3. Menu Principal (`menu`)
**Menu de opções para clientes autenticados**

```mermaid
flowchart TD
    Start([Entrada do Login]) --> VerificaCP{Cliente tem<br/>Crédito Pessoal?}

    VerificaCP -->|Sim| MenuCP[menu-cp<br/>tipo: 130]
    VerificaCP -->|Não| MenuSemCP[menu-semcp<br/>tipo: 130]

    MenuCP --> Opcoes
    MenuSemCP --> Opcoes

    subgraph Opcoes[Opções do Menu]
        O1[🧾 2ª via de fatura]
        O2[💳 Limite/Vencimento]
        O3[🤏 Parcelar fatura]
        O4[🍀 Empréstimo Pessoal]
        O5[Atendimento]
        O6[Desbloqueio]
        O7[🗂️ Outros assuntos]
    end

    O1 --> SegundaVia[Grupo: segundavia]
    O2 --> Limite[Grupo: limite]
    O3 --> Parcelamento[Grupo: parcelamento]
    O4 --> CreditoPessoal[Grupo: credito-pessoal]
    O5 --> Atendimento[msg-atendimento]
    O6 --> Desbloqueio[Grupo: desbloqueio]
    O7 --> OutrosAssuntos[msg-assuntosdiversos]

    OutrosAssuntos --> SubMenu[Menu Outros Assuntos]

    subgraph SubMenu[Outros Assuntos]
        S1[Estornar]
        S2[Antecipar]
        S3[Contestar]
        S4[Você Bem]
        S5[Quoti]
    end

    S4 --> VoceBem[Vídeo explicativo<br/>Você Bem]

    Atendimento --> MsgAtendimento[Central de Relacionamento<br/>4004-2420]

    style MenuCP fill:#90EE90
    style MenuSemCP fill:#90EE90
```

**Opções Disponíveis:**
| Opção | Descrição | Destino |
| --- | --- | --- |
| 🧾 2ª via de fatura | Consulta e envio de boleto | Grupo `segundavia` |
| 💳 Limite/Vencimento | Informações do cartão | Grupo `limite` |
| 🤏 Parcelar fatura | Parcelamento de fatura | Grupo `parcelamento` |
| 🍀 Empréstimo Pessoal | Solicitação de CP | Grupo `credito-pessoal` |
| Atendimento | Encaminhamento para humano | LiveChat ou Central |
| Desbloqueio | Desbloqueio de cartão | Grupo `desbloqueio` |
| 🗂️ Outros assuntos | Submenu de opções | Estornar, Antecipar, etc. |

---

### 4. Segunda Via de Fatura (`segundavia`)
**Consulta e envio de 2ª via de boleto**

```mermaid
flowchart TD
    Start([Entrada]) --> Msg2Via[msg-2via<br/>Vou verificar a 2ª via...]
    Msg2Via --> APIConsultaFatura[api-consultafatura<br/>tipo: 120]

    APIConsultaFatura -->|escolhe-fatura| EscolheFatura[escolhe-fatura<br/>tipo: 110]
    APIConsultaFatura -->|sem-fatura| SemFatura[sem-fatura<br/>Não tem faturas]
    APIConsultaFatura -->|Timeout| TimeoutFatura[timeout-busca-fatura]
    APIConsultaFatura -->|erro| ErroFatura[erro-fatura]

    EscolheFatura --> VerificaPosicao[verifica-posicao<br/>tipo: 120]

    VerificaPosicao -->|atual| VerificaOpcao[verifica-opcao]
    VerificaPosicao -->|outras| VerificaOpcao

    VerificaOpcao -->|segue| VerificaValor[verifica-valor]
    VerificaOpcao -->|opcao-invalida| EscolheFatura

    VerificaValor -->|segue| APIFaturaPDF[api-faturapdf-copy-1<br/>tipo: 120]
    VerificaValor -->|valorzerado| ValorZerado[valor-zerado]

    APIFaturaPDF -->|fatura-nao-paga| MsgEntregaFatura[msg-entregafatura]
    APIFaturaPDF -->|fatura-paga| MsgFaturaPaga[msg-faturapaga]

    MsgEntregaFatura --> BuscaPDFBoleto[busca-pdf-boleto<br/>tipo: 120]

    BuscaPDFBoleto -->|saida| LinhaDigitavel[linha-digitavel]
    BuscaPDFBoleto -->|Timeout| TimeoutBoleto[timeout-busca-boleto]
    BuscaPDFBoleto -->|erro| ErroBoleto[erro-boleto]

    LinhaDigitavel --> CodigoBarras[codigo-barras<br/>{{vars.linhaDigitavel}}]
    CodigoBarras --> PerguntaOutraFatura[pergunta-outrafatura<br/>tipo: 130]

    PerguntaOutraFatura -->|Sim| EscolheFatura
    PerguntaOutraFatura -->|Não| FinalFluxo[Grupo: finalfluxo]

    TimeoutFatura --> APIConsultaFatura

    style APIConsultaFatura fill:#FFD700
    style BuscaPDFBoleto fill:#FFD700
    style PerguntaOutraFatura fill:#87CEEB
```

**Mensagens:**
- **Fatura atual**: "Prontinho! Aqui está a *2ª via da sua fatura*..."
- **Fatura paga**: "Identifiquei que a fatura do seu cartão com vencimento em... já foi paga!"
- **Sem fatura**: "Identifiquei que você não tem faturas disponíveis. 😃"

---

### 5. Parcelamento de Fatura (`parcelamento`)
**Parcelamento de faturas em atraso**

```mermaid
flowchart TD
    Start([Entrada]) --> ConfirmarCartao[confirmar-cartao<br/>tipo: 130]

    ConfirmarCartao -->|Sim| APISimulacao[api-simulacao<br/>tipo: 120]
    ConfirmarCartao -->|Não| MsgEscolherCar[mensagem-escolhercar]

    APISimulacao -->|Recupera| GrupoRecupera[Grupo: recupera]
    APISimulacao -->|oferta única| UnicaOferta[unica-oferta]
    APISimulacao -->|oferta| MostraOpcao[mostra-opcao]
    APISimulacao -->|semopcao| SemOpcao[sem-opcao]
    APISimulacao -->|semFatura| NenhumaOpcao[nenhuma-opcao]
    APISimulacao -->|erro| ErroParcelamento[erro-parcelamento]
    APISimulacao -->|timeout| TimeoutParcelamento[timeout-parcelamento]

    UnicaOferta --> MostraOpcaoUnica[mostra-opcao<br/>Entrada + Parcelas]

    MostraOpcao --> EscolherOferta[escolher-oferta<br/>tipo: 110]
    MostraOpcaoUnica --> DesejaContinuar[deseja-continuar<br/>tipo: 130]

    DesejaContinuar -->|Sim| APIEfetivacao[api-efetivacao<br/>tipo: 120]
    DesejaContinuar -->|Não| MensagemSAC[mensagem-sac]

    APIEfetivacao -->|sucesso| InstrucaoPagamento[instrucao-pagamento]
    APIEfetivacao -->|erro| ErroEfetivacao[erro-efetivacao]

    InstrucaoPagamento --> DataVencimento[data-vencimento]
    DataVencimento --> APIPDF[api-pdf<br/>tipo: 120]

    APIPDF -->|saida| EnviaBoleto[Envia PDF + Linha Digitável]
    APIPDF -->|erro| ErroPDF[erro-pdf]

    EnviaBoleto --> FinalFluxo[Grupo: finalfluxo]

    MensagemSAC --> FinalFluxo

    style APISimulacao fill:#FFD700
    style APIEfetivacao fill:#FFD700
    style DesejaContinuar fill:#87CEEB
```

**Mensagem de Oferta:**
```
Entrada de *R$ {{vars.dadosParcelamento.ValorAdesao}}*
+ {{vars.dadosParcelamento.QtdParcelas}} parcelas de *R$ {{vars.dadosParcelamento.ValorParcela}}*
```

---

### 6. Crédito Pessoal (`credito-pessoal`)
**Solicitação de empréstimo pessoal**

```mermaid
flowchart TD
    Start([Entrada]) --> TratativaCP[tratativa-cp<br/>tipo: 120]

    TratativaCP -->|semcp| MsgCPNegativa[msg-cpnegativa<br/>Não tem oferta]
    TratativaCP -->|aceito| PerguntaCP[pergunta-cp1<br/>tipo: 130]
    TratativaCP -->|nao-aceito| HorarioFunc[horariofuncionamento]
    TratativaCP -->|msg-natal| MsgNatal[msg-natal<br/>Feriado Natal]
    TratativaCP -->|msg-anonovo| MsgAnoNovo[msg-anonovo<br/>Feriado Ano Novo]

    PerguntaCP -->|ok| LiveChatAPI[livechat-api<br/>tipo: 120]
    PerguntaCP -->|Não| MsgCPNegativa

    LiveChatAPI -->|Conectado| Atendente[Transfere para Atendente]
    LiveChatAPI -->|Sem atendimentos| SemAtendimento[sem-atendimento]

    SemAtendimento --> MsgSemAtendimento[Nossos especialistas<br/>estão ocupados]

    HorarioFunc --> MsgHorario[Atendimento:<br/>Segunda à sexta 8h às 19h]

    MsgCPNegativa --> FinalFluxo[Grupo: finalfluxo]

    style TratativaCP fill:#FFD700
    style LiveChatAPI fill:#FFD700
    style PerguntaCP fill:#90EE90
```

**Horário de Funcionamento:**
- Segunda à sexta: 8h às 19h
- Feriados: Sem atendimento

---

### 7. Recuperação/Acordo (`recupera`)
**Negociação de dívidas em atraso (+90 dias)**

```mermaid
flowchart TD
    Start([Entrada]) --> APIRecupera[api-recupera<br/>tipo: 120]

    APIRecupera -->|negociação| MsgInicioAcordo[msg-iniciodeacordo<br/>tipo: 130]
    APIRecupera -->|outro canal| MsgOutroCanal[msg-outro-canal]
    APIRecupera -->|acordo quebrado| MsgQuebraAcordo[msg-quebra-acordo]

    MsgInicioAcordo -->|Sim| MsgQueroAcordo[msg-queroacordo]
    MsgInicioAcordo -->|Agora não| MsgNaoQuerAcordo[msg-naoqueracordo]

    MsgQueroAcordo --> MsgInformaDebito[msg-informadebito<br/>Valor do débito]
    MsgInformaDebito --> MsgInformaDesconto[msg-informadesconto<br/>Proposta de acordo]
    MsgInformaDesconto --> MsgParcelas[msg-parcelas<br/>tipo: 120]

    MsgParcelas --> EscolhaParcela[Usuário escolhe<br/>quantidade de parcelas]
    EscolhaParcela --> MsgConfirmaParcela[msg-confirma-parcela]
    MsgConfirmaParcela --> MsgConfirmaData[msg-confirma-data]

    MsgConfirmaData --> PerguntaEmail[msg-pergunta-email<br/>tipo: 110]
    PerguntaEmail --> ConfirmaEmail[msg-confirma-email<br/>tipo: 130]

    ConfirmaEmail -->|Sim| ConfirmaAcordo[msg-confirma-acordo<br/>tipo: 130]
    ConfirmaEmail -->|Corrigir| PerguntaEmail

    ConfirmaAcordo -->|Sim| MsgAprovouParcelas[msg-aprovou-parcelas]
    ConfirmaAcordo -->|Não| MsgNaoAprovouParcel[msg-naoaprovouparcel]

    MsgAprovouParcelas --> MsgInformaIOF[msg-informar-iof]
    MsgInformaIOF --> APIPDFLinha[api-pdf-linhadigita<br/>tipo: 120]

    APIPDFLinha --> MsgEnviaBoleto[msg-envia-boleto]
    MsgEnviaBoleto --> MsgEnviaLinhaDigita[msg-envialinhadigita]
    MsgEnviaLinhaDigita --> MsgFinalizaAcordo[msg-finalizaacordo]

    MsgFinalizaAcordo --> Fim([Fim])

    style APIRecupera fill:#FFD700
    style MsgInicioAcordo fill:#87CEEB
    style ConfirmaAcordo fill:#87CEEB
```

**Mensagem Inicial:**
```
*{{first_name}}*, verifiquei que você tem um débito com mais de 90 dias de atraso.
Que tal fazer um acordo agora?
```

---

### 8. Desbloqueio de Cartão (`desbloqueio`)
**Instruções para desbloqueio via App**

```mermaid
flowchart TD
    Start([Entrada]) --> MsgDesbloqueio[msg-desbloqueio]
    MsgDesbloqueio --> APIDesbloqueio[api-desbloqueio<br/>tipo: 120]

    APIDesbloqueio -->|Bloqueado| MsgDesbloqueio1[msg-desbloqueio1<br/>Instruções]
    APIDesbloqueio -->|Desbloqueado| MsgDesbloqueado[msg-desbloqueado]

    MsgDesbloqueio1 --> MsgDesbloqueio2[msg-desbloqueio2<br/>Passo a passo]
    MsgDesbloqueio2 --> FinalFluxo[Grupo: finalfluxo]

    MsgDesbloqueado --> FinalFluxo

    style APIDesbloqueio fill:#FFD700
```

**Mensagem:**
```
O *desbloqueio* do seu cartão Afinz Visa é feito no nosso App de forma fácil e rápida!
Vou te ajudar! É só seguir as orientações abaixo. 😉
```

---

### 9. NPS - Pesquisa de Satisfação (`nps`)
**Coleta de feedback do cliente**

```mermaid
flowchart TD
    Start([Entrada]) --> NPSNota[nps-nota<br/>tipo: 110<br/>Nota 0-10]

    NPSNota -->|Saída| NotaVerifica[nota-verifica<br/>tipo: 120]
    NPSNota -->|Outros| MsgNota[msg-nota<br/>Digite 0 a 10]

    MsgNota --> NPSNota

    NotaVerifica -->|invalido| MsgNota
    NotaVerifica -->|promotor| MsgPromotor[msg-promotor<br/>9-10]
    NotaVerifica -->|neutro| MsgNeutro[msg-neutro<br/>7-8]
    NotaVerifica -->|detrator| MsgDetrator[msg-detrator<br/>0-6]

    MsgPromotor --> Promotor[promotor<br/>tipo: 110<br/>Feedback]
    MsgNeutro --> Neutro[neutro<br/>tipo: 110<br/>Feedback]
    MsgDetrator --> Detrator[detrator<br/>tipo: 110<br/>Feedback]

    Promotor --> NPSFinal[nps-final]
    Neutro --> NPSFinal
    Detrator --> NPSFinal

    NPSFinal --> ClearsVars[clears-vars<br/>tipo: 120]
    ClearsVars --> FinalDiverso[Grupo: menu-mudo]

    style NPSNota fill:#87CEEB
    style NotaVerifica fill:#FFD700
```

**Classificação NPS:**
| Nota | Classificação | Mensagem |
| --- | --- | --- |
| 9-10 | Promotor | "Uau, adorei!! Fico feliz em te ajudar! 😍" |
| 7-8 | Neutro | "Legal! Com a sua ajuda posso ficar ainda melhor. 😉" |
| 0-6 | Detrator | "Poxa, sinto muito que sua experiência não tenha sido positiva. 😔" |

---

### 10. Saque Já / Empréstimo Emergencial (`saqueja`)
**Fluxo específico para empréstimo emergencial**

```mermaid
flowchart TD
    Start([Entrada]) --> TrativaSaqueJa[trativa-saqueja<br/>tipo: 120]

    TrativaSaqueJa -->|aceito| MsgSaqueJa[Mensagem de boas-vindas<br/>Empréstimo emergencial]
    TrativaSaqueJa -->|nao-aceito| HorarioFunc[Fora do horário]
    TrativaSaqueJa -->|feriados| Feriados[Mensagem de feriado]

    MsgSaqueJa --> ValidaCPF[valida-cpf-func<br/>tipo: 120]

    ValidaCPF -->|Segue| LiveChatAPI[livechat-api-saqueja<br/>tipo: 120]
    ValidaCPF -->|CPF inválido| MsgCPFInvalido[CPF inválido]

    LiveChatAPI -->|Conectado| Atendente[Transfere para<br/>especialista]
    LiveChatAPI -->|Sem atendimentos| MsgTransfLivechat[msg-transf-livechat]

    style TrativaSaqueJa fill:#FFD700
    style LiveChatAPI fill:#FFD700
```

---

## Integrações e APIs

### Endpoints Identificados

| Serviço | URL | Descrição |
| --- | --- | --- |
| Core Services PRD | `https://core-services.afinz.com.br` | Backend principal |
| Core Services DEV | `https://core-services-dev.afinz.com.br` | Ambiente de desenvolvimento |
| Fintalk Chat | `https://fintalkchat.abaionline.com.br` | LiveChat com atendentes |

### APIs por Funcionalidade

#### 1. Login e Autenticação
- **Bloco**: `api-login`
- **Saídas**: OK, NúmeroDiferente, Timeout, Erro
- **Variáveis retornadas**: `user.name`, `vars.firstName`, lista de cartões

#### 2. Validação de CPF
- **Bloco**: `valida-cpf`, `valida-cpf-func`
- **Saídas**: segue, escolhe-cartao, nenhum-cartao, nao-encontrou, timeout, erro
- **Função**: Valida CPF e retorna cartões vinculados

#### 3. Consulta de Fatura
- **Bloco**: `api-consultafatura`
- **Saídas**: escolhe-fatura, sem-fatura, Timeout, erro
- **Dados retornados**: Lista de faturas com valores e vencimentos

#### 4. Busca de PDF/Boleto
- **Saídas**: saida, Timeout, erro
- **Bloco**: `busca-pdf-boleto`, `api-faturapdf`
- **Dados retornados**: URL do PDF, linha digitável

#### 7. Recuperação de Dívidas
- **Bloco**: `api-simulacao`
- **Saídas**: Recupera, oferta proposta ativa, oferta única, oferta, semopcao, semFatura, erro, timeout
- **Dados retornados**: `vars.dadosParcelamento`, `vars.ofertasParcelamento`

#### 6. Efetivação de Parcelamento
- **Bloco**: `api-efetivacao`
- **Saídas**: sucesso, erro
- **Função**: Confirma o parcelamento selecionado

- **Bloco**: `api-recupera`
- **Saídas**: negociação, outro canal, acordo quebrado
- **Função**: Verifica status de negociação

#### 8. Notificações
#### 5. Simulação de Parcelamento
- **Bloco**: `notification`
- **Endpoint**: `/chat_bot/notification/request`
- **Função**: Envio de notificações push

#### 9. LiveChat
- **Saídas**: Conectado, Sem atendimentos
- **Bloco**: `livechat-api`, `livechat-api-saqueja`
- **URL**: `https://fintalkchat.abaionline.com.br`

---

## Variáveis Globais

### Variáveis de Usuário (`user.`)

| Variável | Tipo | Descrição |
| --- | --- | --- |
| `user.name` | String | Nome completo do cliente |
| `user.cpf` | String | CPF do cliente |
| `user.phone` | String | Telefone do cliente |

### Variáveis de Sessão (`vars.`)

| Variável | Tipo | Descrição |
| --- | --- | --- |
| `vars.firstName` | String | Primeiro nome do cliente |
| `vars.cartao` | String | Final do cartão selecionado (4 dígitos) |
| `vars.faturas` | Array | Lista de faturas disponíveis |
| `vars.linhaDigitavel` | String | Código de barras para pagamento |
| `vars.dadosParcelamento` | Object | Dados da simulação de parcelamento |
| `vars.ofertasParcelamento` | Array | Lista de ofertas de parcelamento |
| `vars.opcaoescolhida` | String | Opção de parcelamento escolhida |
| `vars.email` | String | E-mail do cliente (para acordos) |

---

## Informações de Contato

- **Central de Relacionamento**: 4004-2420
- **Site**: afinz.com.br/app
- **Link Cobrança**: https://bit.ly/AfinzCobranca

---

## Observações Técnicas

1. **Timeout Handling**: Todos os blocos de API possuem tratamento de timeout com retry automático
2. **Validação de CPF**: Utiliza função `isValidCPF()` para validação de dígitos
3. **Sessão**: Mantém sessão ativa com verificações periódicas (`verifica-sessao-*`)
4. **Horário de Atendimento**: Crédito Pessoal funciona apenas em dias úteis (8h-19h)
5. **Feriados**: Tratamento especial para Natal e Ano Novo
6. **NPS**: Pesquisa de satisfação ao final de cada fluxo

---

*Documentação gerada a partir do arquivo **`bot-afinz.json`*
*Última atualização: Janeiro 2026*
