# Entidades e Variáveis do Bot C&A

## Entidades de Reconhecimento de Linguagem Natural

### `base-positiva`
Reconhece respostas afirmativas do usuário.

| Valor Principal | Sinônimos |
| --- | --- |
| sim | sim, s, ok, pode, quero, certo, claro, isso, confirmo, correto, perfeito, blz, blza, vamos, aham |

### `base-negativa`
Reconhece respostas negativas do usuário.

| Valor Principal | Sinônimos |
| --- | --- |
| nao | não, n, nunca, negativo, nem, nenhum, corrigir, esquece, jamais |

### `cep-padrao`
Reconhece padrões de CEP brasileiro.

| Padrão | Exemplo |
| --- | --- |
| `[0-9]{8}` | 12345678 |
| `\d{5}\W\d{3}` | 12345-678 |
| `[0-9]{5}\s[0-9]{3}` | 12345 678 |
| `[0-9]{5}-[0-9]{3}` | 12345-678 |

### `ola`
Reconhece saudações iniciais.

| Valor Principal | Sinônimos |
| --- | --- |
| oi | oi, olá, ola, hey, eae, eai |

### `bomdia`
Reconhece saudação de bom dia.

| Valor Principal | Sinônimos |
| --- | --- |
| bom dia | bom dia, bomdia, bdia |

### `boatarde`
Reconhece saudação de boa tarde.

| Valor Principal | Sinônimos |
| --- | --- |
| boa tarde | boa tarde, boatarde, btarde |

### `boanoite`
Reconhece saudação de boa noite.

| Valor Principal | Sinônimos |
| --- | --- |
| boa noite | boa noite, boanoite, bnoite |

### `despedida`
Reconhece despedidas.

| Valor Principal | Sinônimos |
| --- | --- |
| tchau | tchau, até mais, adeus, flw, falou, vlw |

### `xingamentos`
Filtro de palavras ofensivas para moderação de conteúdo.

---

## Variáveis Globais

### Identificação do Cliente

| Variável | Tipo | Descrição |
| --- | --- | --- |
| `vars.cpf` | string | CPF do cliente |
| `vars.nome` | string | Nome do cliente |
| `vars.celular` | string | Celular do cliente |
| `vars.email` | string | Email do cliente |
| `vars.dataNascimento` | string | Data de nascimento |

### Autenticação e Sessão

| Variável | Tipo | Descrição |
| --- | --- | --- |
| `vars.token` | string | Token de autenticação |
| `vars.tokenCreliq` | string | Token C&A Pay |
| `vars.codigoSMS` | string | Código SMS enviado |
| `vars.autenticado` | boolean | Se está autenticado |
| `vars.tentativas` | number | Contador de tentativas |

### Status do Cliente

| Variável | Tipo | Descrição |
| --- | --- | --- |
| `vars.isCreliq` | boolean | Se é cliente C&A Pay |
| `vars.statusConta` | string | Status da conta |
| `vars.temAtraso` | boolean | Se tem parcelas em atraso |
| `vars.temSeguro` | boolean | Se tem seguros ativos |

---

## Variáveis do Módulo Financeiro - Cartão

### Conta e Limite

| Variável | Tipo | Descrição |
| --- | --- | --- |
| `vars.limiteTotal` | number | Limite total do cartão |
| `vars.limiteDisponivel` | number | Limite disponível |
| `vars.limiteUtilizado` | number | Limite utilizado |
| `vars.idConta` | string | ID da conta |

### Fatura

| Variável | Tipo | Descrição |
| --- | --- | --- |
| `vars.faturaAtual` | object | Dados da fatura atual |
| `vars.valorFatura` | number | Valor da fatura |
| `vars.dataVencimento` | string | Data de vencimento |
| `vars.statusFatura` | string | Status da fatura |
| `vars.codigoBarras` | string | Código de barras |
| `vars.linhaDigitavel` | string | Linha digitável |
| `vars.urlPDF` | string | URL do PDF da fatura |

### Compras e Parcelas

| Variável | Tipo | Descrição |
| --- | --- | --- |
| `vars.compras` | array | Lista de compras |
| `vars.idCompra` | string | ID da compra selecionada |
| `vars.valorCompra` | number | Valor da compra |
| `vars.parcelas` | array | Opções de parcelas |
| `vars.parcelaAtual` | number | Parcela atual |
| `vars.qtdParcelas` | number | Quantidade de parcelas |
| `vars.valorParcela` | number | Valor da parcela |

### Antecipação

| Variável | Tipo | Descrição |
| --- | --- | --- |
| `vars.parcelasAntecipacao` | array | Parcelas para antecipar |
| `vars.valorComDesconto` | number | Valor com desconto |
| `vars.percentualDesconto` | number | Percentual de desconto |

---

## Variáveis do Módulo C&A Pay (Creliq)

### Identificação

| Variável | Tipo | Descrição |
| --- | --- | --- |
| `vars.idCliente` | string | ID do cliente Creliq |
| `vars.idContrato` | string | ID do contrato |
| `vars.idParcela` | string | ID da parcela |

### Valores

| Variável | Tipo | Descrição |
| --- | --- | --- |
| `vars.valorTotal` | number | Valor total devido |
| `vars.valorParcelaCreliq` | number | Valor da parcela |
| `vars.qtdParcelasAbertas` | number | Parcelas em aberto |
| `vars.proximoVencimento` | string | Próximo vencimento |

### Acordo

| Variável | Tipo | Descrição |
| --- | --- | --- |
| `vars.acordoSimulado` | object | Dados do acordo simulado |
| `vars.opcoesAcordo` | array | Opções de parcelamento |
| `vars.acordoEfetivado` | object | Acordo efetivado |
| `vars.idAcordo` | string | ID do acordo |
| `vars.desconto` | number | Percentual de desconto |

---

## Variáveis do Módulo Empréstimo

| Variável | Tipo | Descrição |
| --- | --- | --- |
| `vars.elegivel` | boolean | Se é elegível |
| `vars.valorMaximo` | number | Valor máximo disponível |
| `vars.valorEmprestimo` | number | Valor solicitado |
| `vars.taxaJuros` | number | Taxa de juros mensal |
| `vars.cet` | number | Custo Efetivo Total |
| `vars.comSeguro` | boolean | Se inclui seguro |
| `vars.valorSeguroEmprest` | number | Valor do seguro |
| `vars.idCompraEmprestimo` | string | ID da operação |

---

## Variáveis do Módulo Seguros

| Variável | Tipo | Descrição |
| --- | --- | --- |
| `vars.seguros` | array | Lista de seguros |
| `vars.idSeguro` | string | ID do seguro |
| `vars.nomeSeguro` | string | Nome do seguro |
| `vars.statusSeguro` | string | Status do seguro |
| `vars.valorMensalSeguro` | number | Valor mensal |

---

## Variáveis do Módulo Cartão

| Variável | Tipo | Descrição |
| --- | --- | --- |
| `vars.statusCartao` | string | Status do cartão |
| `vars.tipoCartao` | string | Tipo do cartão |
| `vars.ultimosDigitos` | string | Últimos 4 dígitos |

---

## Variáveis de Controle de Fluxo

| Variável | Tipo | Descrição |
| --- | --- | --- |
| `vars.resposta` | string | Última resposta |
| `vars.opcao` | string | Opção selecionada |
| `vars.contador` | number | Contador genérico |
| `vars.erro` | boolean | Se houve erro |
| `vars.mensagemErro` | string | Mensagem de erro |
| `vars.retry` | number | Número de retentativas |
| `vars.fluxoOrigem` | string | Fluxo de origem |

---

## Variáveis de NPS

| Variável | Tipo | Descrição |
| --- | --- | --- |
| `vars.facilidadeUso` | string | Fácil/Difícil |
| `vars.motivoDificuldade` | string | Motivo da dificuldade |
| `vars.comentarioNPS` | string | Comentário livre |
| `vars.npsRespondido` | boolean | Se respondeu NPS |

---

## Variáveis de API Response

| Variável | Tipo | Descrição |
| --- | --- | --- |
| `vars.apiResponse` | object | Resposta da API |
| `vars.apiSuccess` | boolean | Se chamada teve sucesso |
| `vars.apiError` | string | Erro da API |
| `vars.apiData` | object | Dados retornados |

---

## Variáveis de Preditivo

| Variável | Tipo | Descrição |
| --- | --- | --- |
| `vars.isPreditivo` | boolean | Se é atendimento preditivo |
| `vars.tipoPreditivo` | string | Tipo de predição |
| `vars.dadosPreditivo` | object | Dados do preditivo |

---

## Constantes do Sistema

| Constante | Valor | Descrição |
| --- | --- | --- |
| `agent` | cea | Identificador do agente |
| `botName` | cea-prd | Nome do bot |
| `av-ambiente` | prd | Ambiente |
| `av-stage` | prd | Stage |

---

## Access Keys

| Chave | Descrição |
| --- | --- |
| `api-hub` | URL API principal |
| `api-hub-creliq` | URL API C&A Pay |
| `api-key-sim` | Chave API SIM |
| `Auth-Creliq` | Autenticação Creliq |
| `av-api` | URL Proxy URA |
| `av-Cookie` | Cookie de sessão |
| `av-client-encoded` | Client encoded |

---

## Próximo: [07-fluxograma-geral.md](./07-fluxograma-geral.md)
