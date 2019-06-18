# Erros

## Autenticação

| Código do Erro | Código HTTP        | Descrição                          |
| -------------- | ------------------ | ---------------------------------- |
| 4100           | 401 - Unauthorized | Credenciais inválidas ou ausentes. |

## Cobrança

| Código do Erro | Código HTTP                | Descrição                                                                                    |
| -------------- | -------------------------- | -------------------------------------------------------------------------------------------- |
| 4400           | 422 - Unprocessable Entity | O valor total dos itens deve ser maior ou igual a 5,00 BRL.                                  |
| 4401           | 422 - Unprocessable Entity | O valor total dos itens com os descontos aplicados deve ser maior ou igual a 5,00 BRL.       |
| 4402           | 422 - Unprocessable Entity | O valor total dos itens com os descontos por antecipação deve ser maior ou igual a 5,00 BRL. |
| 4403           | 422 - Unprocessable Entity | Status do Pagamento inválido para cancelamento.                                              |
| 4404           | 422 - Unprocessable Entity | ID do Cliente inválido ou inexistente.                                                       |
| 4405           | 422 - Unprocessable Entity | Status do Pagamento inválido para alteração do vencimento.                                   |
| 4406           | 422 - Unprocessable Entity | Status do Pagamento inválido para marcar a fatura como paga.                                 |
|                |

## Resgate

| Código do Erro | Código HTTP                | Descrição                                     |
| -------------- | -------------------------- | --------------------------------------------- |
| 4600           | 422 - Unprocessable Entity | ID da conta bancária inválido ou inexistente. |
| 4601           | 422 - Unprocessable Entity | Saldo insuficiente para solicitar resgate.    |

## Requisição

| Código do Erro | Código HTTP                | Descrição                                                          |
| -------------- | -------------------------- | ------------------------------------------------------------------ |
| 4000           | 400 - Bad Request          | Parâmetro(s) inválido(s) ou ausente(s) na requisição.              |
| 4001           | 400 - Bad Request          | Body da requisição é inválido.                                     |
| 4002           | 422 - Unprocessable Entity | Dado(s) duplicado(s). Certifique-se de que inserir valores únicos. |
| 4003           | 429 - Too Many Requests    | Você atingiu o limite máximo de requisições.                       |

## Servidor

| Código do Erro | Código HTTP                 | Descrição                                                                                       |
| -------------- | --------------------------- | ----------------------------------------------------------------------------------------------- |
| 5000           | 500 - Internal Server Error | Erro interno no servidor. Por favor, tente novamente ou entre em contato com o suporte técnico. |
