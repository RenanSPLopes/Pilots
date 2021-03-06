---
layout: manual
title: Gateway Antifraude 3.0
description: Integração técnica API Antifraude Gateway Braspag
search: true
categories: manual
tags:
  - Gestão de Risco
language_tabs:
  json: JSON
  shell: cURL
---

# Gateway AF - ReD Shield

> Na análise de AF, os padrões de siglas para países utilizados nos campos `Country` devem seguir o modelo da ISO 3166-1 ALPHA 3 - https://en.wikipedia.org/wiki/ISO_3166-1_alpha-3

## Criando uma transação com Análise de Fraude ReD Shield

Para que a análise de fraude seja efetuada em tempo de transação, é necessário complementar a mensagem com os dados mencionados no nó "FraudAnalysis".

### Requisição

<aside class="request"><span class="method post">POST</span> <span class="endpoint">/1/sales/</span></aside>

```shell

curl
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
   "MerchantOrderId":"2014111457673454307",
   "Customer":{
      "Name":"Comprador Teste",
      "Identity":"13987441747",
      "IdentityType":"CPF",
      "MerchantCustomerId":"1325374899",
      "Email":"compradorteste@live.com",
      "Birthdate":"1991-01-02",
      "Phone":"21114740",
      "WorkPhone":"21114740",
      "Mobile":"21114740",
      "Address":{
         "Street":"Rua Júpter",
         "Number":"174",
         "Complement":"AP 201",
         "District":"Centro",
         "ZipCode":"21241140",
         "City":"Rio de Janeiro",
         "State":"RJ",
         "Country":"BRASIL"
      },
      "DeliveryAddress":{
         "Street":"Rua Júpter",
         "Number":"174",
         "Complement":"AP 201",
         "District":"Centro",
         "ZipCode":"21241140",
         "City":"Rio de Janeiro",
         "State":"RJ",
         "Country":"BR"
      },
      "BillingAddress":{
         "Street":"Rua Júpter",
         "Number":"174",
         "Complement":"AP 201",
         "ZipCode":"21241140",
         "City":"Rio de Janeiro",
         "State":"RJ",
         "Country":"BR",
         "Neighborhood":"Centro"
      },
      "Status":"NEW"
   },
   "Payment":{
      "Type":"CreditCard",
      "Amount":100,
      "Currency":"BRL",
      "Country":"BRA",
      "Provider":"Simulado",
      "ServiceTaxAmount":0,
      "Installments":1,
      "SoftDescriptor":"123456789ABCD",
      "Interest":"ByMerchant",
      "Capture":false,
      "Authenticate":false,
      "CreditCard":{
         "CardNumber":"6011000000000111",
         "Holder":"Teste accept",
         "ExpirationDate":"12/2019",
         "SecurityCode":"023",
         "Brand":"Discover",
         "EciThreeDSecure":"05"
      },
      "FraudAnalysis":{
         "Sequence":"AnalyseFirst",
         "SequenceCriteria":"Always",
         "FingerPrintId":"074c1ee676ed4998ab66491013c565e2",
         "Provider":"RedShield",
         "CaptureOnLowRisk":true,
         "VoidOnHighRisk":false,
         "TotalOrderAmount":"150000",
         "OrderDate":"2016-12-09",
         "IsRetryTransaction":false,
         "SplitingPaymentMethod":"SINGLE-PAY",
         "Browser":{
            "CookiesAccepted":false,
            "Email":"compradorteste@live.com",
            "HostName":"Teste",
            "IpAddress":"187.32.163.105",
            "Type":"Chrome",
            "BrowserFingerPrint":"04003hQUMXGB0poNf94lis1ztuLYRFk+zJ17aP79a9O8mWOBmEnKs6ziAo94ggAtBvKEN6/FI8Vv2QMAyHLnc295s0Nn8akZzRJtHwsEilYx1P+NzuNQnyK6+7x2OpjJZkl4NlfPt7h9d96X/miNlYT65UIY2PeH7sUAh9vKxMn1nlPu2MJCSi12NBBoiZbfxP1Whlz5wlRFwWJi0FRulruXQQGCQaJkXU7GWWZGI8Ypycnf7F299GIR12G/cdkIMFbm6Yf0/pTJUUz1vNp0X2Zw8QydKgnOIDKXq4HnEqNOos1c6njJgQh/4vXJiqy0MXMQOThNipDmXv9I185O+yC2f3lLEO0Tay66NZEyiLNePemJKSIdwO9O5ZtntuUkG6NTqARuHStXXfwp8cyGF4MPWLuvNvEfRkJupBy3Z8hSEMEK7ZWd2T2HOihQxRh4qp+NANqYKBTl3v6fQJAEKikeSQVeBN8sQqAL0BZFaIMzbrnMivi6m6JRQUIdvEt+MbJEPFc0LjRycC5ApUmJO+Aoo9VKL1B8ftMSQ1iq1uTKn16ZOmDpzZrZhMPbH83aV0rfB2GDXcjpghm9klVFOw7EoYzV7IDBIIRtgqG9KZ+8NH/z6D+YNUMLEUuK1N2ddqKbS5cKs2hplVRjwSv7x8lMXWE7VDaOZWB8+sD1cMLQtEUC0znzxZ4bpRaiSy4dJLxuJpQYAFUrDlfSKRv/eHV3QiboXLuw9Lm6xVBK8ZvpD5d5olGQdc+NgsqjFnAHZUE+OENgY4kVU9wB84+POrI4MkoD4iHJ5a1QF8AZkZDFo1m1h9Bl+J2Ohr6MkBZq8DG5iVaunHfxUdHou5GL7lS1H7r+8ctfDXi8AfOPjzqyODJQ74Aiel35TKTOWG8pq1WO6yzJ1GNmMuMWZBamlGXoG/imnjwHY9HQtQzpGfcm0cR8X2Fd1ngNFGLDGZlWOX0jWtOwU6XVGT37JFD9W/cx4kzI+mPNi65X5WFPYlDG9N0Lbh5nOj3u3DXqRCiKCUrsEkMt8z9fxO9pLLGVQUKIYR2wTw53CiWK96FOpPevDWtH2XR0QkfOd02D73n81x6hEMCy0s3hRLn08Th9FlNHDMJBqLj+Tz8rG2TtNki3mJC7Ass1MT2qnKBI77n6vsQkAp59TfbZm/tBXwAoYdLJXge8F/numhd5AvQ+6I8ZHGJfdN3qWndvJ2I7s5Aeuzb8t9//eNsm73fIa05XreFsNyfOq1vG2COftC6EEsoJWe5h5Nwu1x6PIKuCaWxLY+npfWgM0dwJPmSgPx7TNM31LyVNS65m83pQ+qMTRH6GRVfg7HAcS5fnS/cjdbgHxEkRmgkRq1Qs48sbX9QC8nOTD0ntb6FcJyEOEOVzmJtDqimkzDq+SXR1/63AYe4LEj+ogRgN+Z8HAFhGFzd/m6snVviELfRqJ4LLQIk9Y/fzqnsF6I5OGxfdT2sxxK2Vokpi3jWhCcEknw7dYlHYpOnCHZO7QVgjQTngF2mzKf4GeOF4ECFsWTgLy6HFEitfauYJt1Xh1NfZZerBMwXLFzdhzoTQxGlcXc8lZIoEG1BLYv/ScICf8Ft9PEtpEa+j0cDSlU99UoH2xknwR1W9MRGc5I/euE63/IMJTqguZ3YcnJpjSVnAGSpyz/0gKjypJ3L86rHFRGXt0QbmaXtSl2UmmjI0p0LCCdx7McatCFEVI6FwPpPV0ZSMv/jM75eBid1X/lTV4XNzjowzR/iFlKYMzHZtVO9hCBPKlTwblRXNn4MlvNm/XeSRQ+Mr0YV5w5CL5Z/tGyzqnaLPj/kOVdyfj8r2m5Bcrz4g/ieUIo8qRFv2T2mET46ydqaxi27G4ZYHj7hbiaIqTOxWaE07qMCkJw=="
         },
         "Cart":{
            "IsGift":false,
            "ReturnsAccepted":true,
            "Items":[
               {
                  "Type":"AdultContent",
                  "Name":"ItemTeste",
                  "Risk":"High",
                  "Sku":"201411170235134521346",
                  "MerchantItemId":"1234",
                  "UnitPrice":123,
                  "OriginalPrice":234,
                  "Quantity":1,
                  "HostHedge":"Off",
                  "NonSensicalHedge":"Off",
                  "ObscenitiesHedge":"Off",
                  "PhoneHedge":"Off",
                  "TimeHedge":"Normal",
                  "VelocityHedge":"High",
                  "GiftCategory":"Undefined",
                  "GiftMessage":"Te amo!",
                  "Description":"Uma description do Mouse",
                  "ShippingInstructions":"Proximo ao 546",
                  "ShippingMethod":"SameDay",
                  "ShippingTrackingNumber":"123456",
                  "Passenger":{
                     "Name":"Comprador accept",
                     "Identity":"1234567890",
                     "Status":"Accepted",
                     "Rating":"Adult",
                     "Email":"compradorteste@live.com",
                     "Phone":"999994444"
                  }
               },
               {
                  "Type":"AdultContent",
                  "Name":"ItemTeste",
                  "MerchantItemId":"4",
                  "Risk":"High",
                  "Sku":"201411170235134521346",
                  "UnitPrice":123,
                  "OriginalPrice":12000,
                  "Quantity":1,
                  "HostHedge":"Off",
                  "NonSensicalHedge":"Off",
                  "ObscenitiesHedge":"Off",
                  "PhoneHedge":"Off",
                  "TimeHedge":"Normal",
                  "VelocityHedge":"High",
                  "GiftCategory":"Undefined",
                  "GiftMessage":"Te amo!",
                  "Description":"Uma description do Mouse",
                  "ShippingInstructions":"Proximo ao 546",
                  "ShippingMethod":"SameDay",
                  "ShippingTrackingNumber":"123456",
                  "Passenger":{
                     "Name":"Comprador accept",
                     "Identity":"1234567890",
                     "Status":"Accepted",
                     "Rating":"Adult",
                     "Email":"compradorteste@live.com",
                     "Phone":"999994444"
                  }
               }
            ]
         },
         "MerchantDefinedFields":[
            {
               "Id":95,
               "Value":"Eu defini isso"
            },
            {
               "Id":96,
               "Value":"Outra informação"
            }
         ],
         "Shipping":{
            "Addressee":"Sr Comprador Teste",
            "Email":"ffigueiredo@braspag.com.br",
            "Method":"LowCost",
            "Phone":"21114740",
            "WorkPhone":"123456789-78945612",
            "Mobile":"987456-123456",
            "Comment":"Em frente ao 322."
         },
         "CustomConfiguration":{
            "ServiceId":"0",
            "RiskAmount":0,
            "RiskBrand":"0",
            "MerchantWebsite":"www.test.com",
            "AccountTokenS":"acc123"
         },
         "Travel":{
            "Route":"GIG-CGH-EZE",
            "DepartueTime":"2016-12-10",
            "JouneyType":"JT",
            "Legs":[
               {
                  "Origin":"GIG",
                  "Destination":"CGH"
               },
               {
                  "Origin":"CGH",
                  "Destination":"EZE"
               }
            ]
         }
      }
   }
}
--verbose

```

| CAMPOS                              | TIPO  | TAMANHO | OBRIGATÓRIO | DESCRIÇÃO                                                                                                          | ANTIFRAUDE REDSHIELD                                |
|-------------------------------------|-------|---------|-------------|--------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------|
| MerchantOrderId                     | Texto | 50      | Sim         | Numero de identificação do Pedido.                                                                                 | Obrigatório                                         |

| CAMPOS                              | TIPO  | TAMANHO | OBRIGATÓRIO | DESCRIÇÃO                                                                                                          | ANTIFRAUDE REDSHIELD                                |
|-------------------------------------|-------|---------|-------------|--------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------|
| Customer                            |       |         | Não         | Nó contendo os dados do cliente                                                                                    |                                                     |
| Customer.Name                       | Texto | 255     | Não         | Nome do Comprador.                                                                                                 | ObrigatórioNome e Sobrenome                         |
| Customer.Identity                   | Texto | 18      | Não         | Número do RG, CPF, CNPJ ou Passaporte do Cliente.                                                                  | Obrigatório                                         |
| Customer.IdentityType               | Texto | 25      | Não         | Tipo de documento de identificação do comprador (RG/CFP/CNPJ/Passaporte)                                           | Opcional                                            |
| Customer.Email                      | Texto | 255     | Não         | Email do Comprador.                                                                                                | Opcional                                            |
| Customer.Phone                      | Texto | 15      | Não         | Telefone do Comprador                                                                                              | Opcional                                            |
| Customer.Mobile                     | Texto | 15      | Não         | Celular do Comprador                                                                                               | Opcional                                            |
| Customer.WorkPhone                  | Texto | 15      | Não         | Telefone de trabalho do Comprador                                                                                  | Opcional                                            |
| Customer.Birthdate                  | Date  | 10      | Não         | Data de nascimento do Comprador. Formato: YYYY-MM-DD                                                               | Opcional                                            |
| Customer.Status                     | Texto | -       | Não         | Status de cadastro do comprador na loja.NEW - Cliente novo efetuando a primeira compraEXISTING - Cliente existente | Opcional                                            |

| CAMPOS                              | TIPO  | TAMANHO | OBRIGATÓRIO | DESCRIÇÃO                                                                                                          | ANTIFRAUDE REDSHIELD                                |
|-------------------------------------|-------|---------|-------------|--------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------|
| Customer.Address                    | -     | -       | Não         | Nó contendo dados do Endereço do Comprador.                                                                        | Opcional                                            |
| Customer.Address.Street             | Texto | 255     | Não         | Endereço do Comprador.                                                                                             | -                                                   |
| Customer.Address.Number             | Texto | 15      | Não         | Número do endereço do Comprador.                                                                                   | -                                                   |
| Customer.Address.Complement         | Texto | 50      | Não         | Complemento do endereço do Comprador.                                                                              | -                                                   |
| Customer.Address.District           | Texto | 50      | Não         | Distrito ou Bairro do endereço do Comprador.                                                                       | -                                                   |
| Customer.Address.ZipCode            | Texto | 9       | Não         | CEP do endereço do Comprador.                                                                                      | -                                                   |
| Customer.Address.City               | Texto | 50      | Não         | Cidade do endereço do Comprador.                                                                                   | -                                                   |
| Customer.Address.State              | Texto | 2       | Não         | Estado do endereço do Comprador.                                                                                   | -                                                   |
| Customer.Address.Country            | Texto | 35      | Não         | Pais do endereço do Comprador.                                                                                     | -                                                   |

| CAMPOS                              | TIPO  | TAMANHO | OBRIGATÓRIO | DESCRIÇÃO                                               | ANTIFRAUDE REDSHIELD                                |
|-------------------------------------|-------|---------|-------------|---------------------------------------------------------|-----------------------------------------------------|
| Customer.DeliveryAddress            | -     | -       | Não         | Nó contendo os dados do Endereço de Entrega.            | Opcional                                            |
| Customer.DeliveryAddress.Street     | Texto | 255     | Não         | Endereço de Entrega.                                    | Opcional                                            |
| Customer.DeliveryAddress.Number     | Texto | 15      | Não         | Número do endereço do Entrega.                          | Opcional                                            |
| Customer.DeliveryAddress.Complement | Texto | 50      | Não         | Complemento do endereço de Entrega.                     | Opcional                                            |
| Customer.DeliveryAddress.District   | Texto | 50      | Não         | Distrito ou Bairro do endereço de Entrega.              | Opcional                                            |
| Customer.DeliveryAddress.ZipCode    | Texto | 9       | Não         | CEP do endereço de Entrega.                             | Opcional                                            |
| Customer.DeliveryAddress.City       | Texto | 50      | Não         | Cidade do endereço de Entrega.                          | Opcional                                            |
| Customer.DeliveryAddress.State      | Texto | 2       | Não         | Estado do endereço de Entrega.                          | Opcional                                            |
| Customer.DeliveryAddress.Country    | Texto | 35      | Não         | Pais do endereço de Entrega.                            | OpcionalDeve seguir a ISO 3166-1, com 2 caracteres. |

| CAMPOS                              | TIPO  | TAMANHO | OBRIGATÓRIO | DESCRIÇÃO                                               | ANTIFRAUDE REDSHIELD                                |
|-------------------------------------|-------|---------|-------------|---------------------------------------------------------|-----------------------------------------------------|
| Customer.BillingAddress             | -     | -       | Não         | Nós contendo os dados do Endereço de Cobrança           | Opcional                                            |
| Customer.BillingAddress.Street      | Texto | 255     | Não         | Endereço de Cobrança.                                   | Opcional                                            |
| Customer.BillingAddress.Number      | Texto | 15      | Não         | Número do endereço de Cobrança.                         | Opcional                                            |
| Customer.BillingAddress.Complement  | Texto | 50      | Não         | Complemento do endereço de Cobrança.                    | Opcional                                            |
| Customer.BillingAddress.District    | Texto | 50      | Não         | Distrito ou Bairro do endereço de Cobrança.             | Opcional                                            |
| Customer.BillingAddress.ZipCode     | Texto | 9       | Não         | CEP do endereço de Cobrança.                            | Opcional                                            |
| Customer.BillingAddress.City        | Texto | 50      | Não         | Cidade do endereço de Cobrança.                         | Opcional                                            |
| Customer.BillingAddress.State       | Texto | 2       | Não         | Estado do endereço de Cobrança.                         | Opcional                                            |
| Customer.BillingAddress.Country     | Texto | 35      | Não         | Pais do endereço de Cobrança.                           | OpcionalDeve seguir a ISO 3166-1, com 2 caracteres. |

| CAMPOS                   | TIPO     | TAMANHO | OBRIGATÓRIO | DESCRIÇÃO                                                                                                                                                                | ANTIFRAUDE REDSHIELD      |
|--------------------------|----------|---------|-------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------|
| Payment                  |          |         | Sim         | Nó contendo as informações de Pagamento                                                                                                                                  | Obrigatório               |
| Payment.Type             | Texto    | 100     | Sim         | Tipo do Meio de Pagamento (CreditCard / DebitCard / EletronicTransfer / Boleto)                                                                                          | -                         |
| Payment.Amount           | Número   | 15      | Sim         | Valor do pedido em centavos.                                                                                                                                             | Obrigatório               |
| Payment.Currency         | Texto    | 3       | Não         | Moeda na qual o pagamento será realizado. Valor padrão: BRL                                                                                                              | OpcionalValor Padrão: BRL |
| Payment.Country          | Texto    | 3       | Não         | Pais no qual o pagamento será feito. Valor padrão: BR                                                                                                                    | -                         |
| Payment.Provider         | Texto    | 15      | -           | Nome do Meio de Pagamento. Não obrigatório para Cartão de Crédito.                                                                                                       | -                         |
| Payment.ServiceTaxAmount | Número   | 15      | Não         | Exclusivo para companhias aéreas - Montante do valor da autorização que deve ser destinado à taxa de serviço. Obs.: Esse valor não é adicionado ao valor da autorização. | -                         |
| Payment.Installments     | Número   | 2       | Sim         | Número de Parcelas                                                                                                                                                       | -                         |
| Payment.Interest         | Texto    | 10      | Não         | Tipo de parcelamento - Loja (ByMerchant) ou Cartão (ByIssuer). Valor padrão: ByMerchant                                                                                  | -                         |
| Payment.SoftDescriptor   | Texto    | 13      | Não         | Texto que será impresso na fatura bancária do portador. Disponível apenas para VISA / MASTER. Não permite caracteres especiais.                                          | -                         |
| Payment.Capture          | Booleano | -       | Não         | Define se a autorização deve ser capturada automaticamente. Default: false                                                                                               | -                         |
| Payment.Authenticate     | Booleano | -       | Não         | Define se o comprador será direcionado ao Banco emissor para autenticação do cartão. Default: false                                                                      | -                         |

| CAMPOS                            | TIPO     | TAMANHO | OBRIGATÓRIO | DESCRIÇÃO                                                                         | ANTIFRAUDE REDSHIELD |
|-----------------------------------|----------|---------|-------------|-----------------------------------------------------------------------------------|----------------------|
| Payment.CreditCard                |          |         | Sim         | Nó contendo as informações do Cartão                                              | Obrigatório          |
| Payment.CreditCard.CardNumber     | Texto    | 16      | Sim         | Número do Cartão do Comprador.                                                    | Obrigatório          |
| Payment.CreditCard.Holder         | Texto    | 25      | Não         | Nome do Comprador impresso no cartão.                                             | Obrigatório          |
| Payment.CreditCard.ExpirationDate | Texto    | 7       | Sim         | Data de validade do cartão.                                                       | Obrigatório          |
| Payment.CreditCard.SecurityCode   | Texto    | 4       | Não         | Código de segurança impresso no verso do cartão.                                  | Obrigatório          |
| Payment.CreditCard.SaveCard       | Booleano | -       | Não         | Identifica se o cartão será salvo para gerar o CardToken. Valor padrão: false     | -                    |
| Payment.CreditCard.Brand          | Texto    | 10      | Sim         | Bandeira do cartão (Visa / Master / Amex / Elo / Aura / JCB / Diners / Discover). | Obrigatório          |

| CAMPOS                                      | TIPO     | TAMANHO | OBRIGATÓRIO | DESCRIÇÃO                                                                                                                                       | ANTIFRAUDE REDSHIELD   |
|---------------------------------------------|----------|---------|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------|------------------------|
| Payment.FraudAnalysis                       |          |         |             | Nó contendo as informações para Análise de Fraude                                                                                               | Obrigatório            |
| Payment.FraudAnalysis.Provider              | Texto    | 12      | Não         | Identifica o provedor da Solução de Análise de Fraude (CyberSource / RedShield). Valor padrão: CyberSource                                      | Obrigatório(RedShield) |
| Payment.FraudAnalysis.CaptureOnLowRisk      | Booleano | -       | Não         | Captura a transação se a análise de fraude retornar "Accept". Valor padrão: false.                                                              | Opcional               |
| Payment.FraudAnalysis.VoidOnHighRisk        | Booleano | -       | Não         | Cancela a transação se a análise de fraude retornar "Reject". Valor padrão: false.                                                              | Opcional               |
| Payment.TotalOrderAmount                    | Número   | 255     | Não         | Valor total do pedido em centavos. É utilizado pelo provedor Antifraude quando o comprador dividir o pagamento. Ex: Pagamento com dois cartões. | Obrigatório            |
| Payment.OrderDate                           | Date     | 255     | Não         | Data do pedido. Formato: YYYY-MM-DDTHH:MM:SSSZ                                                                                                  | Obrigatório            |
| Payment.FraudAnalysis.IsRetryTransaction    | Booleano | -       | Não         | Indica se é uma retentativa de autorização.                                                                                                     | Opcional               |
| Payment.FraudAnalysis.SplitingPaymentMethod | Texto    | -       | Não         | Indica se o pagamento esta sendo dividido. Ex: Pagamento com dois cartões.Valores possíveis: SINGLE-PAY e SPLIT-PAY                             | Opcional               |
| Payment.FraudAnalysis.FingerPrintId         | Texto    | 50      | Não         | Identificador utilizado para cruzar informações obtidas pelo Browser do internauta com os dados enviados para análise. Este mesmo valor deve ser passado na variável SESSIONID do script do DeviceFingerPrint. | - |
| Payment.FraudAnalysis.Sequence              | Texto    | -       | Não         | Define a ordem de execução da análise de fraude. Valor padrão: AnalyseFirstAnalyseFirst - efetua a análise de fraude antes de enviar para autorizaçãoAuthorizeFirst - efetua a atutorização antes de realizar a análise de fraude | Opcional |
| Payment.FraudAnalysis.SequenceCriteria | Texto | - | Não | Define, de acordo com a sequência, se prosseguirá ou não com o fluxo. Valor padrão: AlwaysAlways - Para o fluxo AnalyseFirst, segue com a autorização independente do resultado da análise de fraude. Para o fluxo AuthorizeFirst, segue com a análise de fraude independente do resultado da autorização.OnSuccess - Para o fluxo AnalyseFirst, segue com a autorização somente se a análise de fraude retornar "Accept". Para o fluxo AuthorizeFirst, segue com a análise de fraude somente se a transação for autorizada. | Opcional |

| CAMPOS                                           | TIPO     | TAMANHO | OBRIGATÓRIO | DESCRIÇÃO                                                                      | ANTIFRAUDE REDSHIELD                   |
|--------------------------------------------------|----------|---------|-------------|--------------------------------------------------------------------------------|----------------------------------------|
| Payment.FraudAnalysis.Browser                    |          |         |             | Nó contendo as informações do Browser para Análise de Fraude                   | Obrigatório                            |
| Payment.FraudAnalysis.Browser.CookiesAccepted    | Booleano | -       | Não         | Booleano que identifica se o browser do cliente aceita cookies. Default: false | -                                      |
| Payment.FraudAnalysis.Browser.Email              | Texto    | 255     | Não         | Email do comprador                                                             | -                                      |
| Payment.FraudAnalysis.Browser.HostName           | Texto    | 60      | Não         | Nome do host onde o comprador entrou no site da loja.                          | -                                      |
| Payment.FraudAnalysis.Browser.IpAddress          | Texto    | 15      | Não         | Endereço IP do comprador. É altamente recomendável o envio deste campo.        | Opcional                               |
| Payment.FraudAnalysis.Browser.Type               | Texto    | 40      | Não         | Nome do browser utilizado pelo comprador.                                      | -                                      |
| Payment.FraudAnalysis.Browser.BrowserFingerPrint | Texto    | 6010    | Não         | Impressão digital de dispositivos e geolocalização real do IP do comprador.    | ObrigatórioConfiguração do Fingerprint |

| CAMPOS                        | TIPO     | TAMANHO | OBRIGATÓRIO | DESCRIÇÃO                                                           | ANTIFRAUDE REDSHIELD |
|-------------------------------|----------|---------|-------------|---------------------------------------------------------------------|----------------------|
| Payment.FraudAnalysis.Cart    |          |         |             | Nó contendo as informações do Carrinho para Análise de Fraude       | Opcional             |
| FraudAnalysis.IsGift          | Booleano | -       | Não         | Indica se o pedido é para presente ou não. Valor padrão: false      | -                    |
| FraudAnalysis.ReturnsAccepted | Booleano | -       | Não         | Indica se devoluções são aceitas para o pedido. Valor padrão: false | -                    |

| CAMPOS                                           | TIPO     | TAMANHO | OBRIGATÓRIO | DESCRIÇÃO                                                                      | ANTIFRAUDE REDSHIELD                   |
|--------------------------------------------------|----------|---------|-------------|--------------------------------------------------------------------------------|----------------------------------------|
| Payment.FraudAnalysis.Cart.Items[]               |  |  |  | Nó contendo as informações dos Itens do Carrinho para Análise de Fraude | Opcional |
| Payment.FraudAnalysis.Cart.Items[].Type          | Texto | - | Não | Tipo do Produto.<BR> AdultContent -  Conteúdo adulto. <BR>Coupon - Cupon de desconto. <BR>Default - Opção padrão para análise. <BR>EletronicGood - Produto eletrônico. <BR>EletronicSoftware - Softwares distribuídos eletronicamente via download. <BR>GiftCertificate - Vale presente. <BR>HandlingOnly - Taxa de instalação ou manuseio. <BR>Service - Serviço.ShippingAndHandling - Frete e taxa de instalação ou manuseio. <BR>ShippingOnly - Frete.Subscription - Assinatura. | - |
| Payment.FraudAnalysis.Cart.Items[].Name | Texto | 255 | Não | Nome do produto. | Opcional |
| Payment.FraudAnalysis.Cart.Items[].Risk | Texto | - | Não | Nível de risco do produto.LOW - O produto tem um histórico de poucos chargebacks. <br>**Normal** - O produto tem um histórico de chargebacks considerado normal. <br>**high** - O produto tem um histórico de chargebacks acima da média. | Opcional |
| Payment.FraudAnalysis.Cart.Items[].Sku | Texto | 255 | Não | Sku do produto. | Opcional |
| Payment.FraudAnalysis.Cart.Items[].MerchantItemId | Texto | 255 | Não | ID do produto na loja. | Opcional |
| Payment.FraudAnalysis.Cart.Items[].UnitPrice | Número | - | Não | Preço unitário do produto em centavos | Opcional |
| Payment.FraudAnalysis.Cart.Items[].OriginalPrice | Número | - | Não | Preço original do produto em centavos | Opcional |
| Payment.FraudAnalysis.Cart.Items[].Quantity | Número | - | Não | Quantidade do produto. | Opcional |
| Payment.FraudAnalysis.Cart.Items[].HostHedge | Texto | - | Não | Nível de importância do e-mail e endereços IP dos clientes em risco de pontuação. <br><br>**Valor Padrão:** **off**<br><br><BR> **low** - Baixa importância do e-mail e endereço IP na análise de risco. <br>**Normal** - Média importância do e-mail e endereço IP na análise de risco. <br>**high** - Alta importância do e-mail e endereço IP na análise de risco.  <br>**off** - E-mail e endereço IP não afetam a análise de risco. | - |
| Payment.FraudAnalysis.Cart.Items[].NonSensicalHedge | Texto | - | Não | Nível dos testes realizados sobre os dados do comprador com pedidos recebidos sem sentido. <br><br>**Valor Padrão:** **off**<br><br><BR> **low** - Baixa importância da verificação feita sobre o pedido do comprador, na análise de risco. <br>**Normal** - Média importância da verificação feita sobre o pedido do comprador, na análise de risco. <br>**high** - Alta importância da verificação feita sobre o pedido do comprador, na análise de risco. <br>**off** - Verificação do pedido do comprador não afeta a análise de risco. | - |
| Payment.FraudAnalysis.Cart.Items[].ObscenitiesHedge | Texto | - | Não | Nível de obscenidade dos pedidos recebedidos. <br><br><br>**Valor Padrão:** **off**<br><br><br><BR> **low** - Baixa importância da verificação sobre obscenidades do pedido do comprador, na análise de risco.Normal - Média importância da verificação sobre obscenidades do pedido do comprador, na análise de risco. <br>**high** - Alta importância da verificação sobre obscenidades do pedido do comprador, na análise de risco. <br>**off** - Verificação de obscenidade no pedido do comprador não afeta a análise de risco. | - |
| Payment.FraudAnalysis.Cart.Items[].PhoneHedge | Texto | - | Não | Nível dos testes realizados com os números de telefones. <br><br>**Valor Padrão:** **off**<br><br><BR> **low** - Baixa importância nos testes realizados com números de telefone.Normal - Média importância nos testes realizados com números de telefone. <br>**high** - Alta importância nos testes realizados com números de telefone.  <br>**off** - Testes de números de telefone não afetam a análise de risco. | - |
| Payment.FraudAnalysis.Cart.Items[].TimeHedge | Texto | - | Não | Nível de importância da hora do dia do pedido do cliente. <br><br>**Valor Padrão:** **off**<br><br><BR> **low** - Baixa importância no horário do dia em que foi feita a compra, para a análise de risco. <br>**Normal** - Média importância no horário do dia em que foi feita a compra, para a análise de risco.High - Alta importância no horário do dia em que foi feita a compra, para a análise de risco.  <br>**off** - O horário da compra não afeta a análise de risco. | - |
| Payment.FraudAnalysis.Cart.Items[].VelocityHedge | Texto | - | Não | Nível de importância de frequência de compra do cliente. <br><br>**Valor Padrão:** **off**<br><br><BR> **low** - Baixa importância no número de compras realizadas pelo cliente nos últimos 15 minutos.Normal - Média importância no número de compras realizadas pelo cliente nos últimos 15 minutos. <br>**high** - Alta importância no número de compras realizadas pelo cliente nos últimos 15 minutos. <br>**off** - A frequência de compras realizadas pelo cliente não afeta a análise de fraude. | - |
| Payment.FraudAnalysis.Cart.Items[].GiftCategory | Texto | - | Não | Flag que avaliará os endereços de cobrança e entrega para difrentes cidades, estados ou países. <br><br>**Valor Padrão:** **off**<br><br> <br>**YES** - Em caso de divergência entre endereços de cobrança e entrega, marca com riscopequeno.  <br>**No** - Em caso de divergência entre endereços de cobrança e entrega, marca com riscoalto.  <br>**off** - Ignora a análise de risco para endereços divergentes. | - |
| Payment.FraudAnalysis.Cart.Items[].GiftMessage | Texto | 50 | Não | Mensagem de presente. | Opcional |
| Payment.FraudAnalysis.Cart.Items[].Description | Texto | 255 | Não | Descrição do produto. | Opcional |
| Payment.FraudAnalysis.Cart.Items[].ShippingInstructions | Texto | 255 | Não | Instruções para entrega do item do carrinho. | Opcional |
| Payment.FraudAnalysis.Cart.Items[].ShippingMethod | Texto | - | Não | Método de entrega do item do carrinho.RedShield: None | SameDay | NextDay | TwoDay | ThreeDay | LowCost | CarrierDesignatedByCustomer | Pickup | International | Military | OtherCyberSource: None | SameDay | OneDay | TwoDay | ThreeDay | LowCost | Pickup | Other | Opcional |
| Payment.FraudAnalysis.Cart.Items[].ShippingTrackingNumber | Texto | 50 | Não | Número de rastreamento da entrega do item do carrinho. | Opcional |

| CAMPOS                                                | TIPO  | TAMANHO | OBRIGATÓRIO | DESCRIÇÃO                                                       | ANTIFRAUDE REDSHIELD     |
|-------------------------------------------------------|-------|---------|-------------|-----------------------------------------------------------------|--------------------------|
| Payment.FraudAnalysis.Cart.Items[].Passenger          |       |         |             | Nó contendo as informações do Passageiro para Análise de Fraude |                          |
| Payment.FraudAnalysis.Cart.Items[].PassengerName      | Texto | 120     | Não         | Nome do passageiro.                                             | OpcionalNome e Sobrenome |
| Payment.FraudAnalysis.Cart.Items[].Passenger.Identity | Texto | 50      | Não         | Identificação do passageiro para quem o bilhete foi emitido     | Opcional                 |
| Payment.FraudAnalysis.Cart.Items[].Passenger.Status   | Texto | 32      | Não         | Classificação da empresa aérea (Gold                            | Platinum)                |
| Payment.FraudAnalysis.Cart.Items[].Passenger.Email    | Texto | 100     | Não         | Email do Passageiro.                                            | Opcional                 |
| Payment.FraudAnalysis.Cart.Items[].Passenger.Phone    | Texto | 15      | Não         | Número do telefone do passageiro. Incluir o código do país.     | Opcional                 |
| Payment.FraudAnalysis.Cart.Items[].Passenger.Rating   | Texto |         | Não         | Classificação do Passageiro. <br>Adult - Passageiro adulto. <br>Child - Passageiro criança.<br>Infant - Passageiro infantil.<br>Youth - Passageiro adolescente.<br>Student - Passageiro estudante.<br>SeniorCitizen  - Passageiro idoso.<br>Military  - Passageiro militar. | Opcional |

| CAMPOS                                              | TIPO   | TAMANHO | OBRIGATÓRIO | DESCRIÇÃO                                                | ANTIFRAUDE REDSHIELD |
|-----------------------------------------------------|--------|---------|-------------|----------------------------------------------------------|----------------------|
| Payment.FraudAnalysis.MerchantDefinedFields[]       |        |         |             | Nó contendo dados adicionais para Análise de Fraude      | Opcional             |
| Payment.FraudAnalysis.MerchantDefinedFields[].Id    | Número | -       | Não         | Identificador de uma informação adicional a ser enviada. | Opcional             |
| Payment.FraudAnalysis.MerchantDefinedFields[].Value | Texto  | 255     | Não         | Valor de uma informação adicional a ser enviada.         | Opcional             |

| CAMPOS                                                    | TIPO   | TAMANHO | OBRIGATÓRIO | DESCRIÇÃO                                                           | ANTIFRAUDE REDSHIELD |
|-----------------------------------------------------------|--------|---------|-------------|---------------------------------------------------------------------|----------------------|
| Payment.FraudAnalysis.CustomConfiguration                 |        |         |             | Nó contendo as configurações personalizadas para Análise de Fraude. | Opcional             |
| Payment.FraudAnalysis.CustomConfiguration.RiskAmount      | Número | -       | Não         | Valor de risco do pedido em centavos                                | Opcional             |
| Payment.FraudAnalysis.CustomConfiguration.MerchantWebsite | Texto  | 100     | Não         | Endereço Web do site da loja                                        | Opcional             |

| CAMPOS                             | TIPO   | TAMANHO | OBRIGATÓRIO | DESCRIÇÃO                                                                                                                                                  | ANTIFRAUDE REDSHIELD |
|------------------------------------|--------|---------|-------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------|
| Payment.FraudAnalysis.Travel       |        |         |             | Nó contendo as informações da viagem                                                                                                                       | Opcional             |
| Payment.FraudAnalysis.Travel       | Texto  | 255     | Não         | Rota completa da viagem. Concatenação das pernas (código do aeroporto) de origem e destino da viagem no formato ORIG1- DEST1. Ex.: SFO-JFK:JFK-LHR:LHR-CDG | Opcional             |
| Payment.FraudAnalysis.DepartueTime | Texto  | 100     | Não         | Data da partida                                                                                                                                            | Opcional             |
| Payment.FraudAnalysis.JourneyType  | Texto  | 100     | Não         | Tipo de viagem.                                                                                                                                            | Opcional             |
| Payment.FraudAnalysis.Origin       | Texto  | 5       | Não         | Código do aeroporto de origem da viagem.                                                                                                                   | Opcional             |
| Payment.FraudAnalysis.Destination  | string | 5       | Não         | Código do aeroporto de destino da viagem.                                                                                                                  | Opcional             |

| CAMPOS                              | TIPO  | TAMANHO | OBRIGATÓRIO | DESCRIÇÃO                                         | ANTIFRAUDE REDSHIELD |
|-------------------------------------|-------|---------|-------------|---------------------------------------------------|----------------------|
| Payment.FraudAnalysis.Travel.Legs[] |       |         |             | Nó contendo as informações da rota da viagem      | Opcional             |
| Payment.FraudAnalysis.Origin        | Texto | 3       | Não         | Código do aeroporto do ponto de destino da viagem | Opcional             |
| Payment.FraudAnalysis.Destination   | Texto | 3       | Não         | Código do aeroporto do ponto de origem da viagem. | Opcional             |

| CAMPOS                                   | TIPO  | TAMANHO | OBRIGATÓRIO | DESCRIÇÃO                                                            | ANTIFRAUDE REDSHIELD     |
|------------------------------------------|-------|---------|-------------|----------------------------------------------------------------------|--------------------------|
| Payment.FraudAnalysis.Shipping           |       |         |             | Nó contendo informações adicionais da entrega para Análise de Fraude | Opcional                 |
| Payment.FraudAnalysis.Shipping.Addressee | Texto | 255     | Não         | Nome do destinatário.                                                | OpcionalNome e Sobrenome |
| Payment.FraudAnalysis.Shipping.Email     | Texto | 255     | Não         | Email do destinatário.                                               | Opcional                 |
| Payment.FraudAnalysis.Shipping.Phone     | Texto | 15      | Não         | Telefone do destinatário.                                            | Opcional                 |
| Payment.FraudAnalysis.Shipping.WorkPhone | Texto | 15      | Não         | Telefone de trabalho do destinatário.                                | Opcional                 |
| Payment.FraudAnalysis.Shipping.Mobile    | Texto | 15      | Não         | Celular do destinatário.                                             | Opcional                 |
| Payment.FraudAnalysis.Shipping.Comment   | Texto | 100     | Não         | Referência do endereço de entrega do destinatário.                   | Opcional                 |
| Payment.FraudAnalysis.ShippingMethod     | Texto |         | Não         | Método de entrega do pedido.*RedShield*:<br> None <br> SameDay <br> NextDay <br> TwoDay <br> ThreeDay <br> LowCost <br> CarrierDesignatedByCustomer <br> Pickup <br> International <br> Military <br> <br><br> OtherCyberSource:<br> None <br> SameDay <br> OneDay <br> TwoDay <br> ThreeDay <br> LowCost <br> Pickup <br> Other <br> Opcional <br>||

### Resposta

```shell

--header "Content-Type: application/json"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
   "MerchantOrderId":"2014111457673454307",
   "Customer":{
      "Name":"Comprador Teste",
      "Identity":"13987441747",
      "IdentityType":"CPF",
      "MerchantCustomerId":"1325374899",
      "Email":"compradorteste@live.com",
      "Birthdate":"1991-01-02",
      "Phone":"21114740",
      "WorkPhone":"21114740",
      "Mobile":"21114740",
      "Address":{
         "Street":"Rua Júpter",
         "Number":"174",
         "Complement":"AP 201",
         "District":"Centro",
         "ZipCode":"21241140",
         "City":"Rio de Janeiro",
         "State":"RJ",
         "Country":"BRASIL"
      },
      "DeliveryAddress":{
         "Street":"Rua Júpter",
         "Number":"174",
         "Complement":"AP 201",
         "District":"Centro",
         "ZipCode":"21241140",
         "City":"Rio de Janeiro",
         "State":"RJ",
         "Country":"BR"
      },
      "BillingAddress":{
         "Street":"Rua Júpter",
         "Number":"174",
         "Complement":"AP 201",
         "ZipCode":"21241140",
         "City":"Rio de Janeiro",
         "State":"RJ",
         "Country":"BR",
         "Neighborhood":"Centro"
      },
      "Status":"NEW"
   },
   "Payment":{
      "Type":"CreditCard",
      "Amount":100,
      "Currency":"BRL",
      "Country":"BRA",
      "Provider":"Simulado",
      "ServiceTaxAmount":0,
      "Installments":1,
      "SoftDescriptor":"123456789ABCD",
      "Interest":"ByMerchant",
      "Capture":false,
      "Authenticate":false,
      "CreditCard":{
         "CardNumber":"6011000000000111",
         "Holder":"Teste accept",
         "ExpirationDate":"12/2019",
         "SecurityCode":"023",
         "Brand":"Discover",
         "EciThreeDSecure":"05"
      },
      "FraudAnalysis":{
         "Id": "236bd8b5-8e15-e711-93fe-000d3ac03bed",
         "Status": 2,
         "StatusDescription": "Reject",
         "ReplyData": {
             "FactorCode": "100.400.147",
             "ReturnMessage": "Payment void and transaction denied by ReD Shield",
             "ProviderTransactionId": "958567039650"
         }
         "Sequence":"AnalyseFirst",
         "SequenceCriteria":"Always",
         "FingerPrintId":"074c1ee676ed4998ab66491013c565e2",
         "Provider":"RedShield",
         "CaptureOnLowRisk":true,
         "VoidOnHighRisk":false,
         "TotalOrderAmount":"150000",
         "OrderDate":"2016-12-09",
         "IsRetryTransaction":false,
         "SplitingPaymentMethod":"SINGLE-PAY",
         "Browser":{
            "CookiesAccepted":false,
            "Email":"compradorteste@live.com",
            "HostName":"Teste",
            "IpAddress":"187.32.163.105",
            "Type":"Chrome",
            "BrowserFingerPrint":"04003hQUMXGB0poNf94lis1ztuLYRFk+zJ17aP79a9O8mWOBmEnKs6ziAo94ggAtBvKEN6/FI8Vv2QMAyHLnc295s0Nn8akZzRJtHwsEilYx1P+NzuNQnyK6+7x2OpjJZkl4NlfPt7h9d96X/miNlYT65UIY2PeH7sUAh9vKxMn1nlPu2MJCSi12NBBoiZbfxP1Whlz5wlRFwWJi0FRulruXQQGCQaJkXU7GWWZGI8Ypycnf7F299GIR12G/cdkIMFbm6Yf0/pTJUUz1vNp0X2Zw8QydKgnOIDKXq4HnEqNOos1c6njJgQh/4vXJiqy0MXMQOThNipDmXv9I185O+yC2f3lLEO0Tay66NZEyiLNePemJKSIdwO9O5ZtntuUkG6NTqARuHStXXfwp8cyGF4MPWLuvNvEfRkJupBy3Z8hSEMEK7ZWd2T2HOihQxRh4qp+NANqYKBTl3v6fQJAEKikeSQVeBN8sQqAL0BZFaIMzbrnMivi6m6JRQUIdvEt+MbJEPFc0LjRycC5ApUmJO+Aoo9VKL1B8ftMSQ1iq1uTKn16ZOmDpzZrZhMPbH83aV0rfB2GDXcjpghm9klVFOw7EoYzV7IDBIIRtgqG9KZ+8NH/z6D+YNUMLEUuK1N2ddqKbS5cKs2hplVRjwSv7x8lMXWE7VDaOZWB8+sD1cMLQtEUC0znzxZ4bpRaiSy4dJLxuJpQYAFUrDlfSKRv/eHV3QiboXLuw9Lm6xVBK8ZvpD5d5olGQdc+NgsqjFnAHZUE+OENgY4kVU9wB84+POrI4MkoD4iHJ5a1QF8AZkZDFo1m1h9Bl+J2Ohr6MkBZq8DG5iVaunHfxUdHou5GL7lS1H7r+8ctfDXi8AfOPjzqyODJQ74Aiel35TKTOWG8pq1WO6yzJ1GNmMuMWZBamlGXoG/imnjwHY9HQtQzpGfcm0cR8X2Fd1ngNFGLDGZlWOX0jWtOwU6XVGT37JFD9W/cx4kzI+mPNi65X5WFPYlDG9N0Lbh5nOj3u3DXqRCiKCUrsEkMt8z9fxO9pLLGVQUKIYR2wTw53CiWK96FOpPevDWtH2XR0QkfOd02D73n81x6hEMCy0s3hRLn08Th9FlNHDMJBqLj+Tz8rG2TtNki3mJC7Ass1MT2qnKBI77n6vsQkAp59TfbZm/tBXwAoYdLJXge8F/numhd5AvQ+6I8ZHGJfdN3qWndvJ2I7s5Aeuzb8t9//eNsm73fIa05XreFsNyfOq1vG2COftC6EEsoJWe5h5Nwu1x6PIKuCaWxLY+npfWgM0dwJPmSgPx7TNM31LyVNS65m83pQ+qMTRH6GRVfg7HAcS5fnS/cjdbgHxEkRmgkRq1Qs48sbX9QC8nOTD0ntb6FcJyEOEOVzmJtDqimkzDq+SXR1/63AYe4LEj+ogRgN+Z8HAFhGFzd/m6snVviELfRqJ4LLQIk9Y/fzqnsF6I5OGxfdT2sxxK2Vokpi3jWhCcEknw7dYlHYpOnCHZO7QVgjQTngF2mzKf4GeOF4ECFsWTgLy6HFEitfauYJt1Xh1NfZZerBMwXLFzdhzoTQxGlcXc8lZIoEG1BLYv/ScICf8Ft9PEtpEa+j0cDSlU99UoH2xknwR1W9MRGc5I/euE63/IMJTqguZ3YcnJpjSVnAGSpyz/0gKjypJ3L86rHFRGXt0QbmaXtSl2UmmjI0p0LCCdx7McatCFEVI6FwPpPV0ZSMv/jM75eBid1X/lTV4XNzjowzR/iFlKYMzHZtVO9hCBPKlTwblRXNn4MlvNm/XeSRQ+Mr0YV5w5CL5Z/tGyzqnaLPj/kOVdyfj8r2m5Bcrz4g/ieUIo8qRFv2T2mET46ydqaxi27G4ZYHj7hbiaIqTOxWaE07qMCkJw=="
         },
         "Cart":{
            "IsGift":false,
            "ReturnsAccepted":true,
            "Items":[
               {
                  "Type":"AdultContent",
                  "Name":"ItemTeste",
                  "Risk":"High",
                  "Sku":"201411170235134521346",
                  "MerchantItemId":"1234",
                  "UnitPrice":123,
                  "OriginalPrice":234,
                  "Quantity":1,
                  "HostHedge":"Off",
                  "NonSensicalHedge":"Off",
                  "ObscenitiesHedge":"Off",
                  "PhoneHedge":"Off",
                  "TimeHedge":"Normal",
                  "VelocityHedge":"High",
                  "GiftCategory":"Undefined",
                  "GiftMessage":"Te amo!",
                  "Description":"Uma description do Mouse",
                  "ShippingInstructions":"Proximo ao 546",
                  "ShippingMethod":"SameDay",
                  "ShippingTrackingNumber":"123456",
                  "Passenger":{
                     "Name":"Comprador accept",
                     "Identity":"1234567890",
                     "Status":"Accepted",
                     "Rating":"Adult",
                     "Email":"compradorteste@live.com",
                     "Phone":"999994444"
                  }
               },
               {
                  "Type":"AdultContent",
                  "Name":"ItemTeste",
                  "MerchantItemId":"4",
                  "Risk":"High",
                  "Sku":"201411170235134521346",
                  "UnitPrice":123,
                  "OriginalPrice":12000,
                  "Quantity":1,
                  "HostHedge":"Off",
                  "NonSensicalHedge":"Off",
                  "ObscenitiesHedge":"Off",
                  "PhoneHedge":"Off",
                  "TimeHedge":"Normal",
                  "VelocityHedge":"High",
                  "GiftCategory":"Undefined",
                  "GiftMessage":"Te amo!",
                  "Description":"Uma description do Mouse",
                  "ShippingInstructions":"Proximo ao 546",
                  "ShippingMethod":"SameDay",
                  "ShippingTrackingNumber":"123456",
                  "Passenger":{
                     "Name":"Comprador accept",
                     "Identity":"1234567890",
                     "Status":"Accepted",
                     "Rating":"Adult",
                     "Email":"compradorteste@live.com",
                     "Phone":"999994444"
                  }
               }
            ]
         },
         "MerchantDefinedFields":[
            {
               "Id":95,
               "Value":"Eu defini isso"
            },
            {
               "Id":96,
               "Value":"Outra informação"
            }
         ],
         "Shipping":{
            "Addressee":"Sr Comprador Teste",
            "Email":"ffigueiredo@braspag.com.br",
            "Method":"LowCost",
            "Phone":"21114740",
            "WorkPhone":"123456789-78945612",
            "Mobile":"987456-123456",
            "Comment":"Em frente ao 322."
         },
         "CustomConfiguration":{
            "ServiceId":"0",
            "RiskAmount":0,
            "RiskBrand":"0",
            "MerchantWebsite":"www.test.com",
            "AccountTokenS":"acc123"
         },
         "Travel":{
            "Route":"GIG-CGH-EZE",
            "DepartueTime":"2016-12-10",
            "JouneyType":"JT",
            "Legs":[
               {
                  "Origin":"GIG",
                  "Destination":"CGH"
               },
               {
                  "Origin":"CGH",
                  "Destination":"EZE"
               }
            ]
         }
      }
   }
}

```

> **Payment.FraudAnalysis** - Nó contendo as informações da Análise de Fraude

| CAMPOS                                                | TIPO   | TAMANHO | DESCRIÇÃO                                                                                                                                           |
|-------------------------------------------------------|--------|---------|-----------------------------------------------------------------------------------------------------------------------------------------------------|
| Payment.FraudAnalysis.Id                              | Texto  | 36      | Id da Transação no Antifraude.                                                                                                                      |
| Payment.FraudAnalysis.Status                          | Número |         | "Status da Análise de Fraude <br> 0 - Unknown<br> 1 - Accept  <br> 2 - Reject <br> 3 - Review<br> 4 - Aborted <br> 5 - Error                        |
| Payment.FraudAnalysis.StatusDescription               | Texto  |         | "Descrição do status da Análise de Fraude.    <br> 0 - Unknown   <br> 1 - Accept<br> 2 - Reject <br> 3 - Review  <br> 4 - Aborted    <br> 5 - Error |

> **Payment.FraudAnalysis.ReplyData** - Nó contendo as informações detalhadas da Análise de Fraude

| CAMPOS                                                | TIPO   | TAMANHO | DESCRIÇÃO                                                                                                                                           |
|-------------------------------------------------------|--------|---------|-----------------------------------------------------------------------------------------------------------------------------------------------------|
| Payment.FraudAnalysis.ReplyData.FactorCode            | Texto  | 255     | Código retornado do provider indicando o faotres que influenciaram na Análise de Fraude.                                                            |
| Payment.FraudAnalysis.ReplyData.ReturnMessage         | Texto  | 255     | Descrição do código retornado pelo provider                                                                                                         |
| Payment.FraudAnalysis.ReplyData.ProviderTransactionId | Texto  | 255     | Identificador da transação no provedor Antifraude.                                                                                                  |

