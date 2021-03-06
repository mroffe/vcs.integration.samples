# Enviar Pedido e Autorizar Despacho

Este documento tem por objetivo auxiliar o Seller não VTEX a receber um pedido, e receber a autorização para despachar o pedido


*Exemplo do fuxo de chamadas de descida, autorização para despachar e cancelamento de pedido:*  

![alt text](pedido-seller-nao-vtex.png "Fluxo de pedido") 

##1 - Enviar Pedido##
Quando o pedido é fechado no ambiente VTEX, um POST é feito no Seller não VTEX, para que este possa receber a ordem de pedido.  
 
###Exemplos de Request de Descida de Pedido - Endpoint do Seller###

endpoint: **https://sellerendpoint/pvt/orders?sc=[idcanal]**  
verb: **POST**  
Content-Type: **application/json**  
Accept: **application/json**  
Parametro: **sc=5** // sc serve para destacar o canal por onde o pedido entrou


*Exemplo do Request:*  

	  {
	    "marketplaceOrderId": "959311095",
	    "marketplaceServicesEndpoint": "https://urlmarketplace/", //leia o tópico Invocando MarketplaceServicesEndpoint Actions
	    "marketplacePaymentValue": 11080,
	    "items": [
	      {
	        "id": "2002495",
	        "quantity": 1,
	        "seller": "1",
	        "commission": 0,
	        "freightCommission": 0,
	        "price": 9990,
	        "bundleItems": [], //serviços. Ex: embalagem pra presente.
	        "itemAttachment": { 
	          "name": null,
	          "content": {}
	        },
	        "attachments": [], //customização do item, Ex:camisa com o numero 10
	        "priceTags": [],
	        "measurementUnit": null, unidade de medida
	        "unitMultiplier": 0, unidade multipladora,Ex: venda por quilo
	        "isGift": false
	      }
	    ],
	    "clientProfileData": {
	      "id": "clientProfileData",
	      "email": "32172239852@gmail.com.br",
	      "firstName": "Jonas",
	      "lastName": "Alves de Oliveira",
	      "documentType": null,
	      "document": "3244239851",
	      "phone": "399271258",
	      "corporateName": null,
	      "tradeName": null,
	      "corporateDocument": null,
	      "stateInscription": null,
	      "corporatePhone": null,
	      "isCorporate": false,
	      "userProfileId": null
	    },
	    "shippingData": {
	      "id": "shippingData",
	      "address": {
	        "addressType": "Residencial",
	        "receiverName": "Jonas Alves de Oliveira",
	        "addressId": "Casa",
	        "postalCode": "13476103",
	        "city": "Americana",
	        "state": "SP",
	        "country": "BRA",
	        "street": "JOÃO DAMÁZIO GOMES",
	        "number": "311",
	        "neighborhood": "SÃO JOSÉ",
	        "complement": null,
	        "reference": "Bairro Praia Azul / Posto de Saúde 17",
	        "geoCoordinates": []
	      },
	      "logisticsInfo": [
	        {
	          "itemIndex": 0,
	          "selectedSla": "Normal",
	          "lockTTL": "8d",
	          "shippingEstimate": "7d",
	          "price": 1090,
	          "deliveryWindow": null
	        }
	      ]
	    },
	    "openTextField": null,
	    "marketingData": null,
	    "paymentData":null
	  }

*Exemplo do Response:*

	  {
	    "marketplaceOrderId": "959311095",
	    "orderId": "123543123",
	    "followUpEmail": "75c70c09dbb3498c9b3bbdee59bf0687@ct.vtex.com.br",
	    "items": [
	      {
	        "id": "2002495",
	        "quantity": 1,
	        "seller": "1",
	        "commission": 0,
	        "freightCommission": 0,
	        "price": 9990,
	        "bundleItems": [],
	        "priceTags": [],
	        "measurementUnit": "un",
	        "unitMultiplier": 1,
	        "isGift": false
	      }
	    ],
	    "clientProfileData": {
	      "id": "clientProfileData",
	      "email": "5c77abaea35f4cb6824b9326942c00e5@ct.vtex.com.br",
	      "firstName": "JONAS",
	      "lastName": "ALVES DE OLIVEIRA",
	      "documentType": "cpf",
	      "document": "32133239851",
	      "phone": "1592712979",
	      "corporateName": null,
	      "tradeName": null,
	      "corporateDocument": null,
	      "stateInscription": null,
	      "corporatePhone": null,
	      "isCorporate": false,
	      "userProfileId": null
	    },
	    "shippingData": {
	      "id": "shippingData",
	      "address": {
	        "addressType": "Residencial",
	        "receiverName": "JONAS ALVES DE OLIVEIRA",
	        "addressId": "Casa",
	        "postalCode": "13476103",
	        "city": "Americana",
	        "state": "SP",
	        "country": "BRA",
	        "street": "JOÃO DAMÁZIO GOMES",
	        "number": "121",
	        "neighborhood": "SÃO JOSÉ",
	        "complement": null,
	        "reference": "Bairro Praia Azul / Posto de Saúde 17",
	        "geoCoordinates": []
	      },
	      "logisticsInfo": [
	        {
	          "itemIndex": 0,
	          "selectedSla": "Normal",
	          "lockTTL": "8d",
	          "shippingEstimate": "5d",
	          "price": 1090,
	          "deliveryWindow": null
	        }
	      ]
	    },
	   "paymentData":null
	  }

*Exemplo do Retorno de Erro:*

	{
		"error": {
		"code": "1",
		"message": "O verbo 'GET' não é compatível com a rota '/api/fulfillment/pvt/orders'",
		"exception": null
		}
	}


##2 - Enviar Autorização Para Despachar##
Quando o pagamento do pedido é concluído no Seller (pagamento válido), um POST deverá ser feito na "callbackUrl" do pagamento, informando sucesso do pagamento ("status":"approved"), nesse momento a loja VTEX envia autorização para despachar o respectivo pedido no Seller.  
 
*Exemplos de Request de Autorização - Endpoint da Seller*

endpoint: **https://sellerendpoint/pvt/orders/[orderid]/fulfill?sc=[idcanal]**  
verb: **POST**  
Content-Type: **application/json**  
Accept: **application/json**  
Parametro: **sc=5** // sc é o canal de vendas cadastrado no marketplace, serve para destacar o canal por onde o pedido entrou.  

*Exemplo do Request:*  

	{
		"marketplaceOrderId": "959311095" //id do pedido originado no canal de vendas
	}

*Exemplo do Response:*

	{
		"date": "2014-10-06 18:52:00",
		"marketplaceOrderId": "959311095",
		"orderId": "123543123",
		"receipt": "e39d05f9-0c54-4469-a626-8bb5cff169f8",
	}
  

##3 Invocando Marketplace Services Endpoint Actions##
O MarketplaceServicesEndpoint serve para receber informações do seller referentes a nota fiscal e tracking de pedido. É permitido o envio de notas fiscais parciais, obrigando assim ao informador informar além dos valores da nota fiscal,os items ele está mandando na nota fiscal parcial. O pedido na VTEX só andará pra o status FATURADO quando o valor total de todas as notas fiscais de um pedido forem enviadas.

###Exemplos de Request Para Informar Nota Fiscal - Endpoint VTEX###

endpoint: **https://marketplaceServicesEndpoint/pub/orders/[orderId]/invoice**  
verb: **POST**  
Content-Type: **application/json**  
Accept: **application/json**  


*Exemplo do Request:*  

	{
	    "type": "Output", //hard code
	    "invoiceNumber": "NFe-00001", //numero da nota fiscal
	    "courier": "", //quando é nota fiscal, dados de tracking vem vazio
	    "trackingNumber": "", //quando é nota fiscal, dados de tracking vem vazio
	    "trackingUrl": "",//quando é nota fiscal, dados de tracking vem vazio
	    "items": [ //itens da nota
	      {
	        "id": "345117",
	        "quantity": 1,
	        "price": 9003
	      }
	    ],
	    "issuanceDate": "2013-11-21T00:00:00", //data da nota
	    "invoiceValue": 9508 //valor da nota
	  }

*Exemplo do Response:*

	{
	    "date": "2014-02-07T15:22:56.7612218-02:00", //data do recibo
	    "orderId": "123543123",
	    "receipt": "38e0e47da2934847b489216d208cfd91" //protocolo gerado, pode ser nulo
  	}


###3.2 - Exemplos de Request Para Informar Tracking - Endpoint VTEX###

endpoint: **https://marketplaceServicesEndpoint/pub/orders/[orderId]/invoice**  
verb: **POST**  
Content-Type: **application/json**  
Accept: **application/json**  


*Exemplo do Request:*  

	{
	    "type": "Output",
	    "invoiceNumber": "NFe-00001",
	    "courier": "Correios", //transportadora
	    "trackingNumber": "SR000987654321", /tracking number
	    "trackingUrl": "http://traking.correios.com.br/sedex/SR000987654321", url de tracking
	    "items": [
	      {
	        "id": "345117",
	        "quantity": 1,
	        "price": 9003
	      }
	    ],
	    "issuanceDate": "2013-11-21T00:00:00",
	    "invoiceValue": 9508
	  }

*Exemplo do Response:*

	{
	    "date": "2014-02-07T15:22:56.7612218-02:00", //data do recibo
	    "orderId": "123543123",
	    "receipt": "38e0e47da2934847b489216d208cfd91" //protocolo gerado, pode ser nulo
  	}

**A Nota Fiscal e o Tracking podem ser enviados na mesma chamada, basta prenncher todos os dados do POST.


###4 - Enviar Solicitação de Cancelamento###
Uma solicitação de cancelamento pode ser enviada, caso o pedido se encontre em um estado que se possa cancelar, o pedido será cancelado. 
 
*Exemplos de Request de Cancelamento - Endpoint VTEX*

endpoint: **https://marketplaceServicesEndpoint/pvt/orders/[orderid]/cancel**  
verb: **GET**

##5 - Considerações##

####3.1 Header nas Chamadas a API REST da VTEX####
Todas chamadas as API REST devem conter no Headear as seguintes Keys:  
X-VTEX-API-AppToken:**[Value]**  
X-VTEX-API-AppKey:**[Value]**  
Content-Type: **application/json**      
Accept: **application/json**  

*O integrador deverá solitar junto ao contato VTEX a sua chave e token para uso exclusivo na integração,
assim como solicitar a criação do Seller dentro da loja VTEX. 

##5 Versão:Beta 1.1##
Essa versão de documentação suporta a integração na versão da plataforma VTEX smartcheckout. Ela foi escrita para auxiliar um integração e a idéia e que através dela, não  restem nenhuma dúvida de como se integrar com a VTEX. Se recebeu essa documentação e ainda restaram dúvidas, por favor, detalhe as suas dúvidas abaixo no comentário, para chegarmos a um documento rico e funcional.


autor: Jonas Bolognim