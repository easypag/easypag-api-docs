---
title: Referência API

language_tabs: # must be one of https://git.io/vQNgJ
  - json

toc_footers:

includes:
  - errors

search: true
---

# Introdução

## API EasyPag v1-Beta

Esta é a documentação para a API do EasyPag. Atualmente, a nossa API encontra-se em versão beta, pois estamos constantemente monitorando e acompanhando o uso da API por nossos clientes. A API está sujeita às mudanças e melhorias, e esperamos atingir em breve a versão estável.

# Autenticação

## Basic

Todos os endpoints da API são autenticados via Basic Auth.

<aside class="notice">
É obrigatório a utilização de https em todos os endpoints. 
</aside>

### Parâmetros Basic

| Parâmetro | Descrição                               | Obrigatório |
| --------- | --------------------------------------- | ----------- |
| Username  | O Client ID da sua conta de usuário     | Sim         |
| Password  | O Client Secret da sua conta de usuário | Sim         |

# Webhooks

## Introdução

Os eventos são disparados via `POST` na url especificada na criação do webhook. Somente urls que utilizem `https` serão aceitas.

## Eventos disponíveis

| Evento                | Descrição                                                                                                              |
| --------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| INVOICE.\*            | Todos os eventos das cobranças                                                                                         |
| INVOICE.CREATED       | Notifica caso uma cobrança seja criada.                                                                                |
| INVOICE.STATUS_UPDATE | Notifica caso o status de pagamento de uma cobrança seja alterado para qualquer outro status.                          |
| INVOICE.PAID          | Notifica caso o status de pagamento de uma cobrança seja alterado para `2`, que equivale a cobrança paga.              |
| INVOICE.SETTLED       | Notifica caso o status de pagamento de uma cobrança seja alterado para `3`, que equivale a cobrança marcada como paga. |
| INVOICE.DUE           | Notifica caso a cobrança esteja em sua data de vencimento.                                                             |
| INVOICE.OVERDUE       | Notifica caso a cobrança esteja vencida.                                                                               |
| INVOICE.CANCELLED     | Notifica caso o status de pagamento de uma cobrança seja alterado para `5`, que equivale a cobrança cancelada.         |
| INVOICE.EXPIRED       | Notifica caso o status de pagamento de uma cobrança seja alterado para `6`, que equivale a cobrança expirada.          |

## Exemplo de evento recebido

Confira o exemplo ao lado de notificação recebida via POST.

```http
HTTP/1.1 200 Ok
Authorization: token-auth-exemplo
Content-Length: 1555
Accept: application/json, text/plain, */*
Connection: close
Content-Type: application/json;charset=utf-8
Host: requestbin.fullcontact.com
```

```json
{
  "id": "af082812-baf4-4857-aa03-1c11ae7822ab",
  "event": "INVOICE.CREATED",
  "resource": {
    "id": "24852ff8-a068-4c65-96e8-936163754768",
    "reference": null,
    "dueDate": "2019-06-30",
    "amount": 2000,
    "paidAmount": 0,
    "currency": "BRL",
    "items": [
      {
        "id": "3fbaa249-c912-45f3-b71f-5ce107ce4f76",
        "description": "Item de Test-2",
        "quantity": 1,
        "price": 1000,
        "createdAt": "2019-06-17T18:06:07.000Z",
        "updatedAt": "2019-06-17T18:06:07.000Z",
        "invoiceId": "24852ff8-a068-4c65-96e8-936163754768"
      },
      {
        "id": "edc403dc-c62e-428c-a631-b72d90d93904",
        "description": "Item de Test-1",
        "quantity": 1,
        "price": 1000,
        "createdAt": "2019-06-17T18:06:07.000Z",
        "updatedAt": "2019-06-17T18:06:07.000Z",
        "invoiceId": "24852ff8-a068-4c65-96e8-936163754768"
      }
    ],
    "customer": {
      "name": "Cliente de teste",
      "email": "cliente@email.com",
      "phoneNumber": "55999999999",
      "doc": {
        "type": "cpf",
        "number": "70524966079"
      },
      "address": {
        "cep": "60999999",
        "uf": "SP",
        "city": "Sao Paulo",
        "area": "Bairro",
        "addressLine1": "Rua",
        "addressLine2": "Complemento",
        "streetNumber": "9999"
      }
    },
    "notifications": {
      "sendOnCreate": false,
      "reminders": {
        "cc": {
          "emails": []
        },
        "before": {
          "send": false,
          "days": 3,
          "time": "10:00:00"
        },
        "after": {
          "send": false,
          "days": "5",
          "time": "11:00:00"
        },
        "expiration": {
          "send": false,
          "time": "12:00:00"
        },
        "overdue": {
          "send": false
        }
      },
      "types": {
        "email": true,
        "sms": false,
        "whatsapp": false
      }
    },
    "instructionsMsg": "Teste de Instrução",
    "notes": "Teste de Observação",
    "invoiceUrl": "http://faturas.sandbox.easypagamentos.com.br/af082812-baf4-4857-aa03-1c11ae7822ab",
    "payment": {
      "status": "1",
      "description": "Aguardando pagamento"
    },
    "createdAt": "2019-06-17T18:06:07.000Z",
    "updatedAt": "2019-06-17T18:06:07.000Z"
  }
}
```

## Listar todos os webhooks

> Resposta

```http
HTTP/1.1 200 Ok
```

```json
[
  {
    "id": "8fe98a37-7f31-42bf-9e78-41b8aee8f050",
    "targetUrl": "https://requestbin.fullcontact.com/uds0eyud",
    "event": "INVOICE.*",
    "authKey": "token-auth-exemplo",
    "createdAt": "2019-06-17T17:07:51.000Z",
    "updatedAt": "2019-06-17T17:07:51.000Z"
  }
]
```

Este endpoint lista todos os webhooks.

### Requisição HTTP

`GET https://sandbox.easypagamentos.com.br/api/v1/webhooks`

## Listar um webhook

> Resposta

```http
HTTP/1.1 200 Ok
```

```json
{
  "id": "8fe98a37-7f31-42bf-9e78-41b8aee8f050",
  "targetUrl": "https://requestbin.fullcontact.com/uds0eyud",
  "event": "INVOICE.*",
  "authKey": "token-auth-exemplo",
  "createdAt": "2019-06-17T17:07:51.000Z",
  "updatedAt": "2019-06-17T17:07:51.000Z"
}
```

Este endpoint lista um webhook especificado.

### Requisição HTTP

`GET https://sandbox.easypagamentos.com.br/api/v1/webhooks/:id`

### Parâmetros URL

| Parâmetro | Descrição       | Obrigatório |
| --------- | --------------- | ----------- |
| ID        | O ID do webhook | Sim         |

## Criar um webhook

> Requisição

```json
{
  "targetUrl": "https://requestbin.fullcontact.com/uds0eyud",
  "authKey": "token-auth-exemplo",
  "event": "INVOICE.*"
}
```

> Resposta

```http
HTTP/1.1 201 Created
```

```json
{
  "id": "8fe98a37-7f31-42bf-9e78-41b8aee8f050",
  "targetUrl": "https://requestbin.fullcontact.com/uds0eyud",
  "event": "INVOICE.*",
  "authKey": "token-auth-exemplo",
  "createdAt": "2019-06-17T17:07:51.000Z",
  "updatedAt": "2019-06-17T17:07:51.000Z"
}
```

Este endpoint cria um webhook.

### Requisição HTTP

`POST https://sandbox.easypagamentos.com.br/api/v1/webhooks`

### Parâmetros Body

| Parâmetro | Descrição                                                                                                                                                                                                           | Tipo   | Obrigatório |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ | ----------- |
| targetUrl | Url de notficação. Deve ser `https`.                                                                                                                                                                                | String | Sim         |
| event     | Evento de notficação. Deve ser um dos valores: `INVOICE.*`, `INVOICE.CREATED`, `INVOICE.STATUS_UPDATE`, `INVOICE.PAID`, `INVOICE.SETTLED`, `INVOICE.DUE`, `INVOICE.OVERDUE`, `INVOICE.CANCELLED`, `INVOICE.EXPIRED` | String | Sim         |
| authKey   | Chave de autenticação que será enviada no header da notificação                                                                                                                                                     | String | Sim         |

## Editar um webhook

> Requisição

```json
{
  "targetUrl": "https://requestbin.fullcontact.com/1ieouo21"
}
```

> Resposta

```http
HTTP/1.1 200 Ok
```

```json
{
  "id": "8fe98a37-7f31-42bf-9e78-41b8aee8f050",
  "targetUrl": "https://requestbin.fullcontact.com/1ieouo21",
  "event": "INVOICE.*",
  "authKey": "token-auth-exemplo",
  "createdAt": "2019-06-17T17:07:51.000Z",
  "updatedAt": "2019-06-17T17:09:00.000Z"
}
```

Este endpoint edita um webhook.

### Requisição HTTP

`PUT https://sandbox.easypagamentos.com.br/api/v1/webhooks/:id`

### Parâmetros URL

| Parâmetro | Descrição       | Obrigatório |
| --------- | --------------- | ----------- |
| ID        | O ID do webhook | Sim         |

<aside class="notice">
Você pode informar apenas os parâmetros que deseja atualizar.
</aside>

### Parâmetros Body

| Parâmetro | Descrição                                                                                                                                                                                                           | Tipo   | Obrigatório |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ | ----------- |
| targetUrl | Url de notficação. Deve ser `https`.                                                                                                                                                                                | String | Não         |
| event     | Evento de notficação. Deve ser um dos valores: `INVOICE.*`, `INVOICE.CREATED`, `INVOICE.STATUS_UPDATE`, `INVOICE.PAID`, `INVOICE.SETTLED`, `INVOICE.DUE`, `INVOICE.OVERDUE`, `INVOICE.CANCELLED`, `INVOICE.EXPIRED` | String | Não         |
| authKey   | Chave de autenticação que será enviada no header da notificação                                                                                                                                                     | String | Não         |

## Deletar um webhook

> Resposta

```http
HTTP/1.1 204 No Content
```

Este endpoint deleta um webhook especificado.

### Requisição HTTP

`DELETE https://sandbox.easypagamentos.com.br/api/v1/webhooks/:id`

### Parâmetros URL

| Parâmetro | Descrição       | Obrigatório |
| --------- | --------------- | ----------- |
| ID        | O ID do webhook | Sim         |

# Clientes

## Listar todos os clientes

> Resposta

```http
HTTP/1.1 200 Ok
```

```json
{
  "page": 1,
  "limit": 100,
  "pagesTotal": 1,
  "count": 2,
  "results": [
    {
      "id": "bb3fef04-64d9-429e-820f-49efca3b9c35",
      "name": "João da Silva",
      "reference": "ref0002",
      "email": "joao@email.com",
      "phoneNumber": "5511988515697",
      "doc": {
        "type": "cpf",
        "number": "29458917000"
      },
      "address": {
        "cep": "60135222",
        "uf": "CE",
        "city": "Fortaleza",
        "area": "Dionísio Torres",
        "addressLine1": "Rua Marcondes Pereira",
        "addressLine2": "Prox dionísio Torres",
        "streetNumber": "1381"
      },
      "createdAt": "2019-06-20T14:25:58.000Z",
      "updatedAt": "2019-06-20T14:25:58.000Z"
    },
    {
      "id": "46dfd9f0-0fe7-460f-8631-0cc30cb05b8a",
      "name": "Maria Assessoria Jurídica Ltda",
      "reference": "ref0001",
      "email": "maria@email.com",
      "phoneNumber": "551625392136",
      "doc": {
        "type": "cnpj",
        "number": "70384052000194"
      },
      "address": {
        "cep": "29113740",
        "uf": "ES",
        "city": "Vila Velha",
        "area": "Santa Clara",
        "addressLine1": "Rua Santa Lúcia",
        "addressLine2": null,
        "streetNumber": "9999"
      },
      "createdAt": "2019-06-10T21:55:40.000Z",
      "updatedAt": "2019-06-10T21:55:40.000Z"
    }
  ]
}
```

Este endpoint lista todos os clientes.

### Requisição HTTP

`GET https://sandbox.easypagamentos.com.br/api/v1/customers`

### Parâmetros Query

| Parâmetro     | Descrição                                                                                                                                                                                                 | Obrigatório |
| ------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------- |
| page          | Número da página a ser retornada                                                                                                                                                                          | Não         |
| limit         | Número itens a ser retornado por página. Valor mínimo: 1, valor máximo: 100. Caso não seja infromado, será aplicado o valor default de 100 itens                                                          | Não         |
| search        | Parâmetro que recebe texto e efetua pesquisa nos campos de nome, email e referência do cliente                                                                                                            | Não         |
| docNumber     | Parâmetro que busca cliente a partir do cpf/cnpj. Pode ser informado com ou sem máscara.                                                                                                                  | Não         |
| sortBy        | Um hash contendo na chave, o nome do campo para ordenação e o valor DESC ou ASC para descendente e ascendente, respectivamente. Exemplos: sortBy[name]=ASC, sortBy[createdAt]=ASC, sortBy[updatedAt]=DESC | Não         |
| createdAtFrom | Filtra registros criados a partir da data passada no parâmetro. Formato (AAAA-MM-DDThh:mm:ss)                                                                                                             | Não         |
| createdAtTo   | Filtra registros criados até esta data passada no parâmetro. Formato (AAAA-MM-DDThh:mm:ss)                                                                                                                | Não         |
| updatedSince  | Filtra registros atualizados desde o valor passado no parâmetro. Formato (AAAA-MM-DDThh:mm:ss)                                                                                                            | Não         |

## Listar um cliente

> Resposta

```http
HTTP/1.1 200 Ok
```

```json
{
  "id": "46dfd9f0-0fe7-460f-8631-0cc30cb05b8a",
  "name": "Maria Assessoria Jurídica Ltda",
  "reference": "ref0001",
  "email": "maria@email.com",
  "phoneNumber": "551625392136",
  "doc": {
    "type": "cnpj",
    "number": "70384052000194"
  },
  "address": {
    "cep": "29113740",
    "uf": "ES",
    "city": "Vila Velha",
    "area": "Santa Clara",
    "addressLine1": "Rua Santa Lúcia",
    "addressLine2": null,
    "streetNumber": "9999"
  },
  "createdAt": "2019-06-10T21:55:40.000Z",
  "updatedAt": "2019-06-10T21:55:40.000Z"
}
```

Este endpoint lista um cliente especificado.

### Requisição HTTP

`GET https://sandbox.easypagamentos.com.br/api/v1/customers/:id`

### Parâmetros URL

| Parâmetro | Descrição       | Obrigatório |
| --------- | --------------- | ----------- |
| ID        | O ID do cliente | Sim         |

## Criar um cliente

> Requisição

```json
{
  "name": "João da Silva",
  "reference": "ref0002",
  "email": "joao@email.com",
  "phoneNumber": "5511988515697",
  "docNumber": "29458917000",
  "address": {
    "cep": "60135222",
    "uf": "CE",
    "city": "Fortaleza",
    "area": "Dionísio Torres",
    "addressLine1": "Rua Marcondes Pereira",
    "addressLine2": "Prox dionísio Torres",
    "streetNumber": "1381"
  }
}
```

> Resposta

```http
HTTP/1.1 201 Created
```

```json
{
  "id": "acb900d5-0007-4823-93c2-10d5694a2d31",
  "name": "João da Silva",
  "reference": "ref0002",
  "email": "joao@email.com",
  "phoneNumber": "5585996791915",
  "doc": {
    "type": "cpf",
    "number": "29458917000"
  },
  "address": {
    "cep": "60135222",
    "uf": "CE",
    "city": "Fortaleza",
    "area": "Dionísio Torres",
    "addressLine1": "Rua Marcondes Pereira",
    "addressLine2": "Prox dionísio Torres",
    "streetNumber": "1381"
  },
  "createdAt": "2019-06-22T14:11:51.246Z",
  "updatedAt": "2019-06-22T14:11:51.246Z"
}
```

Este endpoint cria um cliente.

### Requisição HTTP

`POST https://sandbox.easypagamentos.com.br/api/v1/customers`

### Parâmetros Body

| Parâmetro            | Descrição                                                                                                                                    | Tipo   | Obrigatório |
| -------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- | ------ | ----------- |
| name                 | O nome do cliente                                                                                                                            | String | Sim         |
| reference            | Referência externa do cliente                                                                                                                | String | Não         |
| email                | Endereço de email do cliente                                                                                                                 | String | Sim         |
| phoneNumber          | Número de telefone do cliente. Deve seguir o seguinte formato: `55`, seguido do número DDD, seguido do telefone, por exemplo: `551199999999` | String | Sim         |
| docNumber            | Número de CPF/CNPJ do cliente. Pode ser informado com ou sem máscara                                                                         | String | Sim         |
| address              | Objeto contendo informações de endereço do cliente                                                                                           | Object | Sim         |
| address.cep          | Número do Cep do endereço. Pode ser com ou sem máscara                                                                                       | String | Sim         |
| address.uf           | Sigle UF do endereço                                                                                                                         | String | Sim         |
| address.city         | Cidade do endereço                                                                                                                           | String | Sim         |
| address.area         | Bairro do endereço                                                                                                                           | String | Sim         |
| address.addressLine1 | Logradouro do endereço                                                                                                                       | String | Sim         |
| address.addressLine2 | Complemento do endereço                                                                                                                      | String | Não         |
| address.streetNumber | Número do logradouro                                                                                                                         | String | Não         |

## Editar um cliente

> Requisição

```json
{
  "email": "joao2@email.com",
  "phoneNumber": "5511888888888"
}
```

> Resposta

```http
HTTP/1.1 200 Ok
```

```json
{
  "id": "acb900d5-0007-4823-93c2-10d5694a2d31",
  "name": "João da Silva",
  "reference": "ref0002",
  "email": "joao2@email.com",
  "phoneNumber": "5511888888888",
  "doc": {
    "type": "cpf",
    "number": "29458917000"
  },
  "address": {
    "cep": "60135222",
    "uf": "CE",
    "city": "Fortaleza",
    "area": "Dionísio Torres",
    "addressLine1": "Rua Marcondes Pereira",
    "addressLine2": "Prox dionísio Torres",
    "streetNumber": "1381"
  },
  "createdAt": "2019-06-22T14:11:51.000Z",
  "updatedAt": "2019-06-22T14:15:42.000Z"
}
```

Este endpoint edita um cliente.

### Requisição HTTP

`PUT https://sandbox.easypagamentos.com.br/api/v1/customers/:id`

### Parâmetros URL

| Parâmetro | Descrição       | Obrigatório |
| --------- | --------------- | ----------- |
| ID        | O ID do cliente | Sim         |

<aside class="notice">
Você pode informar apenas os parâmetros que deseja atualizar.
</aside>

### Parâmetros Body

| Parâmetro            | Descrição                                                                                                                                    | Tipo   | Obrigatório |
| -------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- | ------ | ----------- |
| name                 | O nome do cliente                                                                                                                            | String | Não         |
| reference            | Referência externa do cliente                                                                                                                | String | Não         |
| email                | Endereço de email do cliente                                                                                                                 | String | Não         |
| phoneNumber          | Número de telefone do cliente. Deve seguir o seguinte formato: `55`, seguido do número DDD, seguido do telefone, por exemplo: `551199999999` | String | Não         |
| docNumber            | Número de CPF/CNPJ do cliente. Pode ser informado com ou sem máscara                                                                         | String | Não         |
| address              | Objeto contendo informações de endereço do cliente                                                                                           | Object | Não         |
| address.cep          | Número do Cep do endereço. Pode ser com ou sem máscara                                                                                       | String | Não         |
| address.uf           | Sigle UF do endereço                                                                                                                         | String | Não         |
| address.city         | Cidade do endereço                                                                                                                           | String | Não         |
| address.area         | Bairro do endereço                                                                                                                           | String | Não         |
| address.addressLine1 | Logradouro do endereço                                                                                                                       | String | Não         |
| address.addressLine2 | Complemento do endereço                                                                                                                      | String | Não         |
| address.streetNumber | Número do logradouro                                                                                                                         | String | Não         |

## Deletar um cliente

> Resposta

```http
HTTP/1.1 204 No Content
```

Este endpoint deleta um cliente especificado.

### Requisição HTTP

`DELETE https://sandbox.easypagamentos.com.br/api/v1/customers/:id`

### Parâmetros URL

| Parâmetro | Descrição       | Obrigatório |
| --------- | --------------- | ----------- |
| ID        | O ID do cliente | Sim         |

# Cobranças

## Status de Cobrança

O parâmetro `payment.status`, retornado no objeto de resposta, contém o status do pagamento da cobrança.

| Status | Descrição                              |
| ------ | -------------------------------------- |
| 1      | Aguardando pagamento                   |
| 2      | Cobrança paga                          |
| 3      | Cobrança manualmente marcada como paga |
| 4      | Cobrança atrasada                      |
| 5      | Cobrança cancelada pelo usuário        |
| 6      | Cobrança expirada                      |

## Listar todas as cobranças

> Resposta

```http
HTTP/1.1 200 Ok
```

```json
{
  "page": 1,
  "limit": 100,
  "pagesTotal": 1,
  "count": 2,
  "summary": {
    "amount": 4999,
    "paidAmount": 0
  },
  "results": [
    {
      "id": "d3a15b2f-2f34-4f8d-a8b2-089d4a073ad2",
      "reference": null,
      "dueDate": "2019-07-06",
      "amount": 2999,
      "paidAmount": 0,
      "paidAt": null,
      "currency": "BRL",
      "items": [
        {
          "id": "432512f6-6556-4bf6-ac4c-05d9f11d66de",
          "description": "Item de Test-2",
          "quantity": 1,
          "price": 2000,
          "createdAt": "2019-06-22T14:48:29.000Z",
          "updatedAt": "2019-06-22T14:48:29.000Z",
          "invoiceId": "d3a15b2f-2f34-4f8d-a8b2-089d4a073ad2"
        },
        {
          "id": "b79b78ff-14a5-41bf-a32b-4613608a9de6",
          "description": "Item de Test-1",
          "quantity": 1,
          "price": 999,
          "createdAt": "2019-06-22T14:48:29.000Z",
          "updatedAt": "2019-06-22T14:48:29.000Z",
          "invoiceId": "d3a15b2f-2f34-4f8d-a8b2-089d4a073ad2"
        }
      ],
      "earlyDiscount": {
        "percentage": 10,
        "earlyDays": 5
      },
      "interest": {
        "percentage": 1
      },
      "fine": {
        "lateDays": 3,
        "fineStartDate": "2019-07-09",
        "percentage": 2
      },
      "customer": {
        "name": "João da Silva",
        "email": "joao2@email.com",
        "phoneNumber": "5511888888888",
        "doc": {
          "type": "cpf",
          "number": "29458917000"
        },
        "address": {
          "cep": "60135222",
          "uf": "CE",
          "city": "Fortaleza",
          "area": "Dionísio Torres",
          "addressLine1": "Rua Marcondes Pereira",
          "addressLine2": "Prox dionísio Torres",
          "streetNumber": "1381"
        }
      },
      "notifications": {
        "sendOnCreate": false,
        "reminders": {
          "cc": {
            "emails": []
          },
          "before": {
            "send": false,
            "days": 3,
            "time": "10:00:00"
          },
          "after": {
            "send": false,
            "days": "5",
            "time": "11:00:00"
          },
          "expiration": {
            "send": false,
            "time": "12:00:00"
          },
          "overdue": {
            "send": false
          }
        },
        "types": {
          "email": true,
          "sms": false,
          "whatsapp": false
        }
      },
      "instructionsMsg": "Teste de Instrução",
      "notes": "Teste de Observação",
      "invoiceUrl": "https://sandbox.easypagamentos.com.br/faturas",
      "payment": {
        "status": "1",
        "description": "Aguardando pagamento"
      },
      "events": [
        {
          "id": "bca8a3ed-54d0-49a7-8d12-bf66b3c7d8dd",
          "type": "INVOICE.CREATED",
          "createdAt": "2019-06-22T14:48:29.000Z",
          "updatedAt": "2019-06-22T14:48:29.000Z",
          "invoiceId": "d3a15b2f-2f34-4f8d-a8b2-089d4a073ad2"
        }
      ],
      "createdAt": "2019-06-22T14:48:29.000Z",
      "updatedAt": "2019-06-22T14:48:29.000Z"
    },
    {
      "id": "74691e6e-ceda-4e2f-88a3-1c08b2c86484",
      "reference": null,
      "dueDate": "2019-06-30",
      "amount": 2000,
      "paidAmount": 0,
      "paidAt": null,
      "currency": "BRL",
      "items": [
        {
          "id": "25905bfc-2e6e-4b26-a944-9995de6332e5",
          "description": "Item de Test-1",
          "quantity": 1,
          "price": 1000,
          "createdAt": "2019-06-22T14:39:12.000Z",
          "updatedAt": "2019-06-22T14:39:12.000Z",
          "invoiceId": "74691e6e-ceda-4e2f-88a3-1c08b2c86484"
        },
        {
          "id": "431f9699-e5e6-4b2a-bfa0-fba829cb09c5",
          "description": "Item de Test-2",
          "quantity": 1,
          "price": 1000,
          "createdAt": "2019-06-22T14:39:12.000Z",
          "updatedAt": "2019-06-22T14:39:12.000Z",
          "invoiceId": "74691e6e-ceda-4e2f-88a3-1c08b2c86484"
        }
      ],
      "customer": {
        "name": "Maria Assessoria Jurídica Ltda",
        "email": "maria@email.com",
        "phoneNumber": "551625392136",
        "doc": {
          "type": "cnpj",
          "number": "70384052000194"
        },
        "address": {
          "cep": "29113740",
          "uf": "ES",
          "city": "Vila Velha",
          "area": "Santa Clara",
          "addressLine1": "Rua Santa Lúcia",
          "streetNumber": "9999"
        }
      },
      "notifications": {
        "sendOnCreate": false,
        "reminders": {
          "cc": {
            "emails": []
          },
          "before": {
            "send": false,
            "days": 3,
            "time": "10:00:00"
          },
          "after": {
            "send": false,
            "days": "5",
            "time": "11:00:00"
          },
          "expiration": {
            "send": false,
            "time": "12:00:00"
          },
          "overdue": {
            "send": false
          }
        },
        "types": {
          "email": true,
          "sms": false,
          "whatsapp": false
        }
      },
      "instructionsMsg": "Teste de Instrução",
      "notes": "Teste de Observação",
      "invoiceUrl": "https://sandbox.easypagamentos.com.br/faturas",
      "payment": {
        "status": "1",
        "description": "Aguardando pagamento"
      },
      "events": [
        {
          "id": "18785c31-19d8-49d2-aab5-917e719fce38",
          "type": "INVOICE.CREATED",
          "createdAt": "2019-06-22T14:39:12.000Z",
          "updatedAt": "2019-06-22T14:39:12.000Z",
          "invoiceId": "74691e6e-ceda-4e2f-88a3-1c08b2c86484"
        }
      ],
      "createdAt": "2019-06-22T14:39:12.000Z",
      "updatedAt": "2019-06-22T14:39:12.000Z"
    }
  ]
}
```

Este endpoint lista todas as cobranças.

### Requisição HTTP

`GET https://sandbox.easypagamentos.com.br/api/v1/invoices`

### Parâmetros Query

| Parâmetro     | Descrição                                                                                                                                                                                                                                                    | Obrigatório |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------- |
| page          | Número da página a ser retornada                                                                                                                                                                                                                             | Não         |
| limit         | Número itens a ser retornado por página. Valor mínimo: 1, valor máximo: 100. Caso não seja infromado, será aplicado o valor default de 100 itens                                                                                                             | Não         |
| search        | Parâmetro que recebe texto e efetua pesquisa nos campos de nome do cliente, email do cliente, cpf/cnpj do cliente, observações e referência da cobrança                                                                                                      | Não         |
| docNumber     | Parâmetro que filtra cobranças a partir de cpf/cnpj. Pode ser informado com ou sem máscara                                                                                                                                                                   | Não         |
| paymentStatus | Parâmetro que filtra cobranças por um ou mais status de pagamento separados por vírgula                                                                                                                                                                      | Não         |
| sortBy        | Um hash contendo na chave, o nome do campo para ordenação e o valor DESC ou ASC para descendente e ascendente, respectivamente. Exemplos: sortBy[dueDate]=DESC, sortBy[amount]=DESC, sortBy[customerName]=ASC, sortBy[createdAt]=ASC, sortBy[updatedAt]=DESC | Não         |
| createdAtFrom | Filtra registros criados a partir da data passada no parâmetro. Formato (AAAA-MM-DDThh:mm:ss)                                                                                                                                                                | Não         |
| createdAtTo   | Filtra registros criados até esta data passada no parâmetro. Formato (AAAA-MM-DDThh:mm:ss)                                                                                                                                                                   | Não         |
| updatedSince  | Filtra registros atualizados desde o valor passado no parâmetro. Formato (AAAA-MM-DDThh:mm:ss)                                                                                                                                                               | Não         |
| paymentStatus | Filtra registros que possuam os status de pagamento informados. Formato (Código status do pagamento. Caso seja mais de um status, separar por vírgula.) Exemplo: paymentStatus=1,2,3 ou paymentStatus=1                                                      | Não         |

## Listar uma cobrança

> Resposta

```http
HTTP/1.1 200 Ok
```

```json
{
  "id": "31da8d81-f0a6-4c9b-a120-0ccf12f96fab",
  "reference": null,
  "dueDate": "2019-07-05",
  "amount": 2000,
  "paidAmount": 0,
  "paidAt": null,
  "currency": "BRL",
  "items": [
    {
      "id": "4c467d85-de18-4566-946e-d36c7fc8ee36",
      "description": "Item de Test - 1",
      "quantity": 1,
      "price": 1000,
      "invoiceId": "31da8d81-f0a6-4c9b-a120-0ccf12f96fab",
      "updatedAt": "2019-04-01T15:51:40.175Z",
      "createdAt": "2019-04-01T15:51:40.175Z"
    },
    {
      "id": "d3ec69fb-df58-4706-a552-19522a8b0f63",
      "description": "Item de Test - 2",
      "quantity": 1,
      "price": 1000,
      "invoiceId": "31da8d81-f0a6-4c9b-a120-0ccf12f96fab",
      "updatedAt": "2019-04-01T15:51:40.177Z",
      "createdAt": "2019-04-01T15:51:40.177Z"
    },
    {
      "id": "b9e00954-e12e-49ad-b6ea-cc1707dfad3d",
      "description": "Item de Test - 3",
      "quantity": 1,
      "price": 1000,
      "invoiceId": "31da8d81-f0a6-4c9b-a120-0ccf12f96fab",
      "updatedAt": "2019-04-01T15:51:40.179Z",
      "createdAt": "2019-04-01T15:51:40.179Z"
    }
  ],
  "itemsDiscount": {
    "amount": 1000
  },
  "earlyDiscount": {
    "percentage": 4.75,
    "earlyDays": 1
  },
  "interest": {
    "percentage": 1
  },
  "fine": {
    "lateDays": "2019-07-12",
    "percentage": 5
  },
  "customer": {
    "name": "João Silva",
    "email": "joao@email.com",
    "phoneNumber": "551199999999",
    "doc": {
      "type": "cpf",
      "number": "64223737830"
    },
    "address": {
      "cep": "01014902",
      "uf": "SP",
      "city": "São Paulo",
      "area": "Bela Vista",
      "addressLine1": "Avenida Paulista",
      "addressLine2": "Sala 9999",
      "streetNumber": "9999"
    }
  },
  "notifications": {
    "cc": {
      "emails": ["usuario1@email.com", "usuario2@email.com"]
    },
    "sendOnCreate": true,
    "reminders": {
      "before": {
        "send": true,
        "days": 1,
        "time": "08:00:00"
      },
      "after": {
        "send": true,
        "days": 2,
        "time": "08:00:00",
        "recurrent": false
      },
      "expiration": {
        "send": true,
        "time": "08:00:00"
      },
      "overdue": {
        "send": true
      }
    },
    "types": {
      "email": true,
      "sms": false,
      "whatsapp": false
    }
  },
  "instructionsMsg": "Teste de Instrução",
  "notes": "Teste de Observação",
  "boleto": {
    "digitableLine": "00190.00009 02625.444100 45000.820170 2 79410000002000",
    "barCodeNumber": "00192794100000020000000002625444104500082017"
  },
  "invoiceUrl": "https://sandbox.easypagamentos.com.br/faturas",
  "payment": {
    "status": "1",
    "description": "Aguardando pagamento"
  }
}
```

Este endpoint lista uma cobrança.

### Requisição HTTP

`GET https://sandbox.easypagamentos.com.br/api/v1/invoices/:id`

### Parâmetros URL

| Parâmetro | Descrição        | Obrigatório |
| --------- | ---------------- | ----------- |
| ID        | O ID da cobrança | Sim         |

## Criar uma cobrança com cliente existente

> Requisição

```json
{
  "dueDate": "2019-07-05",
  "itemsDiscount": {
    "amount": 1000
  },
  "earlyDiscount": {
    "percentage": 4.75,
    "earlyDays": 1
  },
  "interest": {
    "percentage": 1.0
  },
  "fine": {
    "percentage": 5.0,
    "lateDays": 7
  },
  "items": [
    {
      "description": "Item de Test - 1",
      "quantity": 1,
      "price": 1000
    },
    {
      "description": "Item de Test - 2",
      "quantity": 1,
      "price": 1000
    },
    {
      "description": "Item de Test - 3",
      "quantity": 1,
      "price": 1000
    }
  ],
  "customer": {
    "id": "03f7f73f-617d-4d4d-845a-7d464a7d8011"
  },
  "notifications": {
    "cc": {
      "emails": ["usuario1@email.com", "usuario2@email.com"]
    },
    "sendOnCreate": true,
    "reminders": {
      "before": {
        "send": true,
        "days": 1,
        "time": "08:00:00"
      },
      "after": {
        "send": true,
        "days": 2,
        "time": "08:00:00",
        "recurrent": false
      },
      "expiration": {
        "send": true,
        "time": "08:00:00"
      },
      "overdue": {
        "send": true
      }
    },
    "types": {
      "email": true,
      "sms": false,
      "whatsapp": false
    }
  },
  "instructionsMsg": "Teste de Instrução",
  "notes": "Teste de Observação"
}
```

> Resposta

```http
HTTP/1.1 201 Created
```

```json
{
  "id": "31da8d81-f0a6-4c9b-a120-0ccf12f96fab",
  "reference": null,
  "dueDate": "2019-07-05",
  "amount": 2000,
  "paidAmount": 0,
  "paidAt": null,
  "currency": "BRL",
  "items": [
    {
      "id": "4c467d85-de18-4566-946e-d36c7fc8ee36",
      "description": "Item de Test - 1",
      "quantity": 1,
      "price": 1000,
      "invoiceId": "31da8d81-f0a6-4c9b-a120-0ccf12f96fab",
      "updatedAt": "2019-04-01T15:51:40.175Z",
      "createdAt": "2019-04-01T15:51:40.175Z"
    },
    {
      "id": "d3ec69fb-df58-4706-a552-19522a8b0f63",
      "description": "Item de Test - 2",
      "quantity": 1,
      "price": 1000,
      "invoiceId": "31da8d81-f0a6-4c9b-a120-0ccf12f96fab",
      "updatedAt": "2019-04-01T15:51:40.177Z",
      "createdAt": "2019-04-01T15:51:40.177Z"
    },
    {
      "id": "b9e00954-e12e-49ad-b6ea-cc1707dfad3d",
      "description": "Item de Test - 3",
      "quantity": 1,
      "price": 1000,
      "invoiceId": "31da8d81-f0a6-4c9b-a120-0ccf12f96fab",
      "updatedAt": "2019-04-01T15:51:40.179Z",
      "createdAt": "2019-04-01T15:51:40.179Z"
    }
  ],
  "itemsDiscount": {
    "amount": 1000
  },
  "earlyDiscount": {
    "percentage": 4.75,
    "earlyDays": 1
  },
  "interest": {
    "percentage": 1
  },
  "fine": {
    "lateDays": "2019-07-12",
    "percentage": 5
  },
  "customer": {
    "name": "João Silva",
    "email": "joao@email.com",
    "phoneNumber": "551199999999",
    "doc": {
      "type": "cpf",
      "number": "64223737830"
    },
    "address": {
      "cep": "01014902",
      "uf": "SP",
      "city": "São Paulo",
      "area": "Bela Vista",
      "addressLine1": "Avenida Paulista",
      "addressLine2": "Sala 9999",
      "streetNumber": "9999"
    }
  },
  "notifications": {
    "cc": {
      "emails": ["usuario1@email.com", "usuario2@email.com"]
    },
    "sendOnCreate": true,
    "reminders": {
      "before": {
        "send": true,
        "days": 1,
        "time": "08:00:00"
      },
      "after": {
        "send": true,
        "days": 2,
        "time": "08:00:00",
        "recurrent": false
      },
      "expiration": {
        "send": true,
        "time": "08:00:00"
      },
      "overdue": {
        "send": true
      }
    },
    "types": {
      "email": true,
      "sms": false,
      "whatsapp": false
    }
  },
  "instructionsMsg": "Teste de Instrução",
  "notes": "Teste de Observação",
  "boleto": {
    "digitableLine": "00190.00009 02625.444100 45000.820170 2 79410000002000",
    "barCodeNumber": "00192794100000020000000002625444104500082017"
  },
  "invoiceUrl": "https://sandbox.easypagamentos.com.br/faturas",
  "payment": {
    "status": "1",
    "description": "Aguardando pagamento"
  }
}
```

Este endpoint cria uma cobrança para um cliente existente.

### Requisição HTTP

`POST https://sandbox.easypagamentos.com.br/api/v1/invoices`

### Parâmetros Body

| Parâmetro                     | Descrição                                                                                                                                                                          | Tipo    | Obrigatório                                    |
| ----------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- | ---------------------------------------------- |
| dueDate                       | Data de vencimento no formato ISO: YYYY-MM-DD. A data deve ser igual ou posterior a data atual                                                                                     | String  | Sim                                            |
| items                         | Array de objetos de itens de cobrança. Deve conter ao menos um item de cobrança                                                                                                    | Object  | Sim                                            |
| items.description             | Nome ou descrição do item de cobrança                                                                                                                                              | String  | Sim                                            |
| items.quantity                | Quantidade do item de cobrança                                                                                                                                                     | Integer | Sim                                            |
| items.price                   | Valor em centavos do item de cobrança                                                                                                                                              | Integer | Sim                                            |
| itemsDiscount                 | Objeto de descontos dos itens de cobrança                                                                                                                                          | Object  | Não                                            |
| itemsDiscount.amount          | Valor em centavos do desconto imediato a ser concedido sobre o valor total dos itens                                                                                               | Integer | Se não informado percentual do desconto        |
| itemsDiscount.percentage      | Valor percentual do desconto imediato a ser concedido sobre o valor total dos itens                                                                                                | Double  | Se não informado valor em centavos do desconto |
| earlyDiscount                 | Objeto de descontos por antecipação de pagamento                                                                                                                                   | Object  | Não                                            |
| earlyDiscount.amount          | Valor em centavos do desconto a ser concedido sob o valor total dos itens por pagamento antecipado                                                                                 | Integer | Se não informado percentual do desconto        |
| earlyDiscount.percentage      | Valor percentual do desconto a ser concedido sob o valor total dos itens por pagamento antecipado                                                                                  | Double  | Se não informado valor em centavos do desconto |
| earlyDiscount.earlyDays       | Número de dias de antecedência para aplicar o desconto por antecipação.                                                                                                            | Integer | Se informado o objeto de desconto              |
| interest                      | Objeto de juros da cobrança                                                                                                                                                        | Object  | Não                                            |
| interest.percentage           | Valor percentual do juros a ser cobrado sob o valor total dos itens por pagamento atrasado. Deve ser maior que 0 e menor ou igual a 1                                              | Double  | Se informado o objeto de juros                 |
| fine                          | Objeto de multa da cobrança                                                                                                                                                        | Object  | Não                                            |
| fine.percentage               | Valor percentual da multa a ser cobrado sob o valor total dos itens por pagamento atrasado. Deve ser maior que 0 e menor ou igual a 10                                             | Double  | Se informado o objeto de multa                 |
| fine.lateDays                 | Número de dias de atraso para aplicação da multa. Deve ser maior que 0 e menor que 30                                                                                              | Integer | Se informado o objeto de multa                 |
| customer                      | Objeto do Cliente                                                                                                                                                                  | Object  | Sim                                            |
| customer.id                   | ID do Cliente existente para qual a cobrança será criada                                                                                                                           | String  | Sim                                            |
| notifications                 | Objeto de notificações                                                                                                                                                             | Object  | Não                                            |
| notifications.cc              | Objeto de envio de cópias                                                                                                                                                          | Object  | Não                                            |
| notifications.cc.emails       | Array de emails, que também recebem como cópia as notificações da cobrança.                                                                                                        | Array   | Não                                            |
| notifications.sendOnCreate    | Define se a cobrança é enviada para o(s) cliente(s) logo após ser criada                                                                                                           | Boolean | Não                                            |
| notifications.reminders       | Objeto de lembretes de cobrança                                                                                                                                                    | Object  | Não                                            |
| notifications.before          | Objeto de lembrete da data do vencimento                                                                                                                                           | Object  | Não                                            |
| notifications.before.send     | Define se o lembrete deve ser enviado. Default: true                                                                                                                               | Boolean | Não                                            |
| notifications.before.days     | Define a quantidade de dias antes da data de vencimento que o lembrete deve ser enviado. Default: true                                                                             | Boolean | Não                                            |
| notifications.before.time     | Define o horário de preferência que o lembrete deve ser enviado. O formato deve ser `HH:mm:ss` ou `HH:mm:ss A`. Exemplos: `22:00:00` ou `10:00:00 PM`. `06:00:00` ou `06:00:00 AM` | String  | Não                                            |
| notifications.after           | Objeto de lembrete após data do vencimento                                                                                                                                         | Object  | Não                                            |
| notifications.after.send      | Define se o lembrete deve ser enviado. Default: true                                                                                                                               | Boolean | Não                                            |
| notifications.after.days      | Define a quantidade/intervalo de dias após da data de vencimento que o lembrete deve ser enviado. Default: true                                                                    | Integer | Não                                            |
| notifications.after.time      | Define o horário de preferência que o lembrete deve ser enviado. O formato deve ser `HH:mm:ss` ou `HH:mm:ss A`. Exemplos: `22:00:00` ou `10:00:00 PM`. `06:00:00` ou `06:00:00 AM` | String  | Não                                            |
| notifications.after.recurrent | Define se o lembrete é recorrente. Caso seja true, enviará o lembrete a cada `X` dias, conforme especificado em `notifications.after.days`                                         | Boolean | Não                                            |
| notifications.expiration      | Objeto de lembrete na data do vencimento                                                                                                                                           | Object  | Não                                            |
| notifications.expiration.send | Define se o lembrete deve ser enviado. Default: true                                                                                                                               | Boolean | Não                                            |
| notifications.expiration.time | Define o horário de preferência que o lembrete deve ser enviado. O formato deve ser `HH:mm:ss` ou `HH:mm:ss A`. Exemplos: `22:00:00` ou `10:00:00 PM`. `06:00:00` ou `06:00:00 AM` | String  | Não                                            |
| notifications.overdue         | Objeto de lembrete de cobrança vencida                                                                                                                                             | Object  | Não                                            |
| notifications.overdue.send    | Define se o lembrete deve ser enviado, caso a cobrança esteja atrasada. Default: true                                                                                              | Boolean | Não                                            |
| instructionsMsg               | Instruções de pagamento que constará no boleto. Deve conter no máximo 100 caracteres                                                                                               | String  | Não                                            |
| notes                         | Observações sobre o pagamento. Não constará no boleto. Deve conter no máximo 100 caracteres                                                                                        | String  | Não                                            |

## Criar uma cobrança com novo cliente

> Requisição

```json
{
  "dueDate": "2019-07-05",
  "itemsDiscount": {
    "amount": 1000
  },
  "earlyDiscount": {
    "percentage": 4.75,
    "earlyDays": 1
  },
  "interest": {
    "percentage": 1.0
  },
  "fine": {
    "percentage": 5.0,
    "lateDays": 7
  },
  "items": [
    {
      "description": "Item de Test - 1",
      "quantity": 1,
      "price": 1000
    },
    {
      "description": "Item de Test - 2",
      "quantity": 1,
      "price": 1000
    },
    {
      "description": "Item de Test - 3",
      "quantity": 1,
      "price": 1000
    }
  ],
  "customer": {
    "name": "João Silva",
    "email": "joao@email.com",
    "phoneNumber": "551199999999",
    "docNumber": "64223737830",
    "address": {
      "cep": "01014902",
      "uf": "SP",
      "city": "São Paulo",
      "area": "Bela Vista",
      "addressLine1": "Avenida Paulista",
      "addressLine2": "Sala 9999",
      "streetNumber": "9999"
    }
  },
  "notifications": {
    "cc": {
      "emails": ["usuario1@email.com", "usuario2@email.com"]
    },
    "sendOnCreate": true,
    "reminders": {
      "before": {
        "send": true,
        "days": 1,
        "time": "08:00:00"
      },
      "after": {
        "send": true,
        "days": 2,
        "time": "08:00:00",
        "recurrent": false
      },
      "expiration": {
        "send": true,
        "time": "08:00:00"
      },
      "overdue": {
        "send": true
      }
    },
    "types": {
      "email": true,
      "sms": false,
      "whatsapp": false
    }
  },
  "instructionsMsg": "Teste de Instrução",
  "notes": "Teste de Observação"
}
```

> Resposta

```http
HTTP/1.1 201 Created
```

```json
{
  "id": "32a41dc3-4a1a-4933-8b6c-9b4e962b7f28",
  "reference": null,
  "dueDate": "2019-07-05",
  "amount": 2000,
  "paidAmount": 0,
  "paidAt": null,
  "currency": "BRL",
  "items": [
    {
      "id": "3a8662dd-71cc-474d-9edd-c82e670d657d",
      "description": "Item de Test - 1",
      "quantity": 1,
      "price": 1000,
      "invoiceId": "32a41dc3-4a1a-4933-8b6c-9b4e962b7f28",
      "updatedAt": "2019-04-01T16:00:46.077Z",
      "createdAt": "2019-04-01T16:00:46.077Z"
    },
    {
      "id": "2d364dcb-4bfd-47e2-83e7-f71298fda992",
      "description": "Item de Test - 2",
      "quantity": 1,
      "price": 1000,
      "invoiceId": "32a41dc3-4a1a-4933-8b6c-9b4e962b7f28",
      "updatedAt": "2019-04-01T16:00:46.079Z",
      "createdAt": "2019-04-01T16:00:46.079Z"
    },
    {
      "id": "08e3d702-18a9-4bfb-94c8-7f0c36589208",
      "description": "Item de Test - 3",
      "quantity": 1,
      "price": 1000,
      "invoiceId": "32a41dc3-4a1a-4933-8b6c-9b4e962b7f28",
      "updatedAt": "2019-04-01T16:00:46.079Z",
      "createdAt": "2019-04-01T16:00:46.079Z"
    }
  ],
  "itemsDiscount": {
    "amount": 1000
  },
  "earlyDiscount": {
    "percentage": 4.75,
    "earlyDays": 1
  },
  "interest": {
    "percentage": 1
  },
  "fine": {
    "lateDays": "2019-07-12",
    "percentage": 5
  },
  "customer": {
    "name": "João Silva",
    "email": "joao@email.com",
    "phoneNumber": "551199999999",
    "doc": {
      "type": "cpf",
      "number": "64223737830"
    },
    "address": {
      "cep": "01014902",
      "uf": "SP",
      "city": "São Paulo",
      "area": "Bela Vista",
      "addressLine1": "Avenida Paulista",
      "addressLine2": "Sala 9999",
      "streetNumber": "9999"
    }
  },
  "notifications": {
    "cc": {
      "emails": ["usuario1@email.com", "usuario2@email.com"]
    },
    "sendOnCreate": true,
    "reminders": {
      "before": {
        "send": true,
        "days": 1,
        "time": "08:00:00"
      },
      "after": {
        "send": true,
        "days": 2,
        "time": "08:00:00",
        "recurrent": false
      },
      "expiration": {
        "send": true,
        "time": "08:00:00"
      },
      "overdue": {
        "send": true
      }
    },
    "types": {
      "email": true,
      "sms": false,
      "whatsapp": false
    }
  },
  "instructionsMsg": "Teste de Instrução",
  "notes": "Teste de Observação",
  "boleto": {
    "digitableLine": "00190.00009 02625.444100 45000.822176 5 79410000002000",
    "barCodeNumber": "00195794100000020000000002625444104500082217"
  },
  "invoiceUrl": "https://sandbox.easypagamentos.com.br/faturas",
  "payment": {
    "status": "1",
    "description": "Aguardando pagamento"
  }
}
```

Este endpoint cria uma cobrança com novo cliente.

### Requisição HTTP

`POST https://sandbox.easypagamentos.com.br/api/v1/invoices`

### Parâmetros Body

| Parâmetro                     | Descrição                                                                                                                                                                          | Tipo    | Obrigatório                                    |
| ----------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- | ---------------------------------------------- |
| dueDate                       | Data de vencimento no formato ISO: YYYY-MM-DD. A data deve ser igual ou posterior a data atual                                                                                     | String  | Sim                                            |
| items                         | Array de objetos de itens de cobrança. Deve conter ao menos um item de cobrança                                                                                                    | Object  | Sim                                            |
| items.description             | Nome ou descrição do item de cobrança                                                                                                                                              | String  | Sim                                            |
| items.quantity                | Quantidade do item de cobrança                                                                                                                                                     | Integer | Sim                                            |
| items.price                   | Valor em centavos do item de cobrança                                                                                                                                              | Integer | Sim                                            |
| itemsDiscount                 | Objeto de descontos dos itens de cobrança                                                                                                                                          | Object  | Não                                            |
| itemsDiscount.amount          | Valor em centavos do desconto imediato a ser concedido sobre o valor total dos itens                                                                                               | Integer | Se não informado percentual do desconto        |
| itemsDiscount.percentage      | Valor percentual do desconto imediato a ser concedido sobre o valor total dos itens                                                                                                | Double  | Se não informado valor em centavos do desconto |
| earlyDiscount                 | Objeto de descontos por antecipação de pagamento                                                                                                                                   | Object  | Não                                            |
| earlyDiscount.amount          | Valor em centavos do desconto a ser concedido sob o valor total dos itens por pagamento antecipado                                                                                 | Integer | Se não informado percentual do desconto        |
| earlyDiscount.percentage      | Valor percentual do desconto a ser concedido sob o valor total dos itens por pagamento antecipado                                                                                  | Double  | Se não informado valor em centavos do desconto |
| earlyDiscount.earlyDays       | Número de dias de antecedência para aplicar o desconto por antecipação.                                                                                                            | Integer | Se informado o objeto de desconto              |
| interest                      | Objeto de juros da cobrança                                                                                                                                                        | Object  | Não                                            |
| interest.percentage           | Valor percentual do juros a ser cobrado sob o valor total dos itens por pagamento atrasado. Deve ser maior que 0 e menor ou igual a 1                                              | Double  | Se informado o objeto de juros                 |
| fine                          | Objeto de multa da cobrança                                                                                                                                                        | Object  | Não                                            |
| fine.percentage               | Valor percentual da multa a ser cobrado sob o valor total dos itens por pagamento atrasado. Deve ser maior que 0 e menor ou igual a 10                                             | Double  | Se informado o objeto de multa                 |
| fine.lateDays                 | Número de dias de atraso para aplicação da multa. Deve ser maior que 0 e menor que 30                                                                                              | Integer | Se informado o objeto de multa                 |
| customer                      | Objeto do Cliente                                                                                                                                                                  | Object  | Sim                                            |
| customer.name                 | Nome do Cliente                                                                                                                                                                    | String  | Sim                                            |
|                               |
| customer.email                | Email do Cliente                                                                                                                                                                   | String  | Sim                                            |
|                               |
| customer.phoneNumber          | Número de telefone do cliente. Deve seguir o seguinte formato: `55`, seguido do número DDD, seguido do telefone, por exemplo: `551199999999`                                       | String  | Sim                                            |
| customer.docNumber            | Número de CPF/CNPJ do cliente. Pode ser informado com ou sem máscara                                                                                                               | String  | Sim                                            |
| customer.address              | Objeto contendo informações de endereço do cliente                                                                                                                                 | Object  | Sim                                            |
| customer.address.cep          | Número do Cep do endereço. Pode ser com ou sem máscara                                                                                                                             | String  | Sim                                            |
| customer.address.uf           | Sigle UF do endereço                                                                                                                                                               | String  | Sim                                            |
| customer.address.city         | Cidade do endereço                                                                                                                                                                 | String  | Sim                                            |
| customer.address.area         | Bairro do endereço                                                                                                                                                                 | String  | Sim                                            |
| customer.address.addressLine1 | Logradouro do endereço                                                                                                                                                             | String  | Sim                                            |
| customer.address.addressLine2 | Complemento do endereço                                                                                                                                                            | String  | Não                                            |
| customer.address.streetNumber | Número do logradouro                                                                                                                                                               | String  | Não                                            |
| String                        | Sim                                                                                                                                                                                |
| notifications                 | Objeto de notificações                                                                                                                                                             | Object  | Não                                            |
| notifications.sendOnCreate    | Define se a cobrança é enviada para o(s) cliente(s) logo após ser criada                                                                                                           | Boolean | Não                                            |
| notifications.reminders       | Objeto de lembretes de cobrança                                                                                                                                                    | Object  | Não                                            |
| notifications.before          | Objeto de lembrete da data do vencimento                                                                                                                                           | Object  | Não                                            |
| notifications.before.send     | Define se o lembrete deve ser enviado. Default: true                                                                                                                               | Boolean | Não                                            |
| notifications.before.days     | Define a quantidade de dias antes da data de vencimento que o lembrete deve ser enviado. Default: true                                                                             | Boolean | Não                                            |
| notifications.before.time     | Define o horário de preferência que o lembrete deve ser enviado. O formato deve ser `HH:mm:ss` ou `HH:mm:ss A`. Exemplos: `22:00:00` ou `10:00:00 PM`. `06:00:00` ou `06:00:00 AM` | String  | Não                                            |
| notifications.after           | Objeto de lembrete após data do vencimento                                                                                                                                         | Object  | Não                                            |
| notifications.after.send      | Define se o lembrete deve ser enviado. Default: true                                                                                                                               | Boolean | Não                                            |
| notifications.after.days      | Define a quantidade/intervalo de dias após da data de vencimento que o lembrete deve ser enviado. Default: true                                                                    | Integer | Não                                            |
| notifications.after.time      | Define o horário de preferência que o lembrete deve ser enviado. O formato deve ser `HH:mm:ss` ou `HH:mm:ss A`. Exemplos: `22:00:00` ou `10:00:00 PM`. `06:00:00` ou `06:00:00 AM` | String  | Não                                            |
| notifications.after.recurrent | Define se o lembrete é recorrente. Caso seja true, enviará o lembrete a cada `X` dias, conforme especificado em `notifications.after.days`                                         | Boolean | Não                                            |
| notifications.expiration      | Objeto de lembrete na data do vencimento                                                                                                                                           | Object  | Não                                            |
| notifications.expiration.send | Define se o lembrete deve ser enviado. Default: true                                                                                                                               | Boolean | Não                                            |
| notifications.expiration.time | Define o horário de preferência que o lembrete deve ser enviado. O formato deve ser `HH:mm:ss` ou `HH:mm:ss A`. Exemplos: `22:00:00` ou `10:00:00 PM`. `06:00:00` ou `06:00:00 AM` | String  | Não                                            |
| notifications.overdue         | Objeto de lembrete de cobrança vencida                                                                                                                                             | Object  | Não                                            |
| notifications.overdue.send    | Define se o lembrete deve ser enviado, caso a cobrança esteja atrasada. Default: true                                                                                              | Boolean | Não                                            |
| instructionsMsg               | Instruções de pagamento que constará no boleto. Deve conter no máximo 100 caracteres                                                                                               | String  | Não                                            |
| notes                         | Observações sobre o pagamento. Não constará no boleto. Deve conter no máximo 100 caracteres                                                                                        | String  | Não                                            |

## Atualizar uma cobrança

> Requisição

```json
{
  "dueDate": "2019-05-23",
  "earlyDiscount": {
    "percentage": 10,
    "earlyDays": 1
  },
  "interest": {
    "percentage": 1
  },
  "fine": {
    "lateDays": 10,
    "percentage": 2
  },
  "instructionsMsg": "Teste de Instrução atualizado",
  "notes": "Teste de Observação atualizado"
}
```

> Resposta

```http
HTTP/1.1 200 Ok
```

```json
{
  "id": "12c33170-bf9a-4dc1-9388-9a7c73a2cfa2",
  "reference": null,
  "dueDate": "2019-05-23",
  "amount": 2000,
  "paidAmount": 0,
  "currency": "BRL",
  "items": [
    {
      "id": "86c6c457-de9f-4da9-ac17-2b12f0a47e51",
      "description": "Serviço 1",
      "quantity": 1,
      "price": 1000,
      "createdAt": "2019-03-27T17:47:05.000Z",
      "updatedAt": "2019-03-27T17:47:05.000Z",
      "invoiceId": "12c33170-bf9a-4dc1-9388-9a7c73a2cfa2"
    },
    {
      "id": "b9bd1a93-1a06-45c4-8b7e-f7dacb1a740f",
      "description": "Serviço 2",
      "quantity": 1,
      "price": 1000,
      "createdAt": "2019-03-27T17:47:05.000Z",
      "updatedAt": "2019-03-27T17:47:05.000Z",
      "invoiceId": "12c33170-bf9a-4dc1-9388-9a7c73a2cfa2"
    }
  ],
  "customer": {
    "name": "João Silva",
    "email": "joao2@email.com",
    "phoneNumber": "5511888888888",
    "doc": {
      "type": "cpf",
      "number": "64223737830"
    },
    "address": {
      "cep": "60135222",
      "uf": "SP",
      "city": "São Paulo",
      "area": "Bela Vista",
      "addressLine1": "Avenida Paulista",
      "addressLine2": "Sala 9999",
      "streetNumber": "9999"
    },
    "createdAt": "2019-03-28T18:42:44.000Z",
    "updatedAt": "2019-03-28T18:45:50.000Z"
  },
  "instructionsMsg": "Teste de Instrução",
  "notes": "Teste de Observação",
  "boleto": {
    "digitableLine": "00190.00009 02625.444100 45000.805171 2 78910000002000",
    "barCodeNumber": "00192789100000020000000002625444104500080517"
  },
  "invoiceUrl": "https://sandbox.easypagamentos.com.br/faturas",
  "payment": {
    "status": "1",
    "description": "Aguardando pagamento"
  }
}
```

Este atualiza data de vencimento de uma cobrança.

### Requisição HTTP

`PUT https://sandbox.easypagamentos.com.br/api/v1/invoices/:id`

### Parâmetros URL

| Parâmetro | Descrição        | Obrigatório |
| --------- | ---------------- | ----------- |
| ID        | O ID da cobrança | Sim         |

### Parâmetros Body

| Parâmetro                     | Descrição                                                                                                                                                                          | Tipo    | Obrigatório                                    |
| ----------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- | ---------------------------------------------- |
| dueDate                       | Data de vencimento no formato ISO: YYYY-MM-DD. A data deve ser igual ou posterior a data atual                                                                                     | String  | Sim                                            |
| earlyDiscount                 | Objeto de descontos por antecipação de pagamento                                                                                                                                   | Object  | Não                                            |
| earlyDiscount.amount          | Valor em centavos do desconto a ser concedido sob o valor total dos itens por pagamento antecipado                                                                                 | Integer | Se não informado percentual do desconto        |
| earlyDiscount.percentage      | Valor percentual do desconto a ser concedido sob o valor total dos itens por pagamento antecipado                                                                                  | Double  | Se não informado valor em centavos do desconto |
| earlyDiscount.earlyDays       | Número de dias de antecedência para aplicar o desconto por antecipação.                                                                                                            | Integer | Se informado o objeto de desconto              |
| interest                      | Objeto de juros da cobrança                                                                                                                                                        | Object  | Não                                            |
| interest.percentage           | Valor percentual do juros a ser cobrado sob o valor total dos itens por pagamento atrasado. Deve ser maior que 0 e menor ou igual a 1                                              | Double  | Se informado o objeto de juros                 |
| fine                          | Objeto de multa da cobrança                                                                                                                                                        | Object  | Não                                            |
| fine.percentage               | Valor percentual da multa a ser cobrado sob o valor total dos itens por pagamento atrasado. Deve ser maior que 0 e menor ou igual a 10                                             | Double  | Se informado o objeto de multa                 |
| fine.lateDays                 | Número de dias de atraso para aplicação da multa. Deve ser maior que 0 e menor que 30                                                                                              | Integer | Se informado o objeto de multa                 |
| notifications                 | Objeto de notificações                                                                                                                                                             | Object  | Não                                            |
| notifications.sendOnCreate    | Define se a cobrança é enviada para o(s) cliente(s) logo após ser criada                                                                                                           | Boolean | Não                                            |
| notifications.reminders       | Objeto de lembretes de cobrança                                                                                                                                                    | Object  | Não                                            |
| notifications.before          | Objeto de lembrete da data do vencimento                                                                                                                                           | Object  | Não                                            |
| notifications.before.send     | Define se o lembrete deve ser enviado. Default: true                                                                                                                               | Boolean | Não                                            |
| notifications.before.days     | Define a quantidade de dias antes da data de vencimento que o lembrete deve ser enviado. Default: true                                                                             | Boolean | Não                                            |
| notifications.before.time     | Define o horário de preferência que o lembrete deve ser enviado. O formato deve ser `HH:mm:ss` ou `HH:mm:ss A`. Exemplos: `22:00:00` ou `10:00:00 PM`. `06:00:00` ou `06:00:00 AM` | String  | Não                                            |
| notifications.after           | Objeto de lembrete após data do vencimento                                                                                                                                         | Object  | Não                                            |
| notifications.after.send      | Define se o lembrete deve ser enviado. Default: true                                                                                                                               | Boolean | Não                                            |
| notifications.after.days      | Define a quantidade/intervalo de dias após da data de vencimento que o lembrete deve ser enviado. Default: true                                                                    | Integer | Não                                            |
| notifications.after.time      | Define o horário de preferência que o lembrete deve ser enviado. O formato deve ser `HH:mm:ss` ou `HH:mm:ss A`. Exemplos: `22:00:00` ou `10:00:00 PM`. `06:00:00` ou `06:00:00 AM` | String  | Não                                            |
| notifications.after.recurrent | Define se o lembrete é recorrente. Caso seja true, enviará o lembrete a cada `X` dias, conforme especificado em `notifications.after.days`                                         | Boolean | Não                                            |
| notifications.expiration      | Objeto de lembrete na data do vencimento                                                                                                                                           | Object  | Não                                            |
| notifications.expiration.send | Define se o lembrete deve ser enviado. Default: true                                                                                                                               | Boolean | Não                                            |
| notifications.expiration.time | Define o horário de preferência que o lembrete deve ser enviado. O formato deve ser `HH:mm:ss` ou `HH:mm:ss A`. Exemplos: `22:00:00` ou `10:00:00 PM`. `06:00:00` ou `06:00:00 AM` | String  | Não                                            |
| notifications.overdue         | Objeto de lembrete de cobrança vencida                                                                                                                                             | Object  | Não                                            |
| notifications.overdue.send    | Define se o lembrete deve ser enviado, caso a cobrança esteja atrasada. Default: true                                                                                              | Boolean | Não                                            |
| instructionsMsg               | Instruções de pagamento que constará no boleto. Deve conter no máximo 100 caracteres                                                                                               | String  | Não                                            |
| notes                         | Observações sobre o pagamento. Não constará no boleto. Deve conter no máximo 100 caracteres                                                                                        | String  | Não                                            |

## Cancelar uma cobrança

> Resultado

```http
HTTP/1.1 200 Ok
```

Este endpoint cancela uma cobrança.

### Requisição HTTP

`POST https://sandbox.easypagamentos.com.br/api/v1/invoices/:id/cancel`

### Parâmetros URL

| Parâmetro | Descrição        | Obrigatório |
| --------- | ---------------- | ----------- |
| ID        | O ID da cobrança | Sim         |

## Visualizar boleto de uma cobrança

> Resultado

```http
HTTP/1.1 200 Ok
```

Este endpoint renderiza o arquivo pdf do boleto de uma cobrança.

### Requisição HTTP

`GET https://sandbox.easypagamentos.com.br/api/v1/invoices/:id/view/boleto`

### Parâmetros URL

| Parâmetro | Descrição                         | Obrigatório |
| --------- | --------------------------------- | ----------- |
| ID        | O ID da cobrança a ser consultado | Sim         |

## Reenviar uma cobrança

> Resposta

```http
HTTP/1.1 200 Ok
```

Este endpoint reenvia uma cobrança.

### Requisição HTTP

`GET https://sandbox.easypagamentos.com.br/api/v1/invoices/:id/resend`

### Parâmetros URL

| Parâmetro | Descrição        | Obrigatório |
| --------- | ---------------- | ----------- |
| ID        | O ID da cobrança | Sim         |

# Bancos

## Listar Bancos

> Resposta

```http
HTTP/1.1 200 Ok
```

```json
[
  {
    "id": "8c0be6f8-f8ee-47a0-bb44-bc51580224c3",
    "bankCode": "001",
    "bankName": "Banco do Brasil S.A.",
    "websiteHref": "www.bb.com.br",
    "createdAt": "2019-04-01T20:01:45.000Z",
    "updatedAt": "2019-04-01T20:01:45.000Z"
  }
]
```

Este endpoint lista as informações das instituições financeiras disponíveis.

### Requisição HTTP

`GET https://sandbox.easypagamentos.com.br/api/v1/banks?search=banco do brasil`

### Parâmetros Query

| Parâmetro | Descrição                                                                 | Obrigatório |
| --------- | ------------------------------------------------------------------------- | ----------- |
| search    | Palavra-chave para efetuar busca. Se não informado retorna todos os itens | Não         |

# Contas Bancárias

## Listar todas as contas bancárias

> Resposta

```http
HTTP/1.1 200 Ok
```

```json
[
  {
    "id": "4f21bc32-f630-406b-840c-254b529b3c27",
    "beneficiary": {
      "name": "João Silva",
      "doc": {
        "type": "cpf",
        "number": "64223737830"
      }
    },
    "bank": {
      "name": "Banco do Brasil S.A."
    },
    "agency": {
      "number": "9999",
      "digit": "9"
    },
    "account": {
      "number": "999999",
      "digit": "9",
      "type": "checking"
    },
    "status": {
      "code": "2",
      "description": "Conta aprovada."
    },
    "createdAt": "2019-04-02T16:27:44.000Z",
    "updatedAt": "2019-04-02T16:27:44.000Z"
  }
]
```

Este endpoint lista as contas bancárias cadastradas do usuário.

### Requisição HTTP

`GET https://sandbox.easypagamentos.com.br/api/v1/bank-accounts`

## Listar uma conta bancária

> Resposta

```http
HTTP/1.1 200 Ok
```

```json
{
  "id": "4f21bc32-f630-406b-840c-254b529b3c27",
  "beneficiary": {
    "name": "João Silva",
    "doc": {
      "type": "cpf",
      "number": "64223737830"
    }
  },
  "bank": {
    "name": "Banco do Brasil S.A."
  },
  "agency": {
    "number": "9999",
    "digit": "9"
  },
  "account": {
    "number": "999999",
    "digit": "9",
    "type": "checking"
  },
  "status": {
    "code": "2",
    "description": "Conta aprovada."
  },
  "createdAt": "2019-04-02T16:27:44.000Z",
  "updatedAt": "2019-04-02T16:27:44.000Z"
}
```

Este endpoint lista uma conta bancária do usuário.

### Requisição HTTP

`GET https://sandbox.easypagamentos.com.br/api/v1/bank-accounts/:id`

### Parâmetros URL

| Parâmetro | Descrição              | Obrigatório |
| --------- | ---------------------- | ----------- |
| ID        | O ID da conta bancária | Sim         |

## Criar uma conta bancária

> Requisição

```json
{
  "beneficiary": {
    "name": "João Silva",
    "docNumber": "64223737830"
  },
  "bankCode": "001",
  "agency": {
    "number": "9999",
    "digit": "9"
  },
  "account": {
    "number": "999999",
    "digit": "9",
    "type": "checking"
  }
}
```

> Resposta

```http
HTTP/1.1 200 Ok
```

```json
{
  "id": "4f21bc32-f630-406b-840c-254b529b3c27",
  "beneficiary": {
    "name": "João Silva",
    "doc": {
      "type": "cpf",
      "number": "64223737830"
    }
  },
  "bank": {
    "name": "Banco do Brasil S.A."
  },
  "agency": {
    "number": "9999",
    "digit": "9"
  },
  "account": {
    "number": "999999",
    "digit": "9",
    "type": "checking"
  },
  "status": {
    "code": "2",
    "description": "Conta aprovada."
  },
  "createdAt": "2019-04-02T16:27:44.000Z",
  "updatedAt": "2019-04-02T16:27:44.000Z"
}
```

Este endpoint cria uma conta bancária.

### Requisição HTTP

`POST https://sandbox.easypagamentos.com.br/api/v1/bank-accounts`

### Parâmetros Body

| Parâmetro             | Descrição                                                                                       | Tipo   | Obrigatório |
| --------------------- | ----------------------------------------------------------------------------------------------- | ------ | ----------- |
| beneficiary           | Objeto do titular da conta                                                                      | Object | Sim         |
| beneficiary.name      | Nome completo do titular da conta                                                               | Object | Sim         |
| beneficiary.docNumber | CPF/CNPJ do titular da conta                                                                    | Object | Sim         |
| agency                | Objeto da agência da conta                                                                      | Object | Sim         |
| agency.number         | Número agência da conta. Máximo de 5 dígitos                                                    | String | Sim         |
| agency.digit          | Dígito agência da conta . Máximo de 2 digitos                                                   | String | Não         |
| account               | Objeto da conta                                                                                 | Object | Sim         |
| account.number        | Número da conta. Máximo de 12 dígitos                                                           | String | Sim         |
| account.digit         | Dígito conta. Máximo de 2 digitos                                                               | String | Sim         |
| account.type          | Tipo de conta bancária. Informar "checking" para conta corrente ou "saving" para conta poupança | String | Sim         |

## Deletar uma conta bancária

> Resposta

```http
HTTP/1.1 204 No Content
```

Este endpoint exclui uma conta bancária do usuário.

### Requisição HTTP

`DELETE https://sandbox.easypagamentos.com.br/api/v1/bank-accounts/:id`

### Parâmetros URL

| Parâmetro | Descrição              | Obrigatório |
| --------- | ---------------------- | ----------- |
| ID        | O ID da conta bancária | Sim         |

# Resgates

## Status de Resgate

O parâmetro `status.code`, retornado no objeto de resposta, contém o status do resgate.

| Status | Descrição                                                                                            |
| ------ | ---------------------------------------------------------------------------------------------------- |
| 1      | Pedido de resgate efetuado                                                                           |
| 2      | Transferência bancária efetuada. Aguardando confirmação do banco de destino                          |
| 3      | Pedido de resgate recusado. Por favor entre em contato com nosso suporte                             |
| 4      | Pedido de resgate concluído com sucesso. O banco de destino confirmou o recebimento da transferência |
| 5      | Pedido de resgate falhou. A transferência foi devolvida pelo banco de destino                        |

## Listar todos os resgates

> Resposta

```http
HTTP/1.1 200 Ok
```

```json
{
  "page": 1,
  "limit": 100,
  "pagesTotal": 1,
  "count": 1,
  "results": [
    {
      "id": "2c486134-0e27-4c1c-a312-0fe468dae711",
      "amount": 1000,
      "bankAccount": {
        "beneficiary": {
          "name": "João Silva",
          "doc": {
            "type": "cpf",
            "number": "64223737830"
          }
        },
        "bank": {
          "name": "Banco do Brasil S.A.",
          "bankCode": "001"
        },
        "agency": {
          "number": "9999",
          "digit": "9"
        },
        "account": {
          "number": "999999",
          "digit": "9",
          "type": "checking"
        }
      },
      "status": {
        "code": "1",
        "description": "Pedido de resgate recebido."
      },
      "createdAt": "2019-04-02T17:11:20.000Z",
      "updatedAt": "2019-04-02T17:11:20.000Z"
    }
  ]
}
```

Este endpoint lista todos os resgates do usuário.

### Requisição HTTP

`GET https://sandbox.easypagamentos.com.br/api/v1/redemptions`

### Parâmetros Query

| Parâmetro     | Descrição                                                                                                                                                                                                                           | Obrigatório |
| ------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------- |
| page          | Número da página a ser retornada                                                                                                                                                                                                    | Não         |
| limit         | Número itens a ser retornado por página. Valor mínimo: 1, valor máximo: 100. Caso não seja infromado, será aplicado o valor default de 100 itens                                                                                    | Não         |
| search        | Parâmetro que recebe texto e efetua pesquisa nos campos de nome do banco, nome do titular da conta, número da agência da conta, número da conta                                                                                     | Não         |
| docNumber     | Parâmetro que filtra resgates a partir de cpf/cnpj do beneficiário. Pode ser informado com ou sem máscara                                                                                                                           | Não         |
| status        | Parâmetro que filtra resgates por um ou mais status separados por vírgula                                                                                                                                                           | Não         |
| sortBy        | Um hash contendo na chave, o nome do campo para ordenação e o valor DESC ou ASC para descendente e ascendente, respectivamente. Exemplos: sortBy[amount]=ASC, sortBy[approvedAt]=ASC, sortBy[createdAt]=ASC, sortBy[updatedAt]=DESC | Não         |
| createdAtFrom | Filtra registros criados a partir da data passada no parâmetro. Formato (AAAA-MM-DDThh:mm:ss)                                                                                                                                       | Não         |
| createdAtTo   | Filtra registros criados até esta data passada no parâmetro. Formato (AAAA-MM-DDThh:mm:ss)                                                                                                                                          | Não         |
| updatedSince  | Filtra registros atualizados desde o valor passado no parâmetro. Formato (AAAA-MM-DDThh:mm:ss)                                                                                                                                      | Não         |

## Listar um resgate

> Resposta

```http
HTTP/1.1 200 Ok
```

```json
{
  "id": "2c486134-0e27-4c1c-a312-0fe468dae711",
  "amount": 1000,
  "bankAccount": {
    "beneficiary": {
      "name": "João Silva",
      "doc": {
        "type": "cpf",
        "number": "64223737830"
      }
    },
    "bank": {
      "name": "Banco do Brasil S.A.",
      "bankCode": "001"
    },
    "agency": {
      "number": "9999",
      "digit": "9"
    },
    "account": {
      "number": "999999",
      "digit": "9",
      "type": "checking"
    }
  },
  "status": {
    "code": "1",
    "description": "Pedido de resgate recebido."
  },
  "createdAt": "2019-04-02T17:11:20.000Z",
  "updatedAt": "2019-04-02T17:11:20.000Z"
}
```

Este endpoint lista um resgate do usuário.

### Requisição HTTP

`GET https://sandbox.easypagamentos.com.br/api/v1/bank-accounts/:id`

### Parâmetros URL

| Parâmetro | Descrição       | Obrigatório |
| --------- | --------------- | ----------- |
| ID        | O ID do resgate | Sim         |

## Criar resgate

> Requisição

```json
{
  "amount": "1000",
  "bankAccount": {
    "id": "8001d1cb-be89-488c-aee2-01c42e7f8c4b"
  }
}
```

> Resposta

```http
HTTP/1.1 200 Ok
```

```json
{
  "id": "2c486134-0e27-4c1c-a312-0fe468dae711",
  "amount": "1000",
  "bankAccount": {
    "beneficiary": {
      "name": "João Silva",
      "doc": {
        "type": "cpf",
        "number": "64223737830"
      }
    },
    "bank": {
      "name": "Banco do Brasil S.A.",
      "bankCode": "001"
    },
    "agency": {
      "number": "9999",
      "digit": "9"
    },
    "account": {
      "number": "999999",
      "digit": "9",
      "type": "checking"
    }
  },
  "status": {
    "code": "1",
    "description": "Pedido de resgate recebido."
  },
  "createdAt": "2019-04-02T17:11:20.395Z",
  "updatedAt": "2019-04-02T17:11:20.395Z"
}
```

Este endpoint cria um resgate do saldo da conta.

### Requisição HTTP

`POST https://sandbox.easypagamentos.com.br/api/v1/redemptions`

### Parâmetros Body

| Parâmetro      | Descrição                                                                                         | Tipo    | Obrigatório |
| -------------- | ------------------------------------------------------------------------------------------------- | ------- | ----------- |
| amount         | Valor em centavos a ser solicitado. Deve ser menor ou igual ao valor disponível do saldo da conta | Integer | Sim         |
| bankAccount    | Objeto da conta bancária                                                                          | Object  | Sim         |
| bankAccount.id | Id conta bancária cadastrada, para qual será efetuada a transferência do valor solicitado         | String  | Sim         |

# Saldos

## Consultar saldos

> Resposta

```http
HTTP/1.1 200 Ok
```

```json
{
  "available": {
    "amount": 4000,
    "currency": "BRL"
  },
  "pendingPayments": {
    "amount": 28000,
    "currency": "BRL"
  },
  "pendingRedemptions": {
    "amount": 0,
    "currency": "BRL"
  }
}
```

Este endpoint lista os saldos do usuário.

### Requisição HTTP

`GET https://sandbox.easypagamentos.com.br/api/v1/balances`

# Extratos

## Tipos de Transação

O parâmetro `type`, retornado no objeto de resposta, contém o tipo de transação no extrato.

| Tipo | Descrição                                |
| ---- | ---------------------------------------- |
| 1    | Crédito de cobrança paga                 |
| 2    | Valor de resgate solicitado              |
| 3    | Extorno de resgate solicitado            |
| 4    | Débito de taxa de recebimento por boleto |

## Listar Extratos

> Resposta

```http
HTTP/1.1 200 Ok
```

```json
{
  "page": 1,
  "limit": 100,
  "pagesTotal": 1,
  "count": 1,
  "results": [
    {
      "id": "134c09ae-9293-11e9-9611-73ceb10b2493",
      "amount": 1000,
      "balance": {
        "previousAmount": 1000,
        "availableAmount": 2000
      },
      "currency": "BRL",
      "type": "1",
      "description": "Crédito de cobrança(s) paga(s)",
      "reference": {
        "id": "1cae37d7-8006-439b-9a07-2580d79569f2",
        "type": "INVOICE"
      },
      "createdAt": "2019-03-26T10:45:38.000Z"
    }
  ]
}
```

Este endpoint lista os extratos do usuário.

### Requisição HTTP

`GET https://sandbox.easypagamentos.com.br/api/v1/statements`

### Parâmetros Query

| Parâmetro     | Descrição                                                                                                                                                                            | Obrigatório |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------- |
| page          | Número da página a ser retornada                                                                                                                                                     | Não         |
| limit         | Número itens a ser retornado por página. Valor mínimo: 1, valor máximo: 100. Caso não seja infromado, será aplicado o valor default de 100 itens                                     | Não         |
| type          | Filtra registros que possuam o tipo de transação informada. Formato (Código do tipo de transação. Caso seja mais de um tipo, separar por vírgula.) Exemplo: type=1,2,3 ou type=1     | Não         |
| sortBy        | Um hash contendo na chave, o nome do campo para ordenação e o valor DESC ou ASC para descendente e ascendente, respectivamente. Exemplos: sortBy[amount]=DESC, sortBy[createdAt]=ASC | Não         |
| createdAtFrom | Filtra registros criados a partir da data passada no parâmetro. Formato (AAAA-MM-DDThh:mm:ss)                                                                                        | Não         |
| createdAtTo   | Filtra registros criados até esta data passada no parâmetro. Formato (AAAA-MM-DDThh:mm:ss)                                                                                           | Não         |

# Relatórios

## Listar Saldos de cobranças

> Resposta

```http
HTTP/1.1 200 Ok
```

```json
{
  "total": {
    "amount": 816000,
    "paidAmount": 0,
    "count": 408
  },
  "payments": [
    {
      "status": "1",
      "description": "Aguardando pagamento",
      "count": 400,
      "amount": 800000
    },
    {
      "status": "2",
      "description": "Pago",
      "count": 3,
      "amount": 6000,
      "paidAmount": 0
    },
    {
      "status": "3",
      "description": "Marcado como pago",
      "count": 2,
      "amount": 4000
    },
    {
      "status": "4",
      "description": "Não Pago",
      "count": 1,
      "amount": 2000
    },
    {
      "status": "5",
      "description": "Cancelado",
      "count": 1,
      "amount": 2000
    },
    {
      "status": "6",
      "description": "Expirado",
      "count": 1,
      "amount": 2000
    }
  ]
}
```

Este endpoint lista os saldos acumulados de cobranças.

### Requisição HTTP

`GET https://sandbox.easypagamentos.com.br/api/v1/reports/invoices/balances?startDate=2019-04-02&endDate=2019-04-30`

### Parâmetros Query

| Parâmetro | Descrição                                                                                          | Obrigatório |
| --------- | -------------------------------------------------------------------------------------------------- | ----------- |
| startDate | Data inicial de referência em formato ISO: YYYY-MM-DD                                              | Não         |
| endDate   | Data final de referência em formato ISO: YYYY-MM-DD                                                | Não         |
| docNumber | CPF/CNPJ do cliente. Pode ser utilizado para exibir os saldos de cobranças referentes a um cliente | Não         |
