# Entidades e Variáveis do Bot Alloha

## Entidades de Reconhecimento de Linguagem Natural

### `base-positiva`
Reconhece respostas afirmativas do usuário.

| Valor Principal | Sinônimos |
| --- | --- |
| sim | sim, S, SI, si, acertou, aham, ahaam, as vezes, blz, blza, certo, claro, isso, confere, confirmo, correto, exato, iso, isso, obvio, óbvio, ok, otimo, perfeito, perfeitu, pode, pode ser, s, sempre, sí, sss, ss, simm, simmm, é, quero, 👍, vamos, vamu |

### `base-negativa`
Reconhece respostas negativas do usuário.

| Valor Principal | Sinônimos |
| --- | --- |
| nao | nao, não, NãO, NÃO, Não, N, n, No, errou, esquece, jaamais, jamais, na, naumm, necas, negativo, nehativo, nem, nenhum, nenhuma, nn, nnn, nnnnn, nop, mops, nunca, nunquinha, corrigir, Corrigir, Nao |

### `cep-padrao`
Reconhece padrões de CEP brasileiro.

| Padrão | Exemplo |
| --- | --- |
| `[0-9]{8}` | 12345678 |
| `\d{5}\W\d{3}` | 12345-678 |
| `[0-9]{5}\s[0-9]{3}` | 12345 678 |
| `[0-9]{5}-[0-9]{3}` | 12345-678 |

### `sys_session`
Entidade de controle de sessão do sistema.

---

## Variáveis Globais

### Identificação do Cliente

| Variável | Tipo | Descrição |
| --- | --- | --- |
| `vars.cpf` | string | CPF do cliente |
| `vars.cpf_cnpj` | string | CPF ou CNPJ do cliente |
| `vars.customer_id` | string | ID único do cliente |
| `vars.contract_number` | string | Número do contrato ativo |
| `vars.contrato` | string | Alias para contract_number |
| `vars.phone` | string | Telefone do cliente |
| `vars.nome` | string | Nome do cliente |
| `vars.email` | string | Email do cliente |

### Autenticação e Sessão

| Variável | Tipo | Descrição |
| --- | --- | --- |
| `vars.token` | string | Token de autenticação |
| `vars.auth` | string | Token de sessão |
| `vars.protocol` | string | Protocolo de atendimento |
| `vars.session_id` | string | ID da sessão |
| `vars.validado` | boolean | Se cliente foi validado |

### Status do Cliente

| Variável | Tipo | Descrição |
| --- | --- | --- |
| `vars.status` | string | Status do contrato |
| `vars.suspenso` | boolean | Se está suspenso |
| `vars.ex_cliente` | boolean | Se é ex-cliente |
| `vars.em_massiva` | boolean | Se está em incidente massivo |
| `vars.blacklist` | boolean | Se está na blacklist |
| `vars.reincidente` | boolean | Se é cliente reincidente |

### Contexto de Atendimento

| Variável | Tipo | Descrição |
| --- | --- | --- |
| `vars.fluxo_origem` | string | Fluxo de origem |
| `vars.intencao` | string | Intenção identificada |
| `vars.csat_respondido` | boolean | Se respondeu CSAT |
| `vars.atendido_humano` | boolean | Se foi atendido por humano |
| `vars.tentativas` | number | Contador de tentativas |
| `vars.fila` | string | Fila de atendimento |

---

## Variáveis do Módulo Financeiro

| Variável | Tipo | Descrição |
| --- | --- | --- |
| `vars.faturas` | array | Lista de faturas |
| `vars.fatura_selecionada` | object | Fatura escolhida |
| `vars.valor_total` | number | Valor total devido |
| `vars.vencimento` | string | Data de vencimento |
| `vars.pix_code` | string | Código PIX |
| `vars.boleto_url` | string | URL do boleto |
| `vars.forma_pagamento` | string | PIX ou Boleto |

### Variáveis de Negociação

| Variável | Tipo | Descrição |
| --- | --- | --- |
| `vars.elegivel_negociacao` | boolean | Se pode negociar |
| `vars.propostas` | array | Propostas disponíveis |
| `vars.proposta_selecionada` | object | Proposta escolhida |
| `vars.id_negotiation` | string | ID da negociação |
| `vars.id_parcel_group` | string | ID do grupo de parcelas |
| `vars.parcelas` | number | Número de parcelas |
| `vars.acordo_id` | string | ID do acordo formalizado |

---

## Variáveis do Módulo Suporte Técnico

| Variável | Tipo | Descrição |
| --- | --- | --- |
| `vars.connection_status` | string | Status da conexão |
| `vars.online` | boolean | Se está online |
| `vars.velocidade` | string | Velocidade medida |

### Variáveis de Ordem de Serviço

| Variável | Tipo | Descrição |
| --- | --- | --- |
| `vars.os_list` | array | Lista de OS |
| `vars.os_selecionada` | object | OS escolhida |
| `vars.os_id` | string | ID da OS |
| `vars.data_agendamento` | string | Data agendada |
| `vars.periodo` | string | Período (manhã/tarde) |
| `vars.id_motive` | string | ID do motivo |
| `vars.id_service` | string | ID do serviço |
| `vars.area_code` | string | Código da área |
| `vars.workzone_code` | string | Código da zona |
| `vars.bucket` | string | Bucket de agendamento |
| `vars.booking_code` | string | Código de reserva |

### Variáveis WiFi

| Variável | Tipo | Descrição |
| --- | --- | --- |
| `vars.wifi_nome` | string | Nome da rede |
| `vars.wifi_senha` | string | Senha WiFi |
| `vars.wifi_serial` | string | Serial do equipamento |
| `vars.wifi_type` | string | Tipo de rede (2.4/5GHz) |

---

## Variáveis de Endereço

| Variável | Tipo | Descrição |
| --- | --- | --- |
| `vars.cep` | string | CEP |
| `vars.rua` | string | Logradouro |
| `vars.numero` | string | Número |
| `vars.complemento` | string | Complemento |
| `vars.bairro` | string | Bairro |
| `vars.cidade` | string | Cidade |
| `vars.estado` | string | Estado |
| `vars.ibge_city_code` | string | Código IBGE |
| `vars.condominio` | boolean | Se é condomínio |
| `vars.condominio_id` | string | ID do condomínio |
| `vars.condominium_full_code` | string | Código completo |
| `vars.condominium_block_full_code` | string | Código do bloco |

---

## Variáveis de Planos e Produtos

| Variável | Tipo | Descrição |
| --- | --- | --- |
| `vars.plano_atual` | object | Plano atual |
| `vars.planos_disponiveis` | array | Planos disponíveis |
| `vars.plano_selecionado` | object | Plano escolhido |
| `vars.plan_code` | string | Código do plano |
| `vars.plan_name` | string | Nome do plano |
| `vars.campaign_code` | string | Código da campanha |

### Variáveis OTT

| Variável | Tipo | Descrição |
| --- | --- | --- |
| `vars.ott_id` | string | ID do produto OTT |
| `vars.ott_provider` | string | Provedor (Globoplay, Max) |
| `vars.ott_status` | string | Status do OTT |
| `vars.produtos` | array | Produtos disponíveis |

---

## Variáveis de Cancelamento

| Variável | Tipo | Descrição |
| --- | --- | --- |
| `vars.motivo_cancelamento` | string | Razão do cancelamento |
| `vars.oferta_retencao` | object | Oferta de retenção |
| `vars.aceita_oferta` | boolean | Se aceitou oferta |
| `vars.confirmacao_cancel` | boolean | Confirmação final |

---

## Variáveis de Controle de Fluxo

| Variável | Tipo | Descrição |
| --- | --- | --- |
| `vars.resposta` | string | Última resposta do cliente |
| `vars.opcao` | string | Opção selecionada |
| `vars.contador` | number | Contador genérico |
| `vars.erro` | boolean | Se houve erro |
| `vars.mensagem_erro` | string | Mensagem de erro |
| `vars.retry` | number | Número de retentativas |

---

## Variáveis de API Response

| Variável | Tipo | Descrição |
| --- | --- | --- |
| `vars.api_response` | object | Resposta da API |
| `vars.api_success` | boolean | Se chamada teve sucesso |
| `vars.api_error` | string | Erro da API |
| `vars.api_data` | object | Dados retornados |

---

## Variáveis de Massiva

| Variável | Tipo | Descrição |
| --- | --- | --- |
| `vars.massiva` | object | Dados da massiva |
| `vars.massiva_id` | string | ID do incidente |
| `vars.massiva_previsao` | string | Previsão de resolução |
| `vars.massiva_msg` | string | Mensagem da massiva |

---

## Variáveis de NPS

| Variável | Tipo | Descrição |
| --- | --- | --- |
| `vars.nps_id` | string | ID da pesquisa |
| `vars.nps_nota` | number | Nota do NPS |
| `vars.nps_comentario` | string | Comentário do cliente |
| `vars.nps_respondido` | boolean | Se respondeu NPS |

---

## Variáveis de Token/Validação

| Variável | Tipo | Descrição |
| --- | --- | --- |
| `vars.token_sms` | string | Token enviado |
| `vars.token_digitado` | string | Token informado |
| `vars.telefone_validacao` | string | Telefone para validação |
| `vars.tipo_envio` | string | SMS ou WhatsApp |
| `vars.token_validado` | boolean | Se token é válido |

---

## Variáveis de Histórico (LLM)

| Variável | Tipo | Descrição |
| --- | --- | --- |
| `vars.history` | array | Histórico de conversa |
| `vars.pergunta` | string | Última pergunta |
| `vars.buttons` | array | Botões disponíveis |
| `vars.template` | string | Template de prompt |

---

## Constantes do Sistema

| Constante | Valor | Descrição |
| --- | --- | --- |
| `n8n-url` | `https://n8n-prd-webhook.fintalk.io/webhook/2.0/prd` | URL base n8n |
| `stage` | `prd` | Ambiente |
| `logout` | `3600000` | Timeout de sessão (ms) |

---

## Próximo: [07-filas-atendimento.md](./07-filas-atendimento.md)
