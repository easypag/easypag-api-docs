---
title: Referência API v1
language_tabs:
  - json
toc_footers: null
includes:
  - errors
search: true
---

# Introdução

## Passo a Passo

Integre seu software e tenha acesso às funcionalidades da Easypag, de forma simples e prática, com API moderna e respostas rápidas.

Confira o [passo a passo](https://ajuda.easypag.com.br/pt-BR/articles/3516299-passo-a-passo-integracao-via-api-easypag) que fizemos para guía-lo com a integração.

## Ambientes da API

Ao utilizar a API, cerfique-se de:

- Validar e testar sua integração em ambiente Sandbox, antes de utilizar o ambiente de Produção.
- Utilizar conexão segura (https) em todos as urls/endpoints.

Ambiente | URL                                     | Descrição
-------- | --------------------------------------- | ----------------------------------
Sandbox  | <https://sandbox.easypag.com.br/api/v1> | Ambiente para testes e homologação
Produção | <https://app.easypag.com.br/api/v1>     | Ambiente de produção

## Cadastro Sandbox

<aside class="notice">
Não utilize dados reais no ambiente Sandbox. As cobranças geradas não têm valor. Os disparos de email estão ativos para o Sandbox, por isso evite utilizar endereços de email de clientes reais.
</aside>

Para efetuar o cadastro em nosso ambiente de testes Sandbox, [clique aqui](https://sandbox.easypag.com.br/#/cadastrar)

## Autenticação Basic

Todos os endpoints da API são autenticados via Basic Auth. Confira em nosso [passo a passo](https://ajuda.easypag.com.br/pt-BR/articles/3516299-passo-a-passo-integracao-via-api-easypag) como obter e utilizar as credenciais.

### Parâmetros Basic

Parâmetro | Descrição                               | Obrigatório
--------- | --------------------------------------- | -----------
Username  | O Client ID da sua conta de usuário     | Sim
Password  | O Client Secret da sua conta de usuário | Sim

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
      "id": "8e320b8e-7709-4a5a-b763-e66e6bc4d4b1",
      "name": "PESSOA FÍSICA",
      "reference": null,
      "email": "pessoafisica@email.com",
      "phoneNumber": "5599999999999",
      "doc": {
        "type": "cpf",
        "number": "19953274096"
      },
      "address": {
        "cep": "79106320",
        "uf": "MS",
        "city": "Campo Grande",
        "area": "Jardim Itália",
        "addressLine1": "Rua Diva Lemos Albertini",
        "addressLine2": null,
        "streetNumber": "9999"
      },
      "createdAt": "2019-11-06T12:46:24.000Z",
      "updatedAt": "2019-11-06T12:46:24.000Z"
    },
    {
      "id": "ed31dfe8-3941-4fd0-b6cd-85c4e5f22f62",
      "name": "PESSOA JURÍDICA LTDA",
      "reference": null,
      "email": "pessoajuridica@email.com",
      "phoneNumber": "5599999999999",
      "doc": {
        "type": "cnpj",
        "number": "76336239000107"
      },
      "address": {
        "cep": "03307020",
        "uf": "SP",
        "city": "São Paulo",
        "area": "Tatuapé",
        "addressLine1": "Rua Lourenço Correa",
        "addressLine2": null,
        "streetNumber": "470"
      },
      "createdAt": "2019-11-06T12:45:03.000Z",
      "updatedAt": "2019-11-06T12:45:03.000Z"
    }
  ]
}
```

Este endpoint lista todos os clientes.

### Requisição HTTP

`GET https://sandbox.easypag.com.br/api/v1/customers`

### Parâmetros Query

Parâmetro     | Descrição                                                                                                                                                                                                 | Obrigatório
------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -----------
page          | Número da página a ser retornada                                                                                                                                                                          | Não
limit         | Número itens a ser retornado por página. Valor mínimo: 1, valor máximo: 100\. Caso não seja infromado, será aplicado o valor default de 100 itens                                                         | Não
search        | Parâmetro que recebe texto e efetua pesquisa nos campos de nome, email e referência do cliente                                                                                                            | Não
docNumber     | Parâmetro que busca cliente a partir do cpf/cnpj. Pode ser informado com ou sem máscara.                                                                                                                  | Não
sortBy        | Um hash contendo na chave, o nome do campo para ordenação e o valor DESC ou ASC para descendente e ascendente, respectivamente. Exemplos: sortBy[name]=ASC, sortBy[createdAt]=ASC, sortBy[updatedAt]=DESC | Não
createdAtFrom | Filtra registros criados a partir da data passada no parâmetro. Formato (AAAA-MM-DDThh:mm:ss)                                                                                                             | Não
createdAtTo   | Filtra registros criados até esta data passada no parâmetro. Formato (AAAA-MM-DDThh:mm:ss)                                                                                                                | Não
updatedSince  | Filtra registros atualizados desde o valor passado no parâmetro. Formato (AAAA-MM-DDThh:mm:ss)                                                                                                            | Não

## Listar um cliente

> Resposta

```http
HTTP/1.1 200 Ok
```

```json
{
  "id": "ed31dfe8-3941-4fd0-b6cd-85c4e5f22f62",
  "name": "PESSOA JURÍDICA LTDA",
  "reference": null,
  "email": "pessoajuridica@email.com",
  "phoneNumber": "5599999999999",
  "doc": {
    "type": "cnpj",
    "number": "76336239000107"
  },
  "address": {
    "cep": "03307020",
    "uf": "SP",
    "city": "São Paulo",
    "area": "Tatuapé",
    "addressLine1": "Rua Lourenço Correa",
    "addressLine2": null,
    "streetNumber": "470"
  },
  "createdAt": "2019-11-06T12:45:03.000Z",
  "updatedAt": "2019-11-06T12:45:03.000Z"
}
```

Este endpoint lista um cliente especificado.

### Requisição HTTP

`GET https://sandbox.easypag.com.br/api/v1/customers/:id`

### Parâmetros URL

Parâmetro | Descrição       | Obrigatório
--------- | --------------- | -----------
ID        | O ID do cliente | Sim

## Criar um cliente

> Requisição

```json
{
  "name": "PESSOA JURÍDICA LTDA",
  "email": "pessoajuridica@email.com",
  "phoneNumber": "5599999999999",
  "docNumber": "76336239000107",
  "address": {
    "cep": "03307020",
    "uf": "SP",
    "city": "São Paulo",
    "area": "Tatuapé",
    "addressLine1": "Rua Lourenço Correa",
    "streetNumber": "470"
  }
}
```

> Resposta

```http
HTTP/1.1 201 Created
```

```json
{
  "id": "5451d603-53b6-48b4-a38a-c203ac9c5c5d",
  "name": "PESSOA JURÍDICA LTDA",
  "reference": null,
  "email": "pessoajuridica@email.com",
  "phoneNumber": "5599999999999",
  "doc": {
    "type": "cnpj",
    "number": "76336239000107"
  },
  "address": {
    "cep": "03307020",
    "uf": "SP",
    "city": "São Paulo",
    "area": "Tatuapé",
    "addressLine1": "Rua Lourenço Correa",
    "streetNumber": "470"
  },
  "createdAt": "2019-11-06T12:57:45.997Z",
  "updatedAt": "2019-11-06T12:57:45.997Z"
}
```

Este endpoint cria um cliente.

### Requisição HTTP

`POST https://sandbox.easypag.com.br/api/v1/customers`

### Parâmetros Body

Parâmetro            | Descrição                                                                                                                                    | Tipo   | Obrigatório
-------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- | ------ | -----------
name                 | O nome do cliente                                                                                                                            | String | Sim
reference            | Referência externa do cliente, para controle do seu sistema.                                                                                 | String | Não
email                | Endereço de email do cliente                                                                                                                 | String | Sim
phoneNumber          | Número de telefone do cliente. Deve seguir o seguinte formato: `55`, seguido do número DDD, seguido do telefone, por exemplo: `551199999999` | String | Sim
docNumber            | Número de CPF/CNPJ do cliente. Pode ser informado com ou sem máscara                                                                         | String | Sim
address              | Objeto contendo informações de endereço do cliente                                                                                           | Object | Sim
address.cep          | Número do Cep do endereço. Pode ser com ou sem máscara                                                                                       | String | Sim
address.uf           | Sigle UF do endereço                                                                                                                         | String | Sim
address.city         | Cidade do endereço                                                                                                                           | String | Sim
address.area         | Bairro do endereço                                                                                                                           | String | Sim
address.addressLine1 | Logradouro do endereço                                                                                                                       | String | Sim
address.addressLine2 | Complemento do endereço                                                                                                                      | String | Não
address.streetNumber | Número do logradouro                                                                                                                         | String | Não

## Editar um cliente

> Requisição

```json
{
  "email": "pessoajuridica2@email.com",
  "phoneNumber": "5511888888888"
}
```

> Resposta

```http
HTTP/1.1 200 Ok
```

```json
{
  "id": "5451d603-53b6-48b4-a38a-c203ac9c5c5d",
  "name": "PESSOA JURÍDICA LTDA",
  "reference": null,
  "email": "pessoajuridica2@email.com",
  "phoneNumber": "5511888888888",
  "doc": {
    "type": "cnpj",
    "number": "76336239000107"
  },
  "address": {
    "cep": "03307020",
    "uf": "SP",
    "city": "São Paulo",
    "area": "Tatuapé",
    "addressLine1": "Rua Lourenço Correa",
    "addressLine2": null,
    "streetNumber": "470"
  },
  "createdAt": "2019-11-06T12:57:45.000Z",
  "updatedAt": "2019-11-06T13:01:02.000Z"
}
```

Este endpoint edita um cliente.

### Requisição HTTP

`PUT https://sandbox.easypag.com.br/api/v1/customers/:id`

### Parâmetros URL

Parâmetro | Descrição       | Obrigatório
--------- | --------------- | -----------
ID        | O ID do cliente | Sim

<aside class="notice">
Você pode informar apenas os parâmetros que deseja atualizar.
</aside>

### Parâmetros Body

Parâmetro            | Descrição                                                                                                                                    | Tipo   | Obrigatório
-------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- | ------ | -----------
name                 | O nome do cliente                                                                                                                            | String | Não
reference            | Referência externa do cliente, para controle do seu sistema.                                                                                 | String | Não
email                | Endereço de email do cliente                                                                                                                 | String | Não
phoneNumber          | Número de telefone do cliente. Deve seguir o seguinte formato: `55`, seguido do número DDD, seguido do telefone, por exemplo: `551199999999` | String | Não
docNumber            | Número de CPF/CNPJ do cliente. Pode ser informado com ou sem máscara                                                                         | String | Não
address              | Objeto contendo informações de endereço do cliente                                                                                           | Object | Não
address.cep          | Número do Cep do endereço. Pode ser com ou sem máscara                                                                                       | String | Não
address.uf           | Sigle UF do endereço                                                                                                                         | String | Não
address.city         | Cidade do endereço                                                                                                                           | String | Não
address.area         | Bairro do endereço                                                                                                                           | String | Não
address.addressLine1 | Logradouro do endereço                                                                                                                       | String | Não
address.addressLine2 | Complemento do endereço                                                                                                                      | String | Não
address.streetNumber | Número do logradouro                                                                                                                         | String | Não

## Deletar um cliente

> Resposta

```http
HTTP/1.1 204 No Content
```

Este endpoint deleta um cliente especificado.

### Requisição HTTP

`DELETE https://sandbox.easypag.com.br/api/v1/customers/:id`

### Parâmetros URL

Parâmetro | Descrição       | Obrigatório
--------- | --------------- | -----------
ID        | O ID do cliente | Sim

# Cobranças (Boletos)

## Status de Cobrança

O parâmetro `payment.status`, retornado no objeto de resposta, contém o status do pagamento da cobrança.

Cód. Status | Status                                 | Descrição
----------- | -------------------------------------- | -----------------------------------------------------------------------------------------------------------------------------------
1           | Aguardando pagamento                   | Status inicial quando a cobrança é criada.
2           | Cobrança paga                          | As cobranças que foram pagas são atualizadas como pagas no dia útil seguinte ao pagamento do boleto.
3           | Cobrança manualmente marcada como paga | As cobranças que foram manualmente marcadas como pagas têm o seu status atualizado e o seu boleto automaticamente baixado.
4           | Cobrança atrasada                      | As cobranças que não foram pagas são automaticamente atualizadas como atrasadas.
5           | Cobrança cancelada pelo usuário        | As cobranças que foram manualmente canceladas, têm o seu status atualizado para cancelada e o seu boleto automaticamente baixado.
6           | Cobrança expirada                      | As cobranças que não foram pagas 30 dias após o vencimento têm o seu boleto baixado pelo banco e o status atualizado para Expirada.

## Simular status de pagamento da Cobrança

> Requisição

```json
{
  "paymentStatus": "2"
}
```

> Resposta

```http
HTTP/1.1 200 Ok
```

```json
{
  "id": "c2fb02bb-e4bc-44db-a43b-bd31258a3cf3",
  "reference": null,
  "dueDate": "2019-12-31",
  "amount": 2000,
  "paidAmount": 2000,
  "paidAt": "2019-11-06T19:24:59.000Z",
  "currency": "BRL",
  "items": [
    {
      "id": "4b4e7cd4-bb6a-4a25-b2f8-1e76e2494851",
      "description": "Item - 3",
      "quantity": 1,
      "price": 1000,
      "createdAt": "2019-11-06T13:22:30.000Z",
      "updatedAt": "2019-11-06T13:22:30.000Z",
      "invoiceId": "c2fb02bb-e4bc-44db-a43b-bd31258a3cf3"
    },
    {
      "id": "94637696-08dc-401d-98dd-a96ef4fa807a",
      "description": "Item - 1",
      "quantity": 1,
      "price": 1000,
      "createdAt": "2019-11-06T13:22:30.000Z",
      "updatedAt": "2019-11-06T13:22:30.000Z",
      "invoiceId": "c2fb02bb-e4bc-44db-a43b-bd31258a3cf3"
    },
    {
      "id": "ad3d7333-cdc1-4695-8199-a529b90fd8ee",
      "description": "Item - 2",
      "quantity": 1,
      "price": 1000,
      "createdAt": "2019-11-06T13:22:30.000Z",
      "updatedAt": "2019-11-06T13:22:30.000Z",
      "invoiceId": "c2fb02bb-e4bc-44db-a43b-bd31258a3cf3"
    }
  ],
  "lastMessageEvent": null,
  "lastDateMessageEvent": null,
  "itemsDiscount": {
    "amount": 1000
  },
  "earlyDiscount": {
    "percentage": 10,
    "expiryDate": "2019-12-30",
    "earlyDays": 1
  },
  "interest": {
    "percentage": 1
  },
  "fine": {
    "lateDays": 10,
    "fineStartDate": "2020-01-10",
    "percentage": 2
  },
  "customer": {
    "name": "PESSOA JURÍDICA LTDA",
    "email": "pessoajuridica@email.com",
    "phoneNumber": "5599999999999",
    "doc": {
      "type": "cnpj",
      "number": "76336239000107"
    },
    "address": {
      "cep": "03307020",
      "uf": "SP",
      "city": "São Paulo",
      "area": "Tatuapé",
      "addressLine1": "Rua Lourenço Correa",
      "streetNumber": "470"
    }
  },
  "notifications": {
    "sendOnCreate": true,
    "reminders": {
      "cc": {
        "emails": []
      },
      "before": {
        "send": true,
        "days": 3,
        "time": "06:00:00"
      },
      "after": {
        "send": true,
        "days": "3",
        "time": "06:00:00"
      },
      "expiration": {
        "send": true,
        "time": "06:00:00"
      }
    },
    "types": {
      "email": true,
      "sms": false,
      "whatsapp": false
    }
  },
  "instructionsMsg": "Teste de Instrução atualizado",
  "notes": "Teste de Observação atualizado",
  "boleto": {
    "digitableLine": "00190.00009 02625.444209 58002.630174 2 81200000002000",
    "barCodeNumber": "00192812000000020000000002625444205800263017"
  },
  "invoiceUrl": "https://sandbox.easypag.com.br/invoices/c2fb02bb-e4bc-44db-a43b-bd31258a3cf3/view/boleto",
  "payment": {
    "status": "2",
    "description": "Pago"
  },
  "events": [
    {
      "id": "0b893305-7ee8-42f9-ba86-a6e4b3a3f021",
      "type": "INVOICE.CREATED",
      "createdAt": "2019-11-06T13:22:30.000Z",
      "updatedAt": "2019-11-06T13:22:30.000Z",
      "invoiceId": "c2fb02bb-e4bc-44db-a43b-bd31258a3cf3"
    },
    {
      "id": "e0011939-1160-488c-8efd-bb2cefee0b41",
      "type": "INVOICE.PAID",
      "createdAt": "2019-11-06T19:24:59.000Z",
      "updatedAt": "2019-11-06T19:24:59.000Z",
      "invoiceId": "c2fb02bb-e4bc-44db-a43b-bd31258a3cf3"
    }
  ],
  "createdAt": "2019-11-06T13:22:30.000Z",
  "updatedAt": "2019-11-06T19:24:59.000Z"
}
```

<aside class="warn">
Este endpoint é disponibilizado somente em ambiente Sandbox, para testes.
</aside>

Este endpoint simula o status de pagamento, gera os eventos e notificações webhooks referentes a cobrança.

### Requisição HTTP

`POST https://sandbox.easypag.com.br/api/v1/invoices/:id/simulate`

### Parâmetros Body

Parâmetro     | Descrição                                                                                                                           | Tipo   | Obrigatório
------------- | ----------------------------------------------------------------------------------------------------------------------------------- | ------ | -----------
paymentStatus | Status de pagamento da cobrança. Deve ser diferente do status atual. Consulte a [Lista de status](#status-de-cobranca) disponíveis. | String | Sim

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
  "count": 1,
  "summary": {
    "amount": 2000,
    "paidAmount": 0
  },
  "results": [
    {
      "id": "c2fb02bb-e4bc-44db-a43b-bd31258a3cf3",
      "reference": null,
      "dueDate": "2019-12-31",
      "amount": 2000,
      "paidAmount": 0,
      "paidAt": null,
      "currency": "BRL",
      "items": [
        {
          "id": "94637696-08dc-401d-98dd-a96ef4fa807a",
          "description": "Item - 1",
          "quantity": 1,
          "price": 1000,
          "createdAt": "2019-11-06T13:22:30.000Z",
          "updatedAt": "2019-11-06T13:22:30.000Z",
          "invoiceId": "c2fb02bb-e4bc-44db-a43b-bd31258a3cf3"
        },
        {
          "id": "4b4e7cd4-bb6a-4a25-b2f8-1e76e2494851",
          "description": "Item - 3",
          "quantity": 1,
          "price": 1000,
          "createdAt": "2019-11-06T13:22:30.000Z",
          "updatedAt": "2019-11-06T13:22:30.000Z",
          "invoiceId": "c2fb02bb-e4bc-44db-a43b-bd31258a3cf3"
        },
        {
          "id": "ad3d7333-cdc1-4695-8199-a529b90fd8ee",
          "description": "Item - 2",
          "quantity": 1,
          "price": 1000,
          "createdAt": "2019-11-06T13:22:30.000Z",
          "updatedAt": "2019-11-06T13:22:30.000Z",
          "invoiceId": "c2fb02bb-e4bc-44db-a43b-bd31258a3cf3"
        }
      ],
      "lastMessageEvent": null,
      "lastDateMessageEvent": null,
      "invoiceMessageEvent": [],
      "itemsDiscount": {
        "amount": 1000
      },
      "earlyDiscount": {
        "percentage": 10,
        "expiryDate": "2019-12-30",
        "earlyDays": 1
      },
      "interest": {
        "percentage": 1
      },
      "fine": {
        "lateDays": 10,
        "fineStartDate": "2020-01-10",
        "percentage": 2
      },
      "customer": {
        "name": "PESSOA JURÍDICA LTDA",
        "email": "pessoajuridica@email.com",
        "phoneNumber": "5599999999999",
        "doc": {
          "type": "cnpj",
          "number": "76336239000107"
        },
        "address": {
          "cep": "03307020",
          "uf": "SP",
          "city": "São Paulo",
          "area": "Tatuapé",
          "addressLine1": "Rua Lourenço Correa",
          "streetNumber": "470"
        }
      },
      "notifications": {
        "sendOnCreate": true,
        "reminders": {
          "cc": {
            "emails": []
          },
          "before": {
            "send": true,
            "days": 3,
            "time": "06:00:00"
          },
          "after": {
            "send": true,
            "days": "3",
            "time": "06:00:00"
          },
          "expiration": {
            "send": true,
            "time": "06:00:00"
          }
        },
        "types": {
          "email": true,
          "sms": false,
          "whatsapp": false
        }
      },
      "instructionsMsg": "Teste de Instrução atualizado",
      "notes": "Teste de Observação atualizado",
      "boleto": {
        "digitableLine": "00190.00009 02625.444209 58002.630174 2 81200000002000",
        "barCodeNumber": "00192812000000020000000002625444205800263017"
      },
      "invoiceUrl": "https://sandbox.easypag.com.br/invoices/c2fb02bb-e4bc-44db-a43b-bd31258a3cf3/view/boleto",
      "payment": {
        "status": "1",
        "description": "Aguardando pagamento"
      },
      "events": [
        {
          "id": "a895e595-4a02-4777-ae02-da15c3006886",
          "type": "INVOICE.UPDATED",
          "createdAt": "2019-11-06T18:56:37.000Z",
          "updatedAt": "2019-11-06T18:56:37.000Z",
          "invoiceId": "c2fb02bb-e4bc-44db-a43b-bd31258a3cf3"
        },
        {
          "id": "0b893305-7ee8-42f9-ba86-a6e4b3a3f021",
          "type": "INVOICE.CREATED",
          "createdAt": "2019-11-06T13:22:30.000Z",
          "updatedAt": "2019-11-06T13:22:30.000Z",
          "invoiceId": "c2fb02bb-e4bc-44db-a43b-bd31258a3cf3"
        }
      ],
      "createdAt": "2019-11-06T13:22:30.000Z",
      "updatedAt": "2019-11-06T18:56:37.000Z"
    }
  ]
}
```

Este endpoint lista todas as cobranças.

### Requisição HTTP

`GET https://sandbox.easypag.com.br/api/v1/invoices`

### Parâmetros Query

Parâmetro     | Descrição                                                                                                                                                                                                                                                    | Obrigatório
------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -----------
page          | Número da página a ser retornada                                                                                                                                                                                                                             | Não
limit         | Número itens a ser retornado por página. Valor mínimo: 1, valor máximo: 100\. Caso não seja infromado, será aplicado o valor default de 100 itens                                                                                                            | Não
search        | Parâmetro que recebe texto e efetua pesquisa nos campos de nome do cliente, email do cliente, cpf/cnpj do cliente, observações e referência da cobrança                                                                                                      | Não
docNumber     | Parâmetro que filtra cobranças a partir de cpf/cnpj. Pode ser informado com ou sem máscara                                                                                                                                                                   | Não
paymentStatus | Parâmetro que filtra cobranças por um ou mais status de pagamento separados por vírgula                                                                                                                                                                      | Não
sortBy        | Um hash contendo na chave, o nome do campo para ordenação e o valor DESC ou ASC para descendente e ascendente, respectivamente. Exemplos: sortBy[dueDate]=DESC, sortBy[amount]=DESC, sortBy[customerName]=ASC, sortBy[createdAt]=ASC, sortBy[updatedAt]=DESC | Não
createdAtFrom | Filtra registros criados a partir da data passada no parâmetro. Formato (AAAA-MM-DDThh:mm:ss)                                                                                                                                                                | Não
createdAtTo   | Filtra registros criados até esta data passada no parâmetro. Formato (AAAA-MM-DDThh:mm:ss)                                                                                                                                                                   | Não
updatedSince  | Filtra registros atualizados desde o valor passado no parâmetro. Formato (AAAA-MM-DDThh:mm:ss)                                                                                                                                                               | Não
paymentStatus | Filtra registros que possuam os status de pagamento informados. Formato (Código status do pagamento. Caso seja mais de um status, separar por vírgula.) Exemplo: paymentStatus=1,2,3 ou paymentStatus=1                                                      | Não

## Listar uma cobrança

> Resposta

```http
HTTP/1.1 200 Ok
```

```json
{
  "id": "c2fb02bb-e4bc-44db-a43b-bd31258a3cf3",
  "reference": null,
  "dueDate": "2019-12-31",
  "amount": 2000,
  "paidAmount": 0,
  "paidAt": null,
  "currency": "BRL",
  "items": [
    {
      "id": "94637696-08dc-401d-98dd-a96ef4fa807a",
      "description": "Item - 1",
      "quantity": 1,
      "price": 1000,
      "createdAt": "2019-11-06T13:22:30.000Z",
      "updatedAt": "2019-11-06T13:22:30.000Z",
      "invoiceId": "c2fb02bb-e4bc-44db-a43b-bd31258a3cf3"
    },
    {
      "id": "ad3d7333-cdc1-4695-8199-a529b90fd8ee",
      "description": "Item - 2",
      "quantity": 1,
      "price": 1000,
      "createdAt": "2019-11-06T13:22:30.000Z",
      "updatedAt": "2019-11-06T13:22:30.000Z",
      "invoiceId": "c2fb02bb-e4bc-44db-a43b-bd31258a3cf3"
    },
    {
      "id": "4b4e7cd4-bb6a-4a25-b2f8-1e76e2494851",
      "description": "Item - 3",
      "quantity": 1,
      "price": 1000,
      "createdAt": "2019-11-06T13:22:30.000Z",
      "updatedAt": "2019-11-06T13:22:30.000Z",
      "invoiceId": "c2fb02bb-e4bc-44db-a43b-bd31258a3cf3"
    }
  ],
  "lastMessageEvent": null,
  "lastDateMessageEvent": null,
  "invoiceMessageEvent": [],
  "itemsDiscount": {
    "amount": 1000
  },
  "earlyDiscount": {
    "percentage": 10,
    "expiryDate": "2019-12-30",
    "earlyDays": 1
  },
  "interest": {
    "percentage": 1
  },
  "fine": {
    "lateDays": 10,
    "fineStartDate": "2020-01-10",
    "percentage": 2
  },
  "customer": {
    "name": "PESSOA JURÍDICA LTDA",
    "email": "pessoajuridica@email.com",
    "phoneNumber": "5599999999999",
    "doc": {
      "type": "cnpj",
      "number": "76336239000107"
    },
    "address": {
      "cep": "03307020",
      "uf": "SP",
      "city": "São Paulo",
      "area": "Tatuapé",
      "addressLine1": "Rua Lourenço Correa",
      "streetNumber": "470"
    }
  },
  "notifications": {
    "sendOnCreate": true,
    "reminders": {
      "cc": {
        "emails": []
      },
      "before": {
        "send": true,
        "days": 3,
        "time": "06:00:00"
      },
      "after": {
        "send": true,
        "days": "3",
        "time": "06:00:00"
      },
      "expiration": {
        "send": true,
        "time": "06:00:00"
      }
    },
    "types": {
      "email": true,
      "sms": false,
      "whatsapp": false
    }
  },
  "instructionsMsg": "Teste de Instrução atualizado",
  "notes": "Teste de Observação atualizado",
  "boleto": {
    "digitableLine": "00190.00009 02625.444209 58002.630174 2 81200000002000",
    "barCodeNumber": "00192812000000020000000002625444205800263017"
  },
  "invoiceUrl": "https://sandbox.easypag.com.br/invoices/c2fb02bb-e4bc-44db-a43b-bd31258a3cf3/view/boleto",
  "payment": {
    "status": "1",
    "description": "Aguardando pagamento"
  },
  "events": [
    {
      "id": "a895e595-4a02-4777-ae02-da15c3006886",
      "type": "INVOICE.UPDATED",
      "createdAt": "2019-11-06T18:56:37.000Z",
      "updatedAt": "2019-11-06T18:56:37.000Z",
      "invoiceId": "c2fb02bb-e4bc-44db-a43b-bd31258a3cf3"
    },
    {
      "id": "0b893305-7ee8-42f9-ba86-a6e4b3a3f021",
      "type": "INVOICE.CREATED",
      "createdAt": "2019-11-06T13:22:30.000Z",
      "updatedAt": "2019-11-06T13:22:30.000Z",
      "invoiceId": "c2fb02bb-e4bc-44db-a43b-bd31258a3cf3"
    }
  ],
  "createdAt": "2019-11-06T13:22:30.000Z",
  "updatedAt": "2019-11-06T18:56:37.000Z"
}
```

Este endpoint lista uma cobrança.

### Requisição HTTP

`GET https://sandbox.easypag.com.br/api/v1/invoices/:id`

### Parâmetros URL

Parâmetro | Descrição        | Obrigatório
--------- | ---------------- | -----------
ID        | O ID da cobrança | Sim

## Criar uma cobrança com cliente existente

> Requisição

```json
{
  "dueDate": "2019-11-30",
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
    "percentage": 5,
    "lateDays": 7
  },
  "items": [
    {
      "description": "Item - 1",
      "quantity": 1,
      "price": 1000
    },
    {
      "description": "Item - 2",
      "quantity": 1,
      "price": 1000
    },
    {
      "description": "Item - 3",
      "quantity": 1,
      "price": 1000
    }
  ],
  "customer": {
    "id": "2814eae0-858c-4ba2-a2da-5dbc2a95871f"
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
  "id": "c2fb02bb-e4bc-44db-a43b-bd31258a3cf3",
  "reference": null,
  "dueDate": "2019-11-30",
  "amount": 2000,
  "paidAmount": 0,
  "paidAt": null,
  "currency": "BRL",
  "items": [
    {
      "id": "4b4e7cd4-bb6a-4a25-b2f8-1e76e2494851",
      "description": "Item - 3",
      "quantity": 1,
      "price": 1000,
      "createdAt": "2019-11-06T13:22:30.000Z",
      "updatedAt": "2019-11-06T13:22:30.000Z",
      "invoiceId": "c2fb02bb-e4bc-44db-a43b-bd31258a3cf3"
    },
    {
      "id": "94637696-08dc-401d-98dd-a96ef4fa807a",
      "description": "Item - 1",
      "quantity": 1,
      "price": 1000,
      "createdAt": "2019-11-06T13:22:30.000Z",
      "updatedAt": "2019-11-06T13:22:30.000Z",
      "invoiceId": "c2fb02bb-e4bc-44db-a43b-bd31258a3cf3"
    },
    {
      "id": "ad3d7333-cdc1-4695-8199-a529b90fd8ee",
      "description": "Item - 2",
      "quantity": 1,
      "price": 1000,
      "createdAt": "2019-11-06T13:22:30.000Z",
      "updatedAt": "2019-11-06T13:22:30.000Z",
      "invoiceId": "c2fb02bb-e4bc-44db-a43b-bd31258a3cf3"
    }
  ],
  "lastMessageEvent": null,
  "lastDateMessageEvent": null,
  "itemsDiscount": {
    "amount": 1000
  },
  "earlyDiscount": {
    "percentage": 4.75,
    "expiryDate": "2019-11-29",
    "earlyDays": 1
  },
  "interest": {
    "percentage": 1
  },
  "fine": {
    "lateDays": 7,
    "fineStartDate": "2019-12-07",
    "percentage": 5
  },
  "customer": {
    "name": "PESSOA JURÍDICA LTDA",
    "email": "pessoajuridica@email.com",
    "phoneNumber": "5599999999999",
    "doc": {
      "type": "cnpj",
      "number": "76336239000107"
    },
    "address": {
      "cep": "03307020",
      "uf": "SP",
      "city": "São Paulo",
      "area": "Tatuapé",
      "addressLine1": "Rua Lourenço Correa",
      "streetNumber": "470"
    }
  },
  "notifications": {
    "sendOnCreate": true,
    "reminders": {
      "cc": {
        "emails": ["usuario2@email.com", "usuario1@email.com"]
      },
      "before": {
        "send": true,
        "days": 1,
        "time": "08:00:00"
      },
      "after": {
        "send": true,
        "days": "2",
        "time": "08:00:00"
      },
      "expiration": {
        "send": true,
        "time": "08:00:00"
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
    "digitableLine": "00190.00009 02625.444209 58002.629176 7 80890000002000",
    "barCodeNumber": "00197808900000020000000002625444205800262917"
  },
  "invoiceUrl": "https://sandbox.easypag.com.br/invoices/c2fb02bb-e4bc-44db-a43b-bd31258a3cf3/view/boleto",
  "payment": {
    "status": "1",
    "description": "Aguardando pagamento"
  },
  "events": [
    {
      "id": "0b893305-7ee8-42f9-ba86-a6e4b3a3f021",
      "type": "INVOICE.CREATED",
      "createdAt": "2019-11-06T13:22:30.000Z",
      "updatedAt": "2019-11-06T13:22:30.000Z",
      "invoiceId": "c2fb02bb-e4bc-44db-a43b-bd31258a3cf3"
    }
  ],
  "createdAt": "2019-11-06T13:22:30.000Z",
  "updatedAt": "2019-11-06T13:22:30.000Z"
}
```

Este endpoint cria uma cobrança para um cliente pré-cadastrado no endpoint de clientes.

### Requisição HTTP

`POST https://sandbox.easypag.com.br/api/v1/invoices`

### Parâmetros Body

Parâmetro                     | Descrição                                                                                                                                                                          | Tipo    | Obrigatório
----------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- | ----------------------------------------------
dueDate                       | Data de vencimento no formato ISO: YYYY-MM-DD. A data deve ser igual ou posterior a data atual                                                                                     | String  | Sim
reference                     | Referência externa da cobrança, para controle do seu sistema.                                                                                                                      | String  | Não
items                         | Array de objetos de itens de cobrança. Deve conter ao menos um item de cobrança                                                                                                    | Object  | Sim
items.description             | Nome ou descrição do item de cobrança                                                                                                                                              | String  | Sim
items.quantity                | Quantidade do item de cobrança                                                                                                                                                     | Integer | Sim
items.price                   | Valor em centavos do item de cobrança                                                                                                                                              | Integer | Sim
itemsDiscount                 | Objeto de descontos dos itens de cobrança                                                                                                                                          | Object  | Não
itemsDiscount.amount          | Valor em centavos do desconto imediato a ser concedido sobre o valor total dos itens                                                                                               | Integer | Se não informado percentual do desconto
itemsDiscount.percentage      | Valor percentual do desconto imediato a ser concedido sobre o valor total dos itens                                                                                                | Double  | Se não informado valor em centavos do desconto
earlyDiscount                 | Objeto de descontos por antecipação de pagamento                                                                                                                                   | Object  | Não
earlyDiscount.amount          | Valor em centavos do desconto a ser concedido sob o valor total dos itens por pagamento antecipado                                                                                 | Integer | Se não informado percentual do desconto
earlyDiscount.percentage      | Valor percentual do desconto a ser concedido sob o valor total dos itens por pagamento antecipado                                                                                  | Double  | Se não informado valor em centavos do desconto
earlyDiscount.earlyDays       | Número de dias de antecedência para aplicar o desconto por antecipação.                                                                                                            | Integer | Se informado o objeto de desconto
interest                      | Objeto de juros da cobrança                                                                                                                                                        | Object  | Não
interest.percentage           | Valor percentual do juros a ser cobrado sob o valor total dos itens por pagamento atrasado. Deve ser maior que 0 e menor ou igual a 1                                              | Double  | Se informado o objeto de juros
fine                          | Objeto de multa da cobrança                                                                                                                                                        | Object  | Não
fine.percentage               | Valor percentual da multa a ser cobrado sob o valor total dos itens por pagamento atrasado. Deve ser maior que 0 e menor ou igual a 10                                             | Double  | Se informado o objeto de multa
fine.lateDays                 | Número de dias de atraso para aplicação da multa. Deve ser maior que 0 e menor que 30                                                                                              | Integer | Se informado o objeto de multa
customer                      | Objeto do Cliente                                                                                                                                                                  | Object  | Sim
customer.id                   | ID do Cliente existente para qual a cobrança será criada                                                                                                                           | String  | Sim
notifications                 | Objeto de notificações                                                                                                                                                             | Object  | Não
notifications.cc              | Objeto de envio de cópias                                                                                                                                                          | Object  | Não
notifications.cc.emails       | Array de emails, que também recebem como cópia as notificações da cobrança.                                                                                                        | Array   | Não
notifications.sendOnCreate    | Define se a cobrança é enviada para o(s) cliente(s) logo após ser criada                                                                                                           | Boolean | Não
notifications.reminders       | Objeto de lembretes de cobrança                                                                                                                                                    | Object  | Não
notifications.before          | Objeto de lembrete da data do vencimento                                                                                                                                           | Object  | Não
notifications.before.send     | Define se o lembrete deve ser enviado. Default: true                                                                                                                               | Boolean | Não
notifications.before.days     | Define a quantidade de dias antes da data de vencimento que o lembrete deve ser enviado. Default: true                                                                             | Boolean | Não
notifications.before.time     | Define o horário de preferência que o lembrete deve ser enviado. O formato deve ser `HH:mm:ss` ou `HH:mm:ss A`. Exemplos: `22:00:00` ou `10:00:00 PM`. `06:00:00` ou `06:00:00 AM` | String  | Não
notifications.after           | Objeto de lembrete após data do vencimento                                                                                                                                         | Object  | Não
notifications.after.send      | Define se o lembrete deve ser enviado. Default: true                                                                                                                               | Boolean | Não
notifications.after.days      | Define a quantidade/intervalo de dias após da data de vencimento que o lembrete deve ser enviado. Default: true                                                                    | Integer | Não
notifications.after.time      | Define o horário de preferência que o lembrete deve ser enviado. O formato deve ser `HH:mm:ss` ou `HH:mm:ss A`. Exemplos: `22:00:00` ou `10:00:00 PM`. `06:00:00` ou `06:00:00 AM` | String  | Não
notifications.after.recurrent | Define se o lembrete é recorrente. Caso seja true, enviará o lembrete a cada `X` dias, conforme especificado em `notifications.after.days`                                         | Boolean | Não
notifications.expiration      | Objeto de lembrete na data do vencimento                                                                                                                                           | Object  | Não
notifications.expiration.send | Define se o lembrete deve ser enviado. Default: true                                                                                                                               | Boolean | Não
notifications.expiration.time | Define o horário de preferência que o lembrete deve ser enviado. O formato deve ser `HH:mm:ss` ou `HH:mm:ss A`. Exemplos: `22:00:00` ou `10:00:00 PM`. `06:00:00` ou `06:00:00 AM` | String  | Não
instructionsMsg               | Instruções de pagamento que constará no boleto. Deve conter no máximo 100 caracteres                                                                                               | String  | Não
notes                         | Observações sobre o pagamento. Não constará no boleto. Deve conter no máximo 100 caracteres                                                                                        | String  | Não

## Criar uma cobrança com cliente avulso

> Requisição

```json
{
  "dueDate": "2019-11-30",
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
    "percentage": 5,
    "lateDays": 7
  },
  "items": [
    {
      "description": "Item - 1",
      "quantity": 1,
      "price": 1000
    },
    {
      "description": "Item - 2",
      "quantity": 1,
      "price": 1000
    },
    {
      "description": "Item - 3",  
      "quantity": 1,
      "price": 1000
    }
  ],
  "customer": {
    "name": "PESSOA JURÍDICA LTDA",
    "email": "pessoajuridica@email.com",
    "phoneNumber": "5599999999999",
    "docNumber": "76336239000107",
    "address": {
      "cep": "03307020",
      "uf": "SP",
      "city": "São Paulo",
      "area": "Tatuapé",
      "addressLine1": "Rua Lourenço Correa",
      "streetNumber": "470"
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
  "id": "c2fb02bb-e4bc-44db-a43b-bd31258a3cf3",
  "reference": null,
  "dueDate": "2019-11-30",
  "amount": 2000,
  "paidAmount": 0,
  "paidAt": null,
  "currency": "BRL",
  "items": [
    {
      "id": "4b4e7cd4-bb6a-4a25-b2f8-1e76e2494851",
      "description": "Item - 3",
      "quantity": 1,
      "price": 1000,
      "createdAt": "2019-11-06T13:22:30.000Z",
      "updatedAt": "2019-11-06T13:22:30.000Z",
      "invoiceId": "c2fb02bb-e4bc-44db-a43b-bd31258a3cf3"
    },
    {
      "id": "94637696-08dc-401d-98dd-a96ef4fa807a",
      "description": "Item - 1",
      "quantity": 1,
      "price": 1000,
      "createdAt": "2019-11-06T13:22:30.000Z",
      "updatedAt": "2019-11-06T13:22:30.000Z",
      "invoiceId": "c2fb02bb-e4bc-44db-a43b-bd31258a3cf3"
    },
    {
      "id": "ad3d7333-cdc1-4695-8199-a529b90fd8ee",
      "description": "Item - 2",
      "quantity": 1,
      "price": 1000,
      "createdAt": "2019-11-06T13:22:30.000Z",
      "updatedAt": "2019-11-06T13:22:30.000Z",
      "invoiceId": "c2fb02bb-e4bc-44db-a43b-bd31258a3cf3"
    }
  ],
  "lastMessageEvent": null,
  "lastDateMessageEvent": null,
  "itemsDiscount": {
    "amount": 1000
  },
  "earlyDiscount": {
    "percentage": 4.75,
    "expiryDate": "2019-11-29",
    "earlyDays": 1
  },
  "interest": {
    "percentage": 1
  },
  "fine": {
    "lateDays": 7,
    "fineStartDate": "2019-12-07",
    "percentage": 5
  },
  "customer": {
    "name": "PESSOA JURÍDICA LTDA",
    "email": "pessoajuridica@email.com",
    "phoneNumber": "5599999999999",
    "doc": {
      "type": "cnpj",
      "number": "76336239000107"
    },
    "address": {
      "cep": "03307020",
      "uf": "SP",
      "city": "São Paulo",
      "area": "Tatuapé",
      "addressLine1": "Rua Lourenço Correa",
      "streetNumber": "470"
    }
  },
  "notifications": {
    "sendOnCreate": true,
    "reminders": {
      "cc": {
        "emails": ["usuario2@email.com", "usuario1@email.com"]
      },
      "before": {
        "send": true,
        "days": 1,
        "time": "08:00:00"
      },
      "after": {
        "send": true,
        "days": "2",
        "time": "08:00:00"
      },
      "expiration": {
        "send": true,
        "time": "08:00:00"
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
    "digitableLine": "00190.00009 02625.444209 58002.629176 7 80890000002000",
    "barCodeNumber": "00197808900000020000000002625444205800262917"
  },
  "invoiceUrl": "https://sandbox.easypag.com.br/invoices/c2fb02bb-e4bc-44db-a43b-bd31258a3cf3/view/boleto",
  "payment": {
    "status": "1",
    "description": "Aguardando pagamento"
  },
  "events": [
    {
      "id": "0b893305-7ee8-42f9-ba86-a6e4b3a3f021",
      "type": "INVOICE.CREATED",
      "createdAt": "2019-11-06T13:22:30.000Z",
      "updatedAt": "2019-11-06T13:22:30.000Z",
      "invoiceId": "c2fb02bb-e4bc-44db-a43b-bd31258a3cf3"
    }
  ],
  "createdAt": "2019-11-06T13:22:30.000Z",
  "updatedAt": "2019-11-06T13:22:30.000Z"
}
```

Este endpoint cria uma cobrança com dados avulsos de um cliente, sem a necessidade de tê-lo cadastrado na base. É ideal para sistemas que já possuem o controle de sua base de clientes e desejam apenas emitir as cobranças informando os dados dos seus clientes.

### Requisição HTTP

`POST https://sandbox.easypag.com.br/api/v1/invoices`

### Parâmetros Body

Parâmetro                     | Descrição                                                                                                                                                                          | Tipo    | Obrigatório
----------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- | ----------------------------------------------
dueDate                       | Data de vencimento no formato ISO: YYYY-MM-DD. A data deve ser igual ou posterior a data atual                                                                                     | String  | Sim
reference                     | Referência externa da cobrança, para controle do seu sistema.                                                                                                                      | String  | Não
items                         | Array de objetos de itens de cobrança. Deve conter ao menos um item de cobrança                                                                                                    | Object  | Sim
items.description             | Nome ou descrição do item de cobrança                                                                                                                                              | String  | Sim
items.quantity                | Quantidade do item de cobrança                                                                                                                                                     | Integer | Sim
items.price                   | Valor em centavos do item de cobrança                                                                                                                                              | Integer | Sim
itemsDiscount                 | Objeto de descontos dos itens de cobrança                                                                                                                                          | Object  | Não
itemsDiscount.amount          | Valor em centavos do desconto imediato a ser concedido sobre o valor total dos itens                                                                                               | Integer | Se não informado percentual do desconto
itemsDiscount.percentage      | Valor percentual do desconto imediato a ser concedido sobre o valor total dos itens                                                                                                | Double  | Se não informado valor em centavos do desconto
earlyDiscount                 | Objeto de descontos por antecipação de pagamento                                                                                                                                   | Object  | Não
earlyDiscount.amount          | Valor em centavos do desconto a ser concedido sob o valor total dos itens por pagamento antecipado                                                                                 | Integer | Se não informado percentual do desconto
earlyDiscount.percentage      | Valor percentual do desconto a ser concedido sob o valor total dos itens por pagamento antecipado                                                                                  | Double  | Se não informado valor em centavos do desconto
earlyDiscount.earlyDays       | Número de dias de antecedência para aplicar o desconto por antecipação.                                                                                                            | Integer | Se informado o objeto de desconto
interest                      | Objeto de juros da cobrança                                                                                                                                                        | Object  | Não
interest.percentage           | Valor percentual do juros a ser cobrado sob o valor total dos itens por pagamento atrasado. Deve ser maior que 0 e menor ou igual a 1                                              | Double  | Se informado o objeto de juros
fine                          | Objeto de multa da cobrança                                                                                                                                                        | Object  | Não
fine.percentage               | Valor percentual da multa a ser cobrado sob o valor total dos itens por pagamento atrasado. Deve ser maior que 0 e menor ou igual a 10                                             | Double  | Se informado o objeto de multa
fine.lateDays                 | Número de dias de atraso para aplicação da multa. Deve ser maior que 0 e menor que 30                                                                                              | Integer | Se informado o objeto de multa
customer                      | Objeto do Cliente                                                                                                                                                                  | Object  | Sim
customer.name                 | Nome do Cliente                                                                                                                                                                    | String  | Sim
                              |
customer.email                | Email do Cliente                                                                                                                                                                   | String  | Sim
                              |
customer.phoneNumber          | Número de telefone do cliente. Deve seguir o seguinte formato: `55`, seguido do número DDD, seguido do telefone, por exemplo: `551199999999`                                       | String  | Sim
customer.docNumber            | Número de CPF/CNPJ do cliente. Pode ser informado com ou sem máscara                                                                                                               | String  | Sim
customer.address              | Objeto contendo informações de endereço do cliente                                                                                                                                 | Object  | Sim
customer.address.cep          | Número do Cep do endereço. Pode ser com ou sem máscara                                                                                                                             | String  | Sim
customer.address.uf           | Sigle UF do endereço                                                                                                                                                               | String  | Sim
customer.address.city         | Cidade do endereço                                                                                                                                                                 | String  | Sim
customer.address.area         | Bairro do endereço                                                                                                                                                                 | String  | Sim
customer.address.addressLine1 | Logradouro do endereço                                                                                                                                                             | String  | Sim
customer.address.addressLine2 | Complemento do endereço                                                                                                                                                            | String  | Não
customer.address.streetNumber | Número do logradouro                                                                                                                                                               | String  | Não
notifications                 | Objeto de notificações                                                                                                                                                             | Object  | Não
notifications.sendOnCreate    | Define se a cobrança é enviada para o(s) cliente(s) logo após ser criada                                                                                                           | Boolean | Não
notifications.reminders       | Objeto de lembretes de cobrança                                                                                                                                                    | Object  | Não
notifications.before          | Objeto de lembrete da data do vencimento                                                                                                                                           | Object  | Não
notifications.before.send     | Define se o lembrete deve ser enviado. Default: true                                                                                                                               | Boolean | Não
notifications.before.days     | Define a quantidade de dias antes da data de vencimento que o lembrete deve ser enviado. Default: true                                                                             | Boolean | Não
notifications.before.time     | Define o horário de preferência que o lembrete deve ser enviado. O formato deve ser `HH:mm:ss` ou `HH:mm:ss A`. Exemplos: `22:00:00` ou `10:00:00 PM`. `06:00:00` ou `06:00:00 AM` | String  | Não
notifications.after           | Objeto de lembrete após data do vencimento                                                                                                                                         | Object  | Não
notifications.after.send      | Define se o lembrete deve ser enviado. Default: true                                                                                                                               | Boolean | Não
notifications.after.days      | Define a quantidade/intervalo de dias após da data de vencimento que o lembrete deve ser enviado. Default: true                                                                    | Integer | Não
notifications.after.time      | Define o horário de preferência que o lembrete deve ser enviado. O formato deve ser `HH:mm:ss` ou `HH:mm:ss A`. Exemplos: `22:00:00` ou `10:00:00 PM`. `06:00:00` ou `06:00:00 AM` | String  | Não
notifications.after.recurrent | Define se o lembrete é recorrente. Caso seja true, enviará o lembrete a cada `X` dias, conforme especificado em `notifications.after.days`                                         | Boolean | Não
notifications.expiration      | Objeto de lembrete na data do vencimento                                                                                                                                           | Object  | Não
notifications.expiration.send | Define se o lembrete deve ser enviado. Default: true                                                                                                                               | Boolean | Não
notifications.expiration.time | Define o horário de preferência que o lembrete deve ser enviado. O formato deve ser `HH:mm:ss` ou `HH:mm:ss A`. Exemplos: `22:00:00` ou `10:00:00 PM`. `06:00:00` ou `06:00:00 AM` | String  | Não
instructionsMsg               | Instruções de pagamento que constará no boleto. Deve conter no máximo 100 caracteres                                                                                               | String  | Não
notes                         | Observações sobre o pagamento. Não constará no boleto. Deve conter no máximo 100 caracteres                                                                                        | String  | Não

## Atualizar uma cobrança

> Requisição

```json
{
  "dueDate": "2019-12-31",
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
  "id": "c2fb02bb-e4bc-44db-a43b-bd31258a3cf3",
  "reference": null,
  "dueDate": "2019-12-31",
  "amount": 2000,
  "paidAmount": 0,
  "paidAt": null,
  "currency": "BRL",
  "items": [
    {
      "id": "4b4e7cd4-bb6a-4a25-b2f8-1e76e2494851",
      "description": "Item - 3",
      "quantity": 1,
      "price": 1000,
      "createdAt": "2019-11-06T13:22:30.000Z",
      "updatedAt": "2019-11-06T13:22:30.000Z",
      "invoiceId": "c2fb02bb-e4bc-44db-a43b-bd31258a3cf3"
    },
    {
      "id": "94637696-08dc-401d-98dd-a96ef4fa807a",
      "description": "Item - 1",
      "quantity": 1,
      "price": 1000,
      "createdAt": "2019-11-06T13:22:30.000Z",
      "updatedAt": "2019-11-06T13:22:30.000Z",
      "invoiceId": "c2fb02bb-e4bc-44db-a43b-bd31258a3cf3"
    },
    {
      "id": "ad3d7333-cdc1-4695-8199-a529b90fd8ee",
      "description": "Item - 2",
      "quantity": 1,
      "price": 1000,
      "createdAt": "2019-11-06T13:22:30.000Z",
      "updatedAt": "2019-11-06T13:22:30.000Z",
      "invoiceId": "c2fb02bb-e4bc-44db-a43b-bd31258a3cf3"
    }
  ],
  "lastMessageEvent": null,
  "lastDateMessageEvent": null,
  "itemsDiscount": {
    "amount": 1000
  },
  "earlyDiscount": {
    "percentage": 10,
    "expiryDate": "2019-12-30",
    "earlyDays": 1
  },
  "interest": {
    "percentage": 1
  },
  "fine": {
    "lateDays": 10,
    "fineStartDate": "2020-01-10",
    "percentage": 2
  },
  "customer": {
    "name": "PESSOA JURÍDICA LTDA",
    "email": "pessoajuridica@email.com",
    "phoneNumber": "5599999999999",
    "doc": {
      "type": "cnpj",
      "number": "76336239000107"
    },
    "address": {
      "cep": "03307020",
      "uf": "SP",
      "city": "São Paulo",
      "area": "Tatuapé",
      "addressLine1": "Rua Lourenço Correa",
      "streetNumber": "470"
    }
  },
  "notifications": {
    "sendOnCreate": true,
    "reminders": {
      "cc": {
        "emails": []
      },
      "before": {
        "send": true,
        "days": 3,
        "time": "06:00:00"
      },
      "after": {
        "send": true,
        "days": "3",
        "time": "06:00:00"
      },
      "expiration": {
        "send": true,
        "time": "06:00:00"
      }
    },
    "types": {
      "email": true,
      "sms": false,
      "whatsapp": false
    }
  },
  "instructionsMsg": "Teste de Instrução atualizado",
  "notes": "Teste de Observação atualizado",
  "boleto": {
    "digitableLine": "00190.00009 02625.444209 58002.630174 2 81200000002000",
    "barCodeNumber": "00192812000000020000000002625444205800263017"
  },
  "invoiceUrl": "https://sandbox.easypag.com.br/invoices/c2fb02bb-e4bc-44db-a43b-bd31258a3cf3/view/boleto",
  "payment": {
    "status": "1",
    "description": "Aguardando pagamento"
  },
  "events": [
    {
      "id": "0b893305-7ee8-42f9-ba86-a6e4b3a3f021",
      "type": "INVOICE.CREATED",
      "createdAt": "2019-11-06T13:22:30.000Z",
      "updatedAt": "2019-11-06T13:22:30.000Z",
      "invoiceId": "c2fb02bb-e4bc-44db-a43b-bd31258a3cf3"
    },
    {
      "id": "a895e595-4a02-4777-ae02-da15c3006886",
      "type": "INVOICE.UPDATED",
      "createdAt": "2019-11-06T18:56:37.000Z",
      "updatedAt": "2019-11-06T18:56:37.000Z",
      "invoiceId": "c2fb02bb-e4bc-44db-a43b-bd31258a3cf3"
    }
  ],
  "createdAt": "2019-11-06T13:22:30.000Z",
  "updatedAt": "2019-11-06T18:56:37.000Z"
}
```

Este atualiza data de vencimento de uma cobrança.

### Requisição HTTP

`PUT https://sandbox.easypag.com.br/api/v1/invoices/:id`

### Parâmetros URL

Parâmetro | Descrição        | Obrigatório
--------- | ---------------- | -----------
ID        | O ID da cobrança | Sim

### Parâmetros Body

Parâmetro                     | Descrição                                                                                                                                                                          | Tipo    | Obrigatório
----------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- | ----------------------------------------------
dueDate                       | Data de vencimento no formato ISO: YYYY-MM-DD. A data deve ser igual ou posterior a data atual                                                                                     | String  | Sim
earlyDiscount                 | Objeto de descontos por antecipação de pagamento                                                                                                                                   | Object  | Não
earlyDiscount.amount          | Valor em centavos do desconto a ser concedido sob o valor total dos itens por pagamento antecipado                                                                                 | Integer | Se não informado percentual do desconto
earlyDiscount.percentage      | Valor percentual do desconto a ser concedido sob o valor total dos itens por pagamento antecipado                                                                                  | Double  | Se não informado valor em centavos do desconto
earlyDiscount.earlyDays       | Número de dias de antecedência para aplicar o desconto por antecipação.                                                                                                            | Integer | Se informado o objeto de desconto
interest                      | Objeto de juros da cobrança                                                                                                                                                        | Object  | Não
interest.percentage           | Valor percentual do juros a ser cobrado sob o valor total dos itens por pagamento atrasado. Deve ser maior que 0 e menor ou igual a 1                                              | Double  | Se informado o objeto de juros
fine                          | Objeto de multa da cobrança                                                                                                                                                        | Object  | Não
fine.percentage               | Valor percentual da multa a ser cobrado sob o valor total dos itens por pagamento atrasado. Deve ser maior que 0 e menor ou igual a 10                                             | Double  | Se informado o objeto de multa
fine.lateDays                 | Número de dias de atraso para aplicação da multa. Deve ser maior que 0 e menor que 30                                                                                              | Integer | Se informado o objeto de multa
notifications                 | Objeto de notificações                                                                                                                                                             | Object  | Não
notifications.sendOnCreate    | Define se a cobrança é enviada para o(s) cliente(s) logo após ser criada                                                                                                           | Boolean | Não
notifications.reminders       | Objeto de lembretes de cobrança                                                                                                                                                    | Object  | Não
notifications.before          | Objeto de lembrete da data do vencimento                                                                                                                                           | Object  | Não
notifications.before.send     | Define se o lembrete deve ser enviado. Default: true                                                                                                                               | Boolean | Não
notifications.before.days     | Define a quantidade de dias antes da data de vencimento que o lembrete deve ser enviado. Default: true                                                                             | Boolean | Não
notifications.before.time     | Define o horário de preferência que o lembrete deve ser enviado. O formato deve ser `HH:mm:ss` ou `HH:mm:ss A`. Exemplos: `22:00:00` ou `10:00:00 PM`. `06:00:00` ou `06:00:00 AM` | String  | Não
notifications.after           | Objeto de lembrete após data do vencimento                                                                                                                                         | Object  | Não
notifications.after.send      | Define se o lembrete deve ser enviado. Default: true                                                                                                                               | Boolean | Não
notifications.after.days      | Define a quantidade/intervalo de dias após da data de vencimento que o lembrete deve ser enviado. Default: true                                                                    | Integer | Não
notifications.after.time      | Define o horário de preferência que o lembrete deve ser enviado. O formato deve ser `HH:mm:ss` ou `HH:mm:ss A`. Exemplos: `22:00:00` ou `10:00:00 PM`. `06:00:00` ou `06:00:00 AM` | String  | Não
notifications.after.recurrent | Define se o lembrete é recorrente. Caso seja true, enviará o lembrete a cada `X` dias, conforme especificado em `notifications.after.days`                                         | Boolean | Não
notifications.expiration      | Objeto de lembrete na data do vencimento                                                                                                                                           | Object  | Não
notifications.expiration.send | Define se o lembrete deve ser enviado. Default: true                                                                                                                               | Boolean | Não
notifications.expiration.time | Define o horário de preferência que o lembrete deve ser enviado. O formato deve ser `HH:mm:ss` ou `HH:mm:ss A`. Exemplos: `22:00:00` ou `10:00:00 PM`. `06:00:00` ou `06:00:00 AM` | String  | Não
instructionsMsg               | Instruções de pagamento que constará no boleto. Deve conter no máximo 100 caracteres                                                                                               | String  | Não
notes                         | Observações sobre o pagamento. Não constará no boleto. Deve conter no máximo 100 caracteres                                                                                        | String  | Não

## Cancelar uma cobrança

> Resultado

```http
HTTP/1.1 200 Ok
```

```json
{
  "id": "c2fb02bb-e4bc-44db-a43b-bd31258a3cf3",
  "reference": null,
  "dueDate": "2019-12-31",
  "amount": 2000,
  "paidAmount": 0,
  "paidAt": null,
  "currency": "BRL",
  "items": [
    {
      "id": "4b4e7cd4-bb6a-4a25-b2f8-1e76e2494851",
      "description": "Item - 3",
      "quantity": 1,
      "price": 1000,
      "createdAt": "2019-11-06T13:22:30.000Z",
      "updatedAt": "2019-11-06T13:22:30.000Z",
      "invoiceId": "c2fb02bb-e4bc-44db-a43b-bd31258a3cf3"
    },
    {
      "id": "94637696-08dc-401d-98dd-a96ef4fa807a",
      "description": "Item - 1",
      "quantity": 1,
      "price": 1000,
      "createdAt": "2019-11-06T13:22:30.000Z",
      "updatedAt": "2019-11-06T13:22:30.000Z",
      "invoiceId": "c2fb02bb-e4bc-44db-a43b-bd31258a3cf3"
    },
    {
      "id": "ad3d7333-cdc1-4695-8199-a529b90fd8ee",
      "description": "Item - 2",
      "quantity": 1,
      "price": 1000,
      "createdAt": "2019-11-06T13:22:30.000Z",
      "updatedAt": "2019-11-06T13:22:30.000Z",
      "invoiceId": "c2fb02bb-e4bc-44db-a43b-bd31258a3cf3"
    }
  ],
  "lastMessageEvent": null,
  "lastDateMessageEvent": null,
  "itemsDiscount": {
    "amount": 1000
  },
  "earlyDiscount": {
    "percentage": 10,
    "expiryDate": "2019-12-30",
    "earlyDays": 1
  },
  "interest": {
    "percentage": 1
  },
  "fine": {
    "lateDays": 10,
    "fineStartDate": "2020-01-10",
    "percentage": 2
  },
  "customer": {
    "name": "PESSOA JURÍDICA LTDA",
    "email": "pessoajuridica@email.com",
    "phoneNumber": "5599999999999",
    "doc": {
      "type": "cnpj",
      "number": "76336239000107"
    },
    "address": {
      "cep": "03307020",
      "uf": "SP",
      "city": "São Paulo",
      "area": "Tatuapé",
      "addressLine1": "Rua Lourenço Correa",
      "streetNumber": "470"
    }
  },
  "notifications": {
    "sendOnCreate": true,
    "reminders": {
      "cc": {
        "emails": []
      },
      "before": {
        "send": true,
        "days": 3,
        "time": "06:00:00"
      },
      "after": {
        "send": true,
        "days": "3",
        "time": "06:00:00"
      },
      "expiration": {
        "send": true,
        "time": "06:00:00"
      }
    },
    "types": {
      "email": true,
      "sms": false,
      "whatsapp": false
    }
  },
  "instructionsMsg": "Teste de Instrução atualizado",
  "notes": "Teste de Observação atualizado",
  "invoiceUrl": "https://sandbox.easypag.com.br/invoices/c2fb02bb-e4bc-44db-a43b-bd31258a3cf3/view/boleto",
  "payment": {
    "status": "5",
    "description": "Cancelado"
  },
  "events": [
    {
      "id": "0b893305-7ee8-42f9-ba86-a6e4b3a3f021",
      "type": "INVOICE.CREATED",
      "createdAt": "2019-11-06T13:22:30.000Z",
      "updatedAt": "2019-11-06T13:22:30.000Z",
      "invoiceId": "c2fb02bb-e4bc-44db-a43b-bd31258a3cf3"
    },
    {
      "id": "2c52326a-ff6d-42e9-a76f-07ee6df1b7fa",
      "type": "INVOICE.CANCELLED",
      "createdAt": "2019-11-06T19:20:08.000Z",
      "updatedAt": "2019-11-06T19:20:08.000Z",
      "invoiceId": "c2fb02bb-e4bc-44db-a43b-bd31258a3cf3"
    },
    {
      "id": "a895e595-4a02-4777-ae02-da15c3006886",
      "type": "INVOICE.UPDATED",
      "createdAt": "2019-11-06T18:56:37.000Z",
      "updatedAt": "2019-11-06T18:56:37.000Z",
      "invoiceId": "c2fb02bb-e4bc-44db-a43b-bd31258a3cf3"
    }
  ],
  "createdAt": "2019-11-06T13:22:30.000Z",
  "updatedAt": "2019-11-06T19:20:07.000Z"
}
```

Este endpoint cancela uma cobrança.

### Requisição HTTP

`POST https://sandbox.easypag.com.br/api/v1/invoices/:id/cancel`

### Parâmetros URL

Parâmetro | Descrição        | Obrigatório
--------- | ---------------- | -----------
ID        | O ID da cobrança | Sim

## Marcar uma cobrança como paga

> Resultado

```http
HTTP/1.1 200 Ok
```

```json
{
  "id": "c2fb02bb-e4bc-44db-a43b-bd31258a3cf3",
  "reference": null,
  "dueDate": "2019-12-31",
  "amount": 2000,
  "paidAmount": 0,
  "paidAt": null,
  "currency": "BRL",
  "items": [
    {
      "id": "4b4e7cd4-bb6a-4a25-b2f8-1e76e2494851",
      "description": "Item - 3",
      "quantity": 1,
      "price": 1000,
      "createdAt": "2019-11-06T13:22:30.000Z",
      "updatedAt": "2019-11-06T13:22:30.000Z",
      "invoiceId": "c2fb02bb-e4bc-44db-a43b-bd31258a3cf3"
    },
    {
      "id": "94637696-08dc-401d-98dd-a96ef4fa807a",
      "description": "Item - 1",
      "quantity": 1,
      "price": 1000,
      "createdAt": "2019-11-06T13:22:30.000Z",
      "updatedAt": "2019-11-06T13:22:30.000Z",
      "invoiceId": "c2fb02bb-e4bc-44db-a43b-bd31258a3cf3"
    },
    {
      "id": "ad3d7333-cdc1-4695-8199-a529b90fd8ee",
      "description": "Item - 2",
      "quantity": 1,
      "price": 1000,
      "createdAt": "2019-11-06T13:22:30.000Z",
      "updatedAt": "2019-11-06T13:22:30.000Z",
      "invoiceId": "c2fb02bb-e4bc-44db-a43b-bd31258a3cf3"
    }
  ],
  "lastMessageEvent": null,
  "lastDateMessageEvent": null,
  "itemsDiscount": {
    "amount": 1000
  },
  "earlyDiscount": {
    "percentage": 10,
    "expiryDate": "2019-12-30",
    "earlyDays": 1
  },
  "interest": {
    "percentage": 1
  },
  "fine": {
    "lateDays": 10,
    "fineStartDate": "2020-01-10",
    "percentage": 2
  },
  "customer": {
    "name": "PESSOA JURÍDICA LTDA",
    "email": "pessoajuridica@email.com",
    "phoneNumber": "5599999999999",
    "doc": {
      "type": "cnpj",
      "number": "76336239000107"
    },
    "address": {
      "cep": "03307020",
      "uf": "SP",
      "city": "São Paulo",
      "area": "Tatuapé",
      "addressLine1": "Rua Lourenço Correa",
      "streetNumber": "470"
    }
  },
  "notifications": {
    "sendOnCreate": true,
    "reminders": {
      "cc": {
        "emails": []
      },
      "before": {
        "send": true,
        "days": 3,
        "time": "06:00:00"
      },
      "after": {
        "send": true,
        "days": "3",
        "time": "06:00:00"
      },
      "expiration": {
        "send": true,
        "time": "06:00:00"
      }
    },
    "types": {
      "email": true,
      "sms": false,
      "whatsapp": false
    }
  },
  "instructionsMsg": "Teste de Instrução atualizado",
  "notes": "Teste de Observação atualizado",
  "invoiceUrl": "https://sandbox.easypag.com.br/invoices/c2fb02bb-e4bc-44db-a43b-bd31258a3cf3/view/boleto",
  "payment": {
    "status": "3",
    "description": "Cobrança manualmente marcada como paga"
  },
  "events": [
    {
      "id": "0b893305-7ee8-42f9-ba86-a6e4b3a3f021",
      "type": "INVOICE.CREATED",
      "createdAt": "2019-11-06T13:22:30.000Z",
      "updatedAt": "2019-11-06T13:22:30.000Z",
      "invoiceId": "c2fb02bb-e4bc-44db-a43b-bd31258a3cf3"
    },
    {
      "id": "2c52326a-ff6d-42e9-a76f-07ee6df1b7fa",
      "type": "INVOICE.CANCELLED",
      "createdAt": "2019-11-06T19:20:08.000Z",
      "updatedAt": "2019-11-06T19:20:08.000Z",
      "invoiceId": "c2fb02bb-e4bc-44db-a43b-bd31258a3cf3"
    },
    {
      "id": "a895e595-4a02-4777-ae02-da15c3006886",
      "type": "INVOICE.UPDATED",
      "createdAt": "2019-11-06T18:56:37.000Z",
      "updatedAt": "2019-11-06T18:56:37.000Z",
      "invoiceId": "c2fb02bb-e4bc-44db-a43b-bd31258a3cf3"
    }
  ],
  "createdAt": "2019-11-06T13:22:30.000Z",
  "updatedAt": "2019-11-06T19:20:07.000Z"
}
```

Este endpoint marca uma cobrança como paga.

### Requisição HTTP

`POST https://sandbox.easypag.com.br/api/v1/invoices/:id/settle`

### Parâmetros URL

Parâmetro | Descrição        | Obrigatório
--------- | ---------------- | -----------
ID        | O ID da cobrança | Sim

## Visualizar PDF do boleto

> Resultado

```http
HTTP/1.1 200 Ok
```

Este endpoint renderiza o arquivo pdf do boleto de uma cobrança.

### Requisição HTTP

`GET https://sandbox.easypag.com.br/api/v1/invoices/:id/view/boleto`

### Parâmetros URL

Parâmetro | Descrição                         | Obrigatório
--------- | --------------------------------- | -----------
ID        | O ID da cobrança a ser consultado | Sim

## Reenviar uma cobrança

> Resposta

```http
HTTP/1.1 200 Ok
```

Este endpoint reenvia o email de uma cobrança.

### Requisição HTTP

`POST https://sandbox.easypag.com.br/api/v1/invoices/:id/resend`

### Parâmetros URL

Parâmetro | Descrição        | Obrigatório
--------- | ---------------- | -----------
ID        | O ID da cobrança | Sim

# Cobranças (Carnês)

## Status de Carnê

O parâmetro `payment.status`, retornado no objeto de resposta, contém o status do carnê.

Cód. Status | Status    | Descrição
----------- | --------- | -----------------------------------------------------------------------------------------------------------------------
1           | Ativo     | Status inicial quando o carnê é criado.
2           | Cancelado | Os carnês que foram manualmente cancelados têm as suas cobranças canceladas e os seus boletos automaticamente baixados.

## Listar todos os carnês

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
      "id": "cfb4f19d-a204-4934-9da6-04d35879d6a0",
      "repeat": 12,
      "customer": {
        "name": "PESSOA JURÍDICA LTDA",
        "email": "exemplo@email.com",
        "phoneNumber": "559999999999",
        "doc": {
          "type": "cnpj",
          "number": "20238189000162"
        },
        "address": {
          "cep": "13064110",
          "uf": "SP",
          "city": "Campinas",
          "area": "Parque Santa Bárbara",
          "addressLine1": "Rua Armando Rizzoni",
          "addressLine2": null,
          "streetNumber": "9999"
        }
      },
      "createdAt": "2019-10-31T13:14:10.000Z",
      "updatedAt": "2019-10-31T13:14:10.000Z"
    }
  ]
}
```

### Parâmetros Query

Parâmetro     | Descrição                                                                                                                                                                                                                                                    | Obrigatório
------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -----------
page          | Número da página a ser retornada                                                                                                                                                                                                                             | Não
limit         | Número itens a ser retornado por página. Valor mínimo: 1, valor máximo: 100\. Caso não seja infromado, será aplicado o valor default de 100 itens                                                                                                            | Não
sortBy        | Um hash contendo na chave, o nome do campo para ordenação e o valor DESC ou ASC para descendente e ascendente, respectivamente. Exemplos: sortBy[dueDate]=DESC, sortBy[amount]=DESC, sortBy[customerName]=ASC, sortBy[createdAt]=ASC, sortBy[updatedAt]=DESC | Não
createdAtFrom | Filtra registros criados a partir da data passada no parâmetro. Formato (AAAA-MM-DDThh:mm:ss)                                                                                                                                                                | Não
createdAtTo   | Filtra registros criados até esta data passada no parâmetro. Formato (AAAA-MM-DDThh:mm:ss)                                                                                                                                                                   | Não
updatedSince  | Filtra registros atualizados desde o valor passado no parâmetro. Formato (AAAA-MM-DDThh:mm:ss)                                                                                                                                                               | Não

Este endpoint lista todos carnês de cobranças.

### Requisição HTTP

`GET https://sandbox.easypag.com.br/api/v1/recurrences`

## Listar um carnê

> Resposta

```http
HTTP/1.1 200 Ok
```

```json
{
  "id": "cfb4f19d-a204-4934-9da6-04d35879d6a0",
  "repeat": 12,
  "customer": {
    "name": "PESSOA JURÍDICA LTDA",
    "email": "exemplo@email.com",
    "phoneNumber": "559999999999",
    "doc": {
      "type": "cnpj",
      "number": "20238189000162"
    },
    "address": {
      "cep": "13064110",
      "uf": "SP",
      "city": "Campinas",
      "area": "Parque Santa Bárbara",
      "addressLine1": "Rua Armando Rizzoni",
      "addressLine2": null,
      "streetNumber": "9999"
    }
  },
  "createdAt": "2019-10-31T13:14:10.000Z",
  "updatedAt": "2019-10-31T13:14:10.000Z"
}
```

Este endpoint lista um carnê.

### Requisição HTTP

`GET https://sandbox.easypag.com.br/api/v1/recurrences/:id`

### Parâmetros URL

Parâmetro | Descrição     | Obrigatório
--------- | ------------- | -----------
ID        | O ID do carnê | Sim

## Criar um carnê

> Requisição

```json
{
  "dueDate": "2019-10-30",
  "interest": {
    "percentage": 1.0
  },
  "fine": {
    "percentage": 5.0,
    "lateDays": 7
  },
  "items": [
    {
      "description": "Item de Teste",
      "quantity": 1,
      "price": 1000
    }
  ],
  // Recorrência: 2 a 12 parcelas por carnê.
  // As parcelas geradas tem as datas de vencimento, multa e descontos
  // incrementados em 30 dias, sucessivamente.
  "recurrence": {
    "repeat": 12
  },
  // Pode-se utilizar o ID do cliente ou os dados avulsos.
  "customer": {
    "id": "03f7f73f-617d-4d4d-845a-7d464a7d8011"
  }
}
```

> Resposta

```http
HTTP/1.1 201 Created
```

```json
{
  "id": "1060d867-c529-4852-b183-4fd474ecd812",
  "reference": null,
  "dueDate": "2019-10-30",
  "amount": 1000,
  "paidAmount": 0,
  "paidAt": null,
  "currency": "BRL",
  "items": [
    {
      "id": "720ceeec-148b-46a0-8abf-21d5b2de4681",
      "description": "Item de Teste",
      "quantity": 1,
      "price": 1000,
      "createdAt": "2019-10-22T17:33:35.000Z",
      "updatedAt": "2019-10-22T17:33:35.000Z",
      "invoiceId": "1060d867-c529-4852-b183-4fd474ecd812"
    }
  ],
  "lastMessageEvent": null,
  "lastDateMessageEvent": null,
  "interest": {
    "percentage": 1
  },
  "fine": {
    "lateDays": 7,
    "fineStartDate": "2019-11-06",
    "percentage": 6
  },
  "customer": {
    "name": "João da Silva",
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
    }
  },
  "notifications": {
    "sendOnCreate": true,
    "reminders": {
      "cc": {
        "emails": []
      },
      "before": {
        "send": true,
        "days": 3,
        "time": "06:00:00"
      },
      "after": {
        "send": true,
        "days": "3",
        "time": "06:00:00"
      },
      "expiration": {
        "send": true,
        "time": "06:00:00"
      }
    },
    "types": {
      "email": true,
      "sms": false,
      "whatsapp": false
    }
  },
  "recurrence": {
    // ID da Recorrência.
    // Pode ser utilizada para consultar o carnê e todas as cobranças geradas.
    "id": "49143944-38f8-4112-97ac-0c68602214d6",
    "createdAt": "2019-10-22T17:33:35.000Z",
    "updatedAt": "2019-10-22T17:33:35.000Z"
  },
  "invoiceUrl": "https://sandbox.easypag.com.br",
  "payment": {
    "status": "1",
    "description": "Aguardando pagamento"
  },
  "events": [
    {
      "id": "8afd9586-1035-4e1f-bfd5-a56d070c2482",
      "type": "INVOICE.CREATED",
      "createdAt": "2019-10-22T17:33:35.000Z",
      "updatedAt": "2019-10-22T17:33:35.000Z",
      "invoiceId": "1060d867-c529-4852-b183-4fd474ecd812"
    }
  ],
  "createdAt": "2019-10-22T17:33:35.000Z",
  "updatedAt": "2019-10-22T17:33:35.000Z"
}
```

Este endpoint cria um carnê.

### Requisição HTTP

`POST https://sandbox.easypag.com.br/api/v1/invoices`

<aside class="notice">
Você pode utilizar os mesmos dados e opções de criar cobranças, adicionando apenas os dados abaixo para indicar que se trata de um carnê. Você receberá o id da recorrência na resposta, que poderá utilizar para consultar todas as cobranças geradas pelo carnê.
</aside>

### Parâmetros Body

Parâmetro         | Descrição                                                                                    | Tipo    | Obrigatório
----------------- | -------------------------------------------------------------------------------------------- | ------- | ----------------
recurrence        | Objeto de recorrência                                                                        | Object  | Sim (Para carnê)
recurrence.repeat | Número de parcelas a serem geradas com base no primeiro vencimento. Valores aceitos: 2 a 12. | Integer | Sim (Para carnê)

## Listar todas as cobranças de um carnê

Este endpoint lista todas as cobranças de um carnê.

`GET https://sandbox.easypag.com.br/api/v1/recurrences/:id/invoices`

### Parâmetros URL

Parâmetro | Descrição     | Obrigatório
--------- | ------------- | -----------
ID        | O ID do carnê | Sim

> Resposta

```http
HTTP/1.1 200 Ok
```

```json
[
  {
    "id": "819bb3e4-3421-4208-9809-2a3c475a98b6",
    "reference": null,
    "dueDate": "2019-10-31",
    "amount": 1000,
    "paidAmount": 0,
    "paidAt": null,
    "currency": "BRL",
    "items": [
      {
        "id": "b7788ccb-9c01-4125-a57e-036c67d4b058",
        "description": "ITEM DE TESTE",
        "quantity": 1,
        "price": 1000,
        "createdAt": "2019-10-31T13:14:10.000Z",
        "updatedAt": "2019-10-31T13:14:10.000Z",
        "invoiceId": "819bb3e4-3421-4208-9809-2a3c475a98b6"
      }
    ],
    "lastMessageEvent": null,
    "lastDateMessageEvent": null,
    "invoiceMessageEvent": [],
    "customer": {
      "name": "PESSOA JURÍDICA LTDA",
      "email": "exemplo@email.com",
      "phoneNumber": "559999999999",
      "doc": {
        "type": "cnpj",
        "number": "20238189000162"
      },
      "address": {
        "cep": "13064110",
        "uf": "SP",
        "city": "Campinas",
        "area": "Parque Santa Bárbara",
        "addressLine1": "Rua Armando Rizzoni",
        "streetNumber": "9999"
      }
    },
    "notifications": {
      "sendOnCreate": true,
      "reminders": {
        "cc": {
          "emails": []
        },
        "before": {
          "send": true,
          "days": 3,
          "time": "06:00:00"
        },
        "after": {
          "send": true,
          "days": "3",
          "time": "06:00:00"
        },
        "expiration": {
          "send": true,
          "time": "06:00:00"
        }
      },
      "types": {
        "email": true,
        "sms": false,
        "whatsapp": false
      }
    },
    "recurrence": {
      "id": "cfb4f19d-a204-4934-9da6-04d35879d6a0",
      "createdAt": "2019-10-31T13:14:10.000Z",
      "updatedAt": "2019-10-31T13:14:10.000Z"
    },
    "invoiceUrl": "https://sandbox.easypag.com.br",
    "payment": {
      "status": "1",
      "description": "Aguardando pagamento"
    },
    "events": [
      {
        "id": "4fcd7c08-44e3-4536-80fa-b32aa0678cb2",
        "type": "INVOICE.CREATED",
        "createdAt": "2019-10-31T13:14:10.000Z",
        "updatedAt": "2019-10-31T13:14:10.000Z",
        "invoiceId": "819bb3e4-3421-4208-9809-2a3c475a98b6"
      }
    ],
    "createdAt": "2019-10-31T13:14:10.000Z",
    "updatedAt": "2019-10-31T13:14:10.000Z"
  }
]
```

## Cancelar um carnê

> Resultado

```http
HTTP/1.1 200 Ok
```

```json
{
  "id": "cfb4f19d-a204-4934-9da6-04d35879d6a0",
  "repeat": 12,
  "customer": {
    "name": "PESSOA JURÍDICA LTDA",
    "email": "exemplo@email.com",
    "phoneNumber": "559999999999",
    "doc": {
      "type": "cnpj",
      "number": "20238189000162"
    },
    "address": {
      "cep": "13064110",
      "uf": "SP",
      "city": "Campinas",
      "area": "Parque Santa Bárbara",
      "addressLine1": "Rua Armando Rizzoni",
      "addressLine2": null,
      "streetNumber": "9999"
    }
  },
  "createdAt": "2019-10-31T13:14:10.000Z",
  "updatedAt": "2019-10-31T14:12:33.000Z"
}
```

Este endpoint cancela um carnê e suas cobranças.

### Requisição HTTP

`POST https://sandbox.easypag.com.br/api/v1/recurrences/:id/cancel`

### Parâmetros URL

Parâmetro | Descrição     | Obrigatório
--------- | ------------- | -----------
ID        | O ID do carnê | Sim

## Visualizar carnê

> Resultado

```http
HTTP/1.1 200 Ok
```

Este endpoint renderiza o arquivo pdf do carnê, contendo todos os boletos.

### Requisição HTTP

`GET https://sandbox.easypag.com.br/api/v1/recurrences/:id/view/carne`

### Parâmetros URL

Parâmetro | Descrição                      | Obrigatório
--------- | ------------------------------ | -----------
ID        | O ID do carnê a ser consultado | Sim

# Notificações Webhook

## Introdução

Os eventos são disparados via `POST` na url especificada na criação do webhook. Somente urls que utilizem `https` serão aceitas.

## Eventos disponíveis

Evento            | Descrição
----------------- | ---------------------------------------------------------------------------------------------------------------------------
INVOICE.*         | Notifica em caso de todos os eventos das cobranças.
INVOICE.CREATED   | Notifica caso uma cobrança seja criada.
INVOICE.PAID      | Notifica caso o status de pagamento de uma cobrança seja alterado para `2`, que equivale a cobrança paga.
INVOICE.SETTLED   | Notifica caso o status de pagamento de uma cobrança seja alterado para `3`, que equivale a cobrança marcada como paga.      |
INVOICE.CANCELLED | Notifica caso o status de pagamento de uma cobrança seja alterado para `5`, que equivale a cobrança cancelada pelo usuário.
INVOICE.EXPIRED   | Notifica caso o status de pagamento de uma cobrança seja alterado para `6`, que equivale a cobrança expirada.

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
        "description": "Item-2",
        "quantity": 1,
        "price": 1000,
        "createdAt": "2019-06-17T18:06:07.000Z",
        "updatedAt": "2019-06-17T18:06:07.000Z",
        "invoiceId": "24852ff8-a068-4c65-96e8-936163754768"
      },
      {
        "id": "edc403dc-c62e-428c-a631-b72d90d93904",
        "description": "Item-1",
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

`GET https://sandbox.easypag.com.br/api/v1/webhooks`

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

`GET https://sandbox.easypag.com.br/api/v1/webhooks/:id`

### Parâmetros URL

Parâmetro | Descrição       | Obrigatório
--------- | --------------- | -----------
ID        | O ID do webhook | Sim

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

`POST https://sandbox.easypag.com.br/api/v1/webhooks`

### Parâmetros Body

Parâmetro | Descrição                                                                                                                                                | Tipo   | Obrigatório
--------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ | -----------
targetUrl | Url de notficação. Deve ser `https`.                                                                                                                     | String | Sim
event     | Evento de notficação. Deve ser um dos valores: `INVOICE.*`, `INVOICE.CREATED`, `INVOICE.PAID`, `INVOICE.SETTLED`, `INVOICE.CANCELLED`, `INVOICE.EXPIRED` | String | Sim
authKey   | Chave de autenticação que será enviada no header da notificação                                                                                          | String | Sim

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

`PUT https://sandbox.easypag.com.br/api/v1/webhooks/:id`

### Parâmetros URL

Parâmetro | Descrição       | Obrigatório
--------- | --------------- | -----------
ID        | O ID do webhook | Sim

<aside class="notice">
Você pode informar apenas os parâmetros que deseja atualizar.
</aside>

### Parâmetros Body

Parâmetro | Descrição                                                                                                                                                                                            | Tipo   | Obrigatório
--------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ | -----------
targetUrl | Url de notficação. Deve ser `https`.                                                                                                                                                                 | String | Não
event     | Evento de notficação. Deve ser um dos valores: `INVOICE.*`, `INVOICE.CREATED`, `INVOICE.STATUS_UPDATE`, `INVOICE.PAID`, `INVOICE.SETTLED`, `INVOICE.OVERDUE`, `INVOICE.CANCELLED`, `INVOICE.EXPIRED` | String | Não
authKey   | Chave de autenticação que será enviada no header da notificação                                                                                                                                      | String | Não

## Deletar um webhook

> Resposta

```http
HTTP/1.1 204 No Content
```

Este endpoint deleta um webhook especificado.

### Requisição HTTP

`DELETE https://sandbox.easypag.com.br/api/v1/webhooks/:id`

### Parâmetros URL

Parâmetro | Descrição       | Obrigatório
--------- | --------------- | -----------
ID        | O ID do webhook | Sim

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

`GET https://sandbox.easypag.com.br/api/v1/banks?search=banco do brasil`

### Parâmetros Query

Parâmetro | Descrição                                                                 | Obrigatório
--------- | ------------------------------------------------------------------------- | -----------
search    | Palavra-chave para efetuar busca. Se não informado retorna todos os itens | Não

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

`GET https://sandbox.easypag.com.br/api/v1/bank-accounts`

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

`GET https://sandbox.easypag.com.br/api/v1/bank-accounts/:id`

### Parâmetros URL

Parâmetro | Descrição              | Obrigatório
--------- | ---------------------- | -----------
ID        | O ID da conta bancária | Sim

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

`POST https://sandbox.easypag.com.br/api/v1/bank-accounts`

### Parâmetros Body

Parâmetro             | Descrição                                                                                       | Tipo   | Obrigatório
--------------------- | ----------------------------------------------------------------------------------------------- | ------ | -----------
beneficiary           | Objeto do titular da conta                                                                      | Object | Sim
beneficiary.name      | Nome completo do titular da conta                                                               | Object | Sim
beneficiary.docNumber | CPF/CNPJ do titular da conta                                                                    | Object | Sim
agency                | Objeto da agência da conta                                                                      | Object | Sim
agency.number         | Número agência da conta. Máximo de 5 dígitos                                                    | String | Sim
agency.digit          | Dígito agência da conta . Máximo de 2 digitos                                                   | String | Não
account               | Objeto da conta                                                                                 | Object | Sim
account.number        | Número da conta. Máximo de 12 dígitos                                                           | String | Sim
account.digit         | Dígito conta. Máximo de 2 digitos                                                               | String | Sim
account.type          | Tipo de conta bancária. Informar "checking" para conta corrente ou "saving" para conta poupança | String | Sim

## Deletar uma conta bancária

> Resposta

```http
HTTP/1.1 204 No Content
```

Este endpoint exclui uma conta bancária do usuário.

### Requisição HTTP

`DELETE https://sandbox.easypag.com.br/api/v1/bank-accounts/:id`

### Parâmetros URL

Parâmetro | Descrição              | Obrigatório
--------- | ---------------------- | -----------
ID        | O ID da conta bancária | Sim

# Resgates

## Status de Resgate

O parâmetro `status.code`, retornado no objeto de resposta, contém o status do resgate.

Status | Descrição
------ | ----------------------------------------------------------------------------------------------------
1      | Pedido de resgate efetuado
2      | Transferência bancária efetuada. Aguardando confirmação do banco de destino
3      | Pedido de resgate recusado. Por favor entre em contato com nosso suporte
4      | Pedido de resgate concluído com sucesso. O banco de destino confirmou o recebimento da transferência
5      | Pedido de resgate falhou. A transferência foi devolvida pelo banco de destino

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

`GET https://sandbox.easypag.com.br/api/v1/redemptions`

### Parâmetros Query

Parâmetro     | Descrição                                                                                                                                                                                                                           | Obrigatório
------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -----------
page          | Número da página a ser retornada                                                                                                                                                                                                    | Não
limit         | Número itens a ser retornado por página. Valor mínimo: 1, valor máximo: 100\. Caso não seja infromado, será aplicado o valor default de 100 itens                                                                                   | Não
search        | Parâmetro que recebe texto e efetua pesquisa nos campos de nome do banco, nome do titular da conta, número da agência da conta, número da conta                                                                                     | Não
docNumber     | Parâmetro que filtra resgates a partir de cpf/cnpj do beneficiário. Pode ser informado com ou sem máscara                                                                                                                           | Não
status        | Parâmetro que filtra resgates por um ou mais status separados por vírgula                                                                                                                                                           | Não
sortBy        | Um hash contendo na chave, o nome do campo para ordenação e o valor DESC ou ASC para descendente e ascendente, respectivamente. Exemplos: sortBy[amount]=ASC, sortBy[approvedAt]=ASC, sortBy[createdAt]=ASC, sortBy[updatedAt]=DESC | Não
createdAtFrom | Filtra registros criados a partir da data passada no parâmetro. Formato (AAAA-MM-DDThh:mm:ss)                                                                                                                                       | Não
createdAtTo   | Filtra registros criados até esta data passada no parâmetro. Formato (AAAA-MM-DDThh:mm:ss)                                                                                                                                          | Não
updatedSince  | Filtra registros atualizados desde o valor passado no parâmetro. Formato (AAAA-MM-DDThh:mm:ss)                                                                                                                                      | Não

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

`GET https://sandbox.easypag.com.br/api/v1/bank-accounts/:id`

### Parâmetros URL

Parâmetro | Descrição       | Obrigatório
--------- | --------------- | -----------
ID        | O ID do resgate | Sim

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

`POST https://sandbox.easypag.com.br/api/v1/redemptions`

### Parâmetros Body

Parâmetro      | Descrição                                                                                         | Tipo    | Obrigatório
-------------- | ------------------------------------------------------------------------------------------------- | ------- | -----------
amount         | Valor em centavos a ser solicitado. Deve ser menor ou igual ao valor disponível do saldo da conta | Integer | Sim
bankAccount    | Objeto da conta bancária                                                                          | Object  | Sim
bankAccount.id | Id conta bancária cadastrada, para qual será efetuada a transferência do valor solicitado         | String  | Sim

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

`GET https://sandbox.easypag.com.br/api/v1/balances`

# Extratos

## Tipos de Transação

O parâmetro `type`, retornado no objeto de resposta, contém o tipo de transação no extrato.

Tipo | Descrição
---- | ----------------------------------------
1    | Crédito de cobrança paga
2    | Valor de resgate solicitado
3    | Extorno de resgate solicitado
4    | Débito de taxa de recebimento por boleto

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

`GET https://sandbox.easypag.com.br/api/v1/statements`

### Parâmetros Query

Parâmetro     | Descrição                                                                                                                                                                            | Obrigatório
------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -----------
page          | Número da página a ser retornada                                                                                                                                                     | Não
limit         | Número itens a ser retornado por página. Valor mínimo: 1, valor máximo: 100\. Caso não seja infromado, será aplicado o valor default de 100 itens                                    | Não
type          | Filtra registros que possuam o tipo de transação informada. Formato (Código do tipo de transação. Caso seja mais de um tipo, separar por vírgula.) Exemplo: type=1,2,3 ou type=1     | Não
sortBy        | Um hash contendo na chave, o nome do campo para ordenação e o valor DESC ou ASC para descendente e ascendente, respectivamente. Exemplos: sortBy[amount]=DESC, sortBy[createdAt]=ASC | Não
createdAtFrom | Filtra registros criados a partir da data passada no parâmetro. Formato (AAAA-MM-DDThh:mm:ss)                                                                                        | Não
createdAtTo   | Filtra registros criados até esta data passada no parâmetro. Formato (AAAA-MM-DDThh:mm:ss)                                                                                           | Não

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

`GET https://sandbox.easypag.com.br/api/v1/reports/invoices/balances?startDate=2019-04-02&endDate=2019-04-30`

### Parâmetros Query

Parâmetro | Descrição                                                                                          | Obrigatório
--------- | -------------------------------------------------------------------------------------------------- | -----------
startDate | Data inicial de referência em formato ISO: YYYY-MM-DD                                              | Não
endDate   | Data final de referência em formato ISO: YYYY-MM-DD                                                | Não
docNumber | CPF/CNPJ do cliente. Pode ser utilizado para exibir os saldos de cobranças referentes a um cliente | Não
