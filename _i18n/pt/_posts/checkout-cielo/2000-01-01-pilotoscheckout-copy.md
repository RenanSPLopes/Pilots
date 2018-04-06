---
layout: manual
title: Pilotos Checkout Cielo
description: Integração técnica via API
search: true
translated: true
toc_footers: true
categories: manual
sort_order: 1
tags:
  - Checkout Cielo
language_tabs:
  json: JSON
---

# Cielo OAUTH

O Cielo OUTH é um processo de autenticação utilizado em APIs Cielo que são correlacionadas a produtos E-commerce. Ele utiliza o **Protocolo Oauth** como base de autenticação de requisições

Para utilizar o Cielo Oauth

| PROPRIEDADE    | DESCRIÇÃO                                                             | TIPO   |
|----------------|-----------------------------------------------------------------------|--------|
| `ClientId`     | Identificador chave fornecido pela CIELO                              | guid   |
| `ClientSecret` | Chave que valida o ClientID. Fornecida pela Cielo junto ao `ClientID` | string |

> Para obter o `ClientID` e o `ClientSecret`, acione a equipe de Produtos Cielo. Credênciais liberadas apenas para lojistas selecionados,

## Token de acesso

Para obter acesso a serviços Cielo que utilizam o `Cielo Oauth`, será necessário obter um token de acesso, conforme os passos abaixo:

1. Concatenar o _ClientId_ e o _ClientSecret_, **ClientId:ClientSecret**
2. Codificar o resultado em **Base64**
3. Enviar uma requisição, utilizando o método HTTP POST para o Endpoint abaixo

<aside class="request"><span class="method post">POST</span> <span class="endpoint">https://cieloecommerce.cielo.com.br/api/public/v2/token/</span></aside>

### Concatenação

|**ClientId** | b521b6b2-b9b4-4a30-881d-3b63dece0006|
|**ClientSecret**| 08Qkje79NwWRx5BdgNJsIkBuITt5cIVO
|**ClientId:ClientSecret**| *b521b6b2-b9b4-4a30-881d-3b63dece0006:08Qkje79NwWRx5BdgNJsIkBuITt5cIVO*|
|**Base64**| *YjUyMWI2YjItYjliNC00YTMwLTg4MWQtM2I2M2RlY2UwMDA2OiAwOFFramU3OU53V1J4NUJkZ05Kc0lrQnVJVHQ1Y0lWTw*|

### Request

``` json
"POST": https://cieloecommerce.cielo.com.br/v2/public/v2/token
"Headers"
"Authorization": Basic YjUyMWI2YjItYjliNC00YTMwLTg4MWQtM2I2M2RlY2UwMDA2OiAwOFFramU3OU53V1J4NUJkZ05Kc0lrQnVJVHQ1Y0lWTw
```

### Response


``` json
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJjbGllbnRfbmFtZSI6Ik1ldUNoZWNrb3V0IE1hc3RlciBLZXkiLCJjbGllbnRfaWQiOiJjODlmZGasdasdasdmUyLTRlNzctODA2YS02ZDc1Y2QzOTdkYWMiLCJzY29wZXMiOiJ7XCJTY29wZVwiOlwiQ2hlY2tvdXRBcGlcIixcIkNsYWltc1wiOltdfSIsInJvbGUiOiJasdasdasd291dEFwaSIsImlzc47I6Imh0dHBzOi8vYXV0aGhvbasdasdnJhc3BhZy5jb20uYnIiLCJhdWQiOiJVVlF4Y1VBMmNTSjFma1EzSVVFbk9pSTNkbTl0ZmasdsadQjVKVVV1UVdnPSIsImV4cCI6MTQ5Nzk5NjY3NywibmJmIjoxNDk3OTEwMjc3fQ.ozj4xnH9PA3dji-ARPSbI7Nakn9dw5I8w6myBRkF-uA",
    "token_type": "bearer",
    "expires_in": 1199
}
```

|PROPRIEDADE   | DESCRIÇÃO                                                 |TIPO  |
|--------------|-----------------------------------------------------------|------|
|`Access_token`| Utilizado para acesso aos serviços da API                 |string|
|`Token_type`  | Sempre será do tipo `bearer`                              |texto |
|`Expires_in`  | Validade do token em segundos. Aproximadamente 20 minutos |int   |

> O token possui um tempo de expiração aproximado de 20 minutos (1.200 segundos). Após esse intervalo, será necessário obter um novo token para acesso aos serviços Cielo. 


# API - CONTROLE TRANSACIONAL 

A **API de Controle Transacional** permite ao lojista realizar operações sobre os pedidos que antes eram possíveis somente através do Backoffice do Checkout Cielo. 

As operações possíveis de serem realizadas são: 
* **Consulta** – consultar uma transação
* **Captura** – capturar uma transação com valor total
* **Cancelamento** – cancelar uma transação com valor total

Seu principal objetivo é permitir que lojas e plataformas possam automatizar as operações através de seus próprios sistemas. 

> Endereço: https://cieloecommerce.cielo.com.br/api/public/v2/orders/  

## Pré-requisitos para utilização

Para realizar o controle transacional no Checkout Cielo é OBRIGATÓRIO que a loja possua um dos dois modelos de notificação abaixo configurado:

* URL de Notificação via **POST**
* URL de Notificação via **JSON**

A notificação é obrigatorio pois todos os comandos da API (Consulta / Captura / Cancelamento) usam o identificado unico da transação, chamado de `Checkout_Cielo_Order_Number`.

O `Checkout_Cielo_Order_Number` é um identificado único gerado apenas quando o pagamento é *finalizado na tela transacional*. Ele é enviado apenas pela *URL de Notificação* e não pelo Response da criação da tela transacional. 

### Autorização de acesso

A API de `Controle transacional` utiliza como forma de segurança o **protocolo OAUTH**.
Para acessá-la será necessário possuir: 

| PROPRIEDADE    | DESCRIÇÃO                                                      | TIPO   |
|----------------|----------------------------------------------------------------|--------|
| `ClientId`     | Mesmo que o **MerchantId**                                     | guid   |
| `ClientSecret` | Chave secreta que deverá ser obtida dentro do backoffice Cielo | string |

Para obter as credênciais de acesso a API, basta seguir o fluxo abaixo:

1 - Acesso o Backoffice **Checkout Cielo**
2 - Na aba **"Configurações"**, acesse a opção **"Dados cadastrais"** clique em **"Gerar Credenciais de acesso às APIs"** como na imagem abaixo

![backoffice](http://moss-beaver.cloudvent.net/images/gerarcredenciais.png)

3 - A mensagem abaixo será enviada para o **e-mail de contato tecnico**, contendo as credenciais de acesso:

![e-mail](http://moss-beaver.cloudvent.net/images/emailcredencial.png)

> Caso o botão de **"Gerar Credenciais de acesso às APIs"** não esteja disponivel em seu backoffice, acione a equipe de Produtos Cielo para a liberação da funcionalidade

Para obter acesso aos serviços da API de controle transacional, será necessário obter um token de acesso, conforme os passos abaixo:

1. Concatenar o _ClientId_ e o _ClientSecret_, **ClientId:ClientSecret**
2. Codificar o resultado em *Base64*
3. Enviar uma requisição, utilizando o método HTTP POST, para obter o token de acesso conforme abaixo:

|**POST**         |https://cieloecommerce.cielo.com.br/api/public/v2/token|
|**Authorization**|Basic {Base64}                                         |

**EXEMPLO**

* **ClientId** - MerchantId = b521b6b2-b9b4-4a30-881d-3b63dece0006
* **ClientSecret** - 08Qkje79NwWRx5BdgNJsIkBuITt5cIVO
* **ClientId:ClientSecret** - *b521b6b2-b9b4-4a30-881d-3b63dece0006:08Qkje79NwWRx5BdgNJsIkBuITt5cIVO*
* **Base64** - YjUyMWI2YjItYjliNC00YTMwLTg4MWQtM2I2M2RlY2UwMDA2OiAwOFFramU3OU53V1J4NUJkZ05Kc0lrQnVJVHQ1Y0lWTw

### Request

``` json
"POST": https://cieloecommerce.cielo.com.br/v2/public/v2/token
"Headers"
"Authorization": Basic YjUyMWI2YjItYjliNC00YTMwLTg4MWQtM2I2M2RlY2UwMDA2OiAwOFFramU3OU53V1J4NUJkZ05Kc0lrQnVJVHQ1Y0lWTw
```

### Response


``` json
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJjbGllbnRfbmFtZSI6Ik1ldUNoZWNrb3V0IE1hc3RlciBLZXkiLCJjbGllbnRfaWQiOiJjODlmZGasdasdasdmUyLTRlNzctODA2YS02ZDc1Y2QzOTdkYWMiLCJzY29wZXMiOiJ7XCJTY29wZVwiOlwiQ2hlY2tvdXRBcGlcIixcIkNsYWltc1wiOltdfSIsInJvbGUiOiJasdasdasd291dEFwaSIsImlzc47I6Imh0dHBzOi8vYXV0aGhvbasdasdnJhc3BhZy5jb20uYnIiLCJhdWQiOiJVVlF4Y1VBMmNTSjFma1EzSVVFbk9pSTNkbTl0ZmasdsadQjVKVVV1UVdnPSIsImV4cCI6MTQ5Nzk5NjY3NywibmJmIjoxNDk3OTEwMjc3fQ.ozj4xnH9PA3dji-ARPSbI7Nakn9dw5I8w6myBRkF-uA",
    "token_type": "bearer",
    "expires_in": 1199
}
```

|PROPRIEDADE| DESCRIÇÃO                                                |TIPO|
|-----------|----------------------------------------------------------|----|
|Access_token| Utilizado para acesso aos serviços da API|string|
|Token_type| Sempre será do tipo “bearer”|texto|
|Expires_in| Validade do token em segundos. Aproximadamente 20 minutos|int|

Como se pode observar, o token possui um tempo de expiração aproximado de 20 minutos (1.200 segundos). Portanto, quando o mesmo expirar, será necessário obter um novo token para acesso aos serviços. 

### Utilizando Serviços

Para utilizar os serviços da API de controle transacional, deve ser enviado em todas as requisições o token de acesso obtido na etapa de autorização.
O mesmo deve ser enviado no header da requisição, conforme abaixo:


> `Authorization`: `Bearer` {access_token}


**Exemplo**


``` json
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJjbGllbnRfbmFtZSI6Ik1ldUNoZWNrb3V0IE1hc3RlciBLZXkiLCJjbGllbnRfaWQiOiJjODlmZGasdasdasdmUyLTRlNzctODA2YS02ZDc1Y2QzOTdkYWMiLCJzY29wZXMiOiJ7XCJTY29wZVwiOlwiQ2hlY2tvdXRBcGlcIixcIkNsYWltc1wiOltdfSIsInJvbGUiOiJasdasdasd291dEFwaSIsImlzcyI6Imh0dHBzOi8vYXV0aGhvbasdasdnJhc3BhZy5jb20uYnIiLCJhdWQiOiJVVlF4Y1VBMmNTSjFma1EzSVVFbk9pSTNkbTl0ZmasdsadQjVKVVV1UVdnPSIsImV4cCI6MTQ5Nzk5NjY3NywibmJmIjoxNDk3OTEwMjc3fQ.ozj4xnH9PA3dji-ARPSbI7Nakn9dw5I8w6myBRkF-uA
```

## Consultar transação

Permite consultar uma transação pelo número do pedido gerado pelo Checkout Cielo

> `GET` https://cieloecommerce.cielo.com.br/api/public/v2/orders/`{checkout_cielo_order_number}`

**Header:**

```
Authorization: Bearer {access_token}
```


**Resposta**

> "HTTP Status": 200 – OK

``` json
{ 
    "merchantId": "c89fdfbb-dbe2-4e77-806a-6d75cd397dac", 
    "orderNumber": "054f5b509f7149d6aec3b4023a6a2957", 
    "softDescriptor": "Pedido1234", 
    "cart": { 
        "items": [ 
            { 
                "name": "Pedido ABC", 
                "description": "50 canetas - R$30,00 | 10 cadernos - R$50,00 | 10 Borrachas - R$10,00", 
                "unitPrice": 9000, 
                "quantity": 1, 
                "type": "1" 
            } 
        ] 
    }, 
    "shipping": { 
        "type": "FixedAmount", 
        "services": [ 
            { 
              "name": "Entrega Rápida", 
                "price": 2000 
            } 
        ], 
        "address": { 
            "street": "Estrada Caetano Monteiro", 
            "number": "391A", 
            "complement": "BL 10 AP 208", 
            "district": "Badu", 
            "city": "Niterói", 
            "state": "RJ" 
        } 
    }, 
    "payment": { 
        "status": "Paid", 
        "antifraud": { 
            "description": "Lojista optou não realizar a análise do antifraude." 
        } 
    }, 
    "customer": { 
        "identity": "12345678911", 
        "fullName": "Fulano da Silva", 
        "email": "exemplo@email.com.br", 
        "phone": "11123456789" 
    }, 
    "links": [ 
        { 
            "method": "GET", 
            "rel": "self", 
            "href": "https://cieloecommerce.cielo.com.br/api/public/v2/orders/054f5b509f7149d6aec3b4023a6a2957" 
        }, 
        { 
            "method": "PUT", 
            "rel": "void", 
            "href": "https://cieloecommerce.cielo.com.br/api/public/v2/orders/054f5b509f7149d6aec3b4023a6a2957/void" 
        } 
    ] 
}
```

## Capturar transação

Permite capturar uma transação pelo número do pedido.

**Captura Total**
> `PUT` https://cieloecommerce.cielo.com.br/api/public/v2/orders/`{checkout_cielo_order_number}`/capture 

**Captura Parcial**

> `PUT` https://cieloecommerce.cielo.com.br/api/public/v2/orders/`{checkout_cielo_order_number}`/capture?amount={Valor}


**OBS**: A captura parcial pode ser realizada apenas 1 vez e é exclusiva para cartão de crédito.

**Header:**

```
Authorization: Bearer {access_token}
```

**Resposta**

> HTTP Status: 200 – OK

```
{ 
    "success": true, 
    "status": 2, 
    "returnCode": "6", 
    "returnMessage": "Operation Successful", 
    "links": [ 
        { 
            "method": "GET", 
            "rel": "self", 
            "href": "https://cieloecommerce.cielo.com.br/api/public/v2/orders/a9d517d81fb24b98b2d16eae2744be96" 
        }, 
        { 
            "method": "PUT", 
            "rel": "void", 
            "href": "https://cieloecommerce.cielo.com.br/api/public/v2/orders/a9d517d81fb24b98b2d16eae2744be96/void" 
        } 
    ] 
} 
```

### Cancelar transação

Permite cancelar uma transação pelo número do pedido.

**Cancelamento total**

> `PUT` https://cieloecommerce.cielo.com.br/api/public/v2/orders/`{checkout_cielo_order_number}`/void  

**Camcelamento Parcial**

> `PUT` https://cieloecommerce.cielo.com.br/api/public/v2/orders/`{checkout_cielo_order_number}`/void?amount={Valor}


**OBS**: O cancelamento parcial pode ser realizada apenas após a captura. O cancelamento parcial pode ser realizado inumeras vezes até que o valor total seja cancelado.



**Header:**

```
Authorization: Bearer {access_token}
```

**Resposta**
```
HTTP Status: 200 – OK
```

```
{ 
    "success": true, 
    "status": 2, 
    "returnCode": "6", 
    "returnMessage": "Operation Successful", 
    "links": [ 
        { 
            "method": "GET", 
            "rel": "self", 
            "href": "https://cieloecommerce.cielo.com.br/api/public/v2/orders/a9d517d81fb24b98b2d16eae2744be96" 
        }, 
        { 
            "method": "PUT", 
            "rel": "void", 
            "href": "https://cieloecommerce.cielo.com.br/api/public/v2/orders/a9d517d81fb24b98b2d16eae2744be96/void" 
        } 
    ] 
} 

```

# API - LINK DE PAGAMENTO

A **API de Gerenciamento de Link de Pagamentos** permite ao lojista criar, editar e consultar links de pagamentos. 

Seu principal objetivo é permitir que lojas possam criar links de pagamento (Botões ou QR Codes), através de seus próprios sistemas, sem a necessidade de acessar o Backoffice do Checkout Cielo e compartilhar com seus clientes.

> Endereço: <https://cieloecommerce.cielo.com.br/api/public/v1/products>


> **Atenção**: O link de pagamentos não é uma URL DE **PEDIDO/TRANSAÇÃO**. Ele é um "carrinho" que pode ser reutilizado inúmeras vezes.


> **Atenção**: Para receber notificações sobre transações originadas de Links de pagamento é OBRIGATÓRIO o cadastro da URL de Notificação.

## Caso de uso

Está é um exemplo de como usar a ferramenta de link / Qr code / Botão de pagamentos para elevar suas vendas! 

Para facilitar a leitura, vamos chamar essas opções de "LQB".

Primeiro, nos deixe explicar quais as diferenças entre essas 3 opções.  Em si, eles são 3 meios de acessar a nossa tela de pagamento sem necessariamente uma integração técnica com APIs etc. 

* **Link** - é uma url encurtada que pode ser enviada via redes sociais. Muito prático para acesso via computadores!
* **QR Code** - é a uma imagem que quando lida por um aparelho com leitor QR code (um APP no celular por exemplo), leva o comprador para a tela de pagamento. Ótimo para publicidade impressa!
* **Botão** - é uma Imagem embutida em código HTML para ser usada em um site. Muito bom caso você possua um site, mas não quer lidar com a criação de carrinhos ou fluxos de pagamento. Coloque em seu site e pronto, o botão leva os compradores para a tela de pagamentos

O LQB está disponível quando você registra um carrinho no backoffice do Checkout Cielo.
Basta seguir o caminho abaixo:

Acesso de lojista no site Cielo >  Vendas Online >  Checkout Cielo > ABA: Produtos >. Cadastrar produtos

Pronto! Após cadastrar você já tem um LQB esperando para ser usado!

Veja um exemplo de uso:

* Link + Recorrência:

A empresa PagBras realiza uma festa de aniversário com os funcionários todos os meses, regada a refrigerantes e salgadinhos, fornecidos pela própria companhia, que dizem, serem muito bons! 

Um dia os funcionários decidiram realizar uma "vaquinha" e contribuir mensalmente para que a variedade de petiscos e bebidas fosse maior, assim podendo fazer festas temáticas como festas juninas e de natal por exemplo.

O que eles fizeram? Sendo uma empresa antenada que não queria passar uma caixinha todos os meses para recolher a contribuição mensal, um dos funcionários criou uma recorrência via LQB, e em um grupo do Facebook da empresa, publicou o link de pagamentos. 

Hoje os funcionários contribuem mensalmente sem ter que lembrar de pagar, uma vez que a Recorrência do Checkout Cielo realiza uma nova transação de cobrança todos os meses!

### Autorização de acesso

A API de Gerenciamento de `Links de Pagamento` utiliza como forma de segurança o **protocolo OAUTH**.
Para acessá-la será necessário possuir: 

| PROPRIEDADE    | DESCRIÇÃO                                                      | TIPO   |
|----------------|----------------------------------------------------------------|--------|
| `ClientId`     | Mesmo que o **MerchantId**                                     | guid   |
| `ClientSecret` | Chave secreta que deverá ser obtida dentro do backoffice Cielo | string |

Para obter as credênciais de acesso a API, basta seguir o fluxo abaixo:

1 - Acesso o Backoffice **Checkout Cielo**
2 - Na aba **"Configurações"**, acesse a opção **"Dados cadastrais"** clique em **"Gerar Credenciais de acesso às APIs"** como na imagem abaixo

![backoffice](http://moss-beaver.cloudvent.net/images/gerarcredenciais.png)

3 - A mensagem abaixo será enviada para o **e-mail de contato tecnico**, contendo as credenciais de acesso:

![e-mail](http://moss-beaver.cloudvent.net/images/emailcredencial.png)

> Caso o botão de **"Gerar Credenciais de acesso às APIs"** não esteja disponivel em seu backoffice, acione a equipe de Produtos Cielo para a liberação da funcionalidade

Para obter acesso aos serviços da API de Gerenciamento de `Links de Pagamento`, será necessário obter um token de acesso, conforme os passos abaixo:

1. Concatenar o _ClientId_ e o _ClientSecret_, **ClientId:ClientSecret**
2. Codificar o resultado em *Base64*
3. Enviar uma requisição, utilizando o método HTTP POST, para obter o token de acesso conforme abaixo:

|**POST**|https://cieloecommerce.cielo.com.br/api/public/v2/token|
|**Authorization**|Basic {Base64}|

**EXEMPLO**

* **ClientId** - MerchantId = b521b6b2-b9b4-4a30-881d-3b63dece0006
* **ClientSecret** - 08Qkje79NwWRx5BdgNJsIkBuITt5cIVO
* **ClientId:ClientSecret** - *b521b6b2-b9b4-4a30-881d-3b63dece0006:08Qkje79NwWRx5BdgNJsIkBuITt5cIVO*
* **Base64** - YjUyMWI2YjItYjliNC00YTMwLTg4MWQtM2I2M2RlY2UwMDA2OiAwOFFramU3OU53V1J4NUJkZ05Kc0lrQnVJVHQ1Y0lWTw


**REQUISIÇÃO**
```
"POST": https://cieloecommerce.cielo.com.br/public/v2/token
"Headers"
"Authorization": Basic YjUyMWI2YjItYjliNC00YTMwLTg4MWQtM2I2M2RlY2UwMDA2OiAwOFFramU3OU53V1J4NUJkZ05Kc0lrQnVJVHQ1Y0lWTw
```

**RESPOSTA**
```
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJjbGllbnRfbmFtZSI6Ik1ldUNoZWNrb3V0IE1hc3RlciBLZXkiLCJjbGllbnRfaWQiOiJjODlmZGasdasdasdmUyLTRlNzctODA2YS02ZDc1Y2QzOTdkYWMiLCJzY29wZXMiOiJ7XCJTY29wZVwiOlwiQ2hlY2tvdXRBcGlcIixcIkNsYWltc1wiOltdfSIsInJvbGUiOiJasdasdasd291dEFwaSIsImlzcyI6Imh0dHBzOi8vYXV0aGhvbasdasdnJhc3BhZy5jb20uYnIiLCJhdWQiOiJVVlF4Y1VBMmNTSjFma1EzSVVFbk9pSTNkbTl0ZmasdsadQjVKVVV1UVdnPSIsImV4cCI6MTQ5Nzk5NjY3NywibmJmIjoxNDk3OTEwMjc3fQ.ozj4xnH9PA3dji-ARPSbI7Nakn9dw5I8w6myBRkF-uA",
    "token_type": "bearer",
    "expires_in": 1199
}
```

| PROPRIEDADE  | DESCRIÇÃO                                                 | TIPO   |
|--------------|-----------------------------------------------------------|--------|
| access_token | Utilizado para acesso aos serviços da API                 | string |
| token_type   | Sempre será do tipo “bearer”                              | texto  |
| expires_in   | Validade do token em segundos. Aproximadamente 20 minutos | int    |

Como se pode observar, o token possui um tempo de expiração aproximado de 20 minutos (1.200 segundos). Portanto, quando o mesmo expirar, será necessário obter um novo token para acesso aos serviços. 

### Utilizando Serviços

Para utilizar os serviços da API de Gerenciamento de Link, deve ser enviado em todas as requisições o token de acesso obtido na etapa de autorização.
O mesmo deve ser enviado no header da requisição, conforme abaixo:

```
Authorization: Bearer {access_token}
```

**Exemplo**

```
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJjbGllbnRfbmFtZSI6Ik1ldUNoZWNrb3V0IE1hc3RlciBLZXkiLCJjbGllbnRfaWQiOiJjODlmZGasdasdasdmUyLTRlNzctODA2YS02ZDc1Y2QzOTdkYWMiLCJzY29wZXMiOiJ7XCJTY29wZVwiOlwiQ2hlY2tvdXRBcGlcIixcIkNsYWltc1wiOltdfSIsInJvbGUiOiJasdasdasd291dEFwaSIsImlzcyI6Imh0dHBzOi8vYXV0aGhvbasdasdnJhc3BhZy5jb20uYnIiLCJhdWQiOiJVVlF4Y1VBMmNTSjFma1EzSVVFbk9pSTNkbTl0ZmasdsadQjVKVVV1UVdnPSIsImV4cCI6MTQ5Nzk5NjY3NywibmJmIjoxNDk3OTEwMjc3fQ.ozj4xnH9PA3dji-ARPSbI7Nakn9dw5I8w6myBRkF-uA
```

### Gerar um Link

Você pode criar um link para disponibilizá-los aos seus clientes para pagamentos. Para criar diversos links, você pode efetuar várias requisições.

> URL de POST <https://cieloecommerce.cielo.com.br/api/public/v1/products/>

**Header:**

```
Authorization: Bearer {access_token}
```

**Requisição:**
```
{
   "Type":"Digital",
   "name" : "Pedido",
   "description" : "teste description",
   "price":"1000000",
   "weight": 2000000,
   "ExpirationDate":"2037-06-19",
   "maxNumberOfInstallments":"1",
   "quantity": 2,
   "Sku":"teste",
   "shipping":{
     "type":"WithoutShipping",
     "name": "teste",
     "price":"1000000000"
   },
   "SoftDescriptor" : "Pedido1234"
}
```

**Dados do produto**

| PROPRIEDADE                 | DESCRIÇÃO              | TIPO                                                                                | TAMANHO    | OBRIGATÓRIO |
|-----------------------------|------------------------|-------------------------------------------------------------------------------------|------------|-------------|
| type                        | Tipo de venda a ser realizada através do link de pagamento: <br><br>**Asset** – Material Físico<br>**Digital** – Produto Digital<br>**Service** – Serviço<br>**Payment** – Pagamentos Simples<br>**Recurrent** – Pagamento Recorrente | String                                                                          | 255        | SIM        |
| name                        | Nome do produto        | String                                                                              | 128        | SIM        |
| description                 | Descrição do produto que será exibida na tela de Checkout caso a opção show_description seja verdadeira.Pode-se utilizar o caracter pipe ` | ` caso seja desejável quebrar a linha ao apresentar a descrição na tela de Checkout | String     | 512        |
| showDescription             | Flag indicando se a descrição deve ou não ser exibida na tela de Checkout                                    | String     | --         | Não         |
| price                       | Valor do produto em **centavos**                                                                             | Int        | 1000000    | SIM         |
| expirationDate              | Data de expiração do link. Caso uma data senha informada, o link se torna indisponível na data informada. Se nenhuma data for informada, o link não expira.| String                                                                             | YYYY-MM-DD | Não         |
| weight                      | Peso do produto em **gramas**| String                                                                        | 2000000    | Não         |
| softDescriptor              | Descrição a ser apresentada na fatura do cartão de crédito do portador.| String                              | 13         | Não         |
| maxNumberOf<br>Installments | Número máximo de parcelas que o comprador pode selecionar na tela de Checkout.Se não informado será utilizada as configurações da loja no Checkout Cielo.| int| 2| Não|

**Dados do Frete**

| PROPRIEDADE            | DESCRIÇÃO                                                                                      | TIPO   | TAMANHO | OBRIGATÓRIO |
|------------------------|------------------------------------------------------------------------------------------------|--------|---------|-------------|
| shipping               | Nó contendo informações de entrega do produto                                                  |        |         |             |
| shipping.name          | Nome do frete. **Obrigatório para frete tipo “FixedAmount”**                                   | string | 128     | Sim         |
| shipping.price         | O valor do frete. **Obrigatório para frete tipo “FixedAmount”**                                | int    | 100000  | Sim         |
| shipping.originZipCode | Cep de origem da encomenda. Obrigatório para frete tipo “Correios”. Deve conter apenas números | string | 8       | Não         |
|shipping.type|Tipo de frete.<br>**Correios** – Entrega pelos correios<br>**FixedAmount** – Valor Fixo<br>**Free** - Grátis<br>**WithoutShippingPickUp** – Sem entrega com retirada na loja<br>**WithoutShipping** – Sem entrega<br><br>Se o tipo de produto escolhido for “**Asset**”, os tipos permitidos de frete são: _**“Correios, FixedAmount ou Free”**_.<br><br>Se o tipo produto escolhido for “**Digital**” ou “**Service**”, os tipos permitidos de frete são: _**“WithoutShipping, WithoutShippingPickUp”**_.<br><br>Se o tipo produto escolhido for “**Recurrent**” o tipo de frete permitido é: _**“WithoutShipping”**_.|string|255|Sim|

**Dados da Recorrência**

|PROPRIEDADE|DESCRIÇÃO|TIPO|TAMANHO|OBRIGATÓRIO|
|-----------|---------|----|-------|-----------|
|recurrent|Nó contendo informações da recorrência do pagamento.Pode ser informado caso o tipo do produto seja “Recurrent”|-|-|
|recurrent.interval|Intervalo de cobrança da recorrência.<br><br>**Monthly** - Mensal<br>**Bimonthly** - Bimensal<br>**Quarterly** - Trimestral<br>**SemiAnnual** - Semestral<br>**Annual** – Anual<br>|string|128|Não|
|recurrrent.endDate|Data de término da recorrência. Se não informado a recorrência não terá fim, a cobrança será realizada de acordo com o intervalo selecionado indefinidamente.|string|128|Não|


**Resposta**
```
"HTTP Status": 201 – Created
```
```
{
    "id": "529aca91-2961-4976-8f7d-9e3f2fa8a0c9",
    "shortUrl": "http://bit.ly/2smqdhD",
    "type": "Asset",
    "name": "Pedido ABC",
    "description": "50 canetas - R$30,00 | 10 cadernos - R$50,00",
    "showDescription": false,
    "price": 8000,
    "weight": 4500,
    "shipping": {
        "type": "Correios",
        "originZipcode": "06455030"
    },
    "softDescriptor": "Pedido1234",
    "expirationDate": "2017-06-30T00:00:00",
    "maxNumberOfInstallments": 2,
    "links": [
        {
            "method": "GET",
            "rel": "self",
            "href": "https://cieloecommerce.cielo.com.br/Api/public/v1/product/529aca91-2961-4976-8f7d-9e3f2fa8a0c9"
        },
        {
            "method": "PUT",
            "rel": "update",
            "href": "https://cieloecommerce.cielo.com.br/Api/public/v1/product/529aca91-2961-4976-8f7d-9e3f2fa8a0c9"
        },
        {
            "method": "DELETE",
            "rel": "delete",
            "href": "https://cieloecommerce.cielo.com.br/Api/public/v1/product/529aca91-2961-4976-8f7d-9e3f2fa8a0c9"
        }
    ]
}
```


Os dados retornados na resposta contemplam todos os enviados na requisição e dados adicionais referentes a criação do link.

| PROPRIEDADE | TIPO   | DESCRIÇÃO                                                                                                                     |
|-------------|--------|-------------------------------------------------------------------------------------------------------------------------------|
| id          | guid   | Identificador único do link de pagamento.Pode ser utilizado para consultar, atualizar ou excluir o link.                      |
| shortUrl    | string | Representa o link de pagamento que ao ser aberto, em um browser, apresentará a tela do Checkout Cielo.                        |
| links       | object | Apresenta as operações disponíveis e possíveis (RESTful hypermedia) de serem efetuadas após a criação ou atualização do link. |


### Consultar um Link

Consultar um link existente pelo seu identificador.
**Definição**

> `GET`: https://cieloecommerce.cielo.com.br/api/public/v1/products/`{id}` 


**Header:**

```
Authorization: Bearer {access_token}
```

**Resposta**
```
HTTP Status: 200 – OK
```
```
{
    "id": "529aca91-2961-4976-8f7d-9e3f2fa8a0c9",
    "shortUrl": "http://bit.ly/2smqdhD",
    "type": "Asset",
    "name": "Pedido ABC",
    "description": "50 canetas - R$30,00 | 10 cadernos - R$50,00",
    "showDescription": false,
    "price": 8000,
    "weight": 4500,
    "shipping": {
        "type": "Correios",
        "originZipcode": "06455030"
    },
    "softDescriptor": "Pedido1234",
    "expirationDate": "2017-06-30",
    "maxNumberOfInstallments": 2,
    "links": [
        {
            "method": "GET",
            "rel": "self",
            "href": "https://cieloecommerce.cielo.com.br/Api/public/v1/products/529aca91-2961-4976-8f7d-9e3f2fa8a0c9"
        },
        {
            "method": "PUT",
            "rel": "update",
            "href": "https://cieloecommerce.cielo.com.br/Api/public/v1/products/529aca91-2961-4976-8f7d-9e3f2fa8a0c9"
        },
        {
            "method": "DELETE",
            "rel": "delete",
            "href": " https://cieloecommerce.cielo.com.br/Api/public/v1/products/529aca91-2961-4976-8f7d-9e3f2fa8a0c9"
        }
    ]
}
```
**OBS**: Mesmos dados retornados na resposta de criação do link.

### Atualizar um Link

Atualiza um link pelo seu identificador.
**Definição**

> `PUT`: https://cieloecommerce.cielo.com.br/api/public/v1/products/`{id}` 


**Header:**

```
Authorization: Bearer {access_token}
```

**Requisição**
```
{
   "Type":"Asset",
   "name" : "Pedido ABC",
   "description" : "50 canetas - R$30,00 | 10 cadernos - R$50,00 | 10 Borrachas - R$10,00",
   "price": 9000,
   "expirationDate": "2017-06-30",
   "weight": 4700,
   "shipping":{
     "type":"FixedAmount",
     "name":"Entrega Rápida",
     "price":1000
   },
   "SoftDescriptor" : "Pedido1234",
   "maxNumberOfInstallments" : 2
}
```

 
**Resposta**
```
HTTP Status: 200 – OK
```

```
{
    "id": "529aca91-2961-4976-8f7d-9e3f2fa8a0c9",
    "shortUrl": "http://bit.ly/2smqdhD",
    "type": "Asset",
    "name": "Pedido ABC",
    "description": "50 canetas - R$30,00 | 10 cadernos - R$50,00 | 10 Borrachas - R$10,00",
    "showDescription": false,
    "price": 9000,
    "weight": 4700,
    "shipping": {
        "type": "FixedAmount",
        "name": "Entrega Rápida",
        "price": 1000
    },
    "softDescriptor": "Pedido1234",
    "expirationDate": "2017-06-30",
    "maxNumberOfInstallments": 2,
    "links": [
        {
            "method": "GET",
            "rel": "self",
            "href": "https://cieloecommerce.cielo.com.br/Api/public/v1/products/529aca91-2961-4976-8f7d-9e3f2fa8a0c9"
        },
        {
            "method": "PUT",
            "rel": "update",
            "href": "https://cieloecommerce.cielo.com.br/Api/public/v1/products/529aca91-2961-4976-8f7d-9e3f2fa8a0c9"
        },
        {
            "method": "DELETE",
            "rel": "delete",
            "href": "https://cieloecommerce.cielo.com.br/Api/public/v1/products/529aca91-2961-4976-8f7d-9e3f2fa8a0c9"
        }
    ]
}

```
**OBS**: Mesmos dados retornados na resposta de criação do link.

### Excluir um Link

Exclui um link pelo seu identificador.

**Definição**

> `DELETE`: https://cieloecommerce.cielo.com.br/api/public/v1/products/`{id}` 


**Header:**

```
Authorization: Bearer {access_token}
```

**Resposta**

```
HTTP Status: 204 – No Content
```

### Códigos de Status HTTP

| CÓDIGO                      | DESCRIÇÃO                                                                                                  |
|-----------------------------|------------------------------------------------------------------------------------------------------------|
| 200 - OK                    | Tudo funcionou corretamente.                                                                               |
| 400 – Bad Request           | A requisição não foi aceita. Algum parâmetro não foi informado ou foi informado incorretamente.            |
| 401 - Unauthorized          | O token de acesso enviado no header da requisição não é válido.                                            |
| 404 – Not Found             | O recurso sendo acessado não existe. Ocorre ao tentar atualizar, consultar ou excluir um link inexistente. |
| 500 – Internal Server Error | Ocorreu um erro no sistema.                                                                                |
