---
tags: [Payments]
---
# Payments API

## Use this API to accept Payments of all types, and to process secondary actions like Voids, Refunds or Pre-Auth Completions

You can use our payment API to offer a range of payment methods to your customers. It allows you to take payment using whatever the most appropriate transaction type is. 

Fiserv’s Payment API supports the following major payment methods; Visa, Mastercard, Amex, Apple Pay, Google Pay, Paypal, Klarna, as well as multiple other international and local payment methods.

This API can be used to create and complete a payment, create and manage a recurring payment schedule, generate a payment link to send to a customer, verify a customer’s card or offer them the option to pay in their local currency. 
You can also use our Store Management APIs to manage your store configuration.

Once you’ve executed a transaction using these APIs, you can pull all the detail you need on the transaction using our Merchant Intelligence APIs.

## How our Payments API works

### Header and Parameters

Our Payments API has a consistent Header structure based on a set of Parameters. To create the header, provide the following values:

Header Parameter | Type | Description
-----------------|------|------------
 Content-Type | String | Define this attribute as application/json
 Client-Request-Id | String | This is an ID you generate so we can mutually track your requests. It should be unique per request. We recommend this is built in 128-bit UUID format. 
 Api-Key | String | This is the key you'll use to identify yourself to us. You'll be given a key for development and testing, then a different key when you're ready to go live. 
 Timestamp | Integer | You should define this as an epoch timestamp in milliseconds. This is used for message signature generation and for time limit control.
 Message-Signature | String | We use this for security. The Message-Signature is the Base64 encoded HMAC hash (SHA256 algorithm with the API Secret as the key.) For more information, refer to the supporting documentation on the Developer Portal.

### Polymorphism

Our Payments API is Polymorphic - this means that you can submit various request types with different payloads to the same API and generate different payments types and responses. The term is taken from chemistry - Polymorphism is the ability of solid materials to exist in two or more crystalline forms with different arrangements. When we apply this to API design, it means the same API can be used for multiple payment actions. We enable this by defining different schemas for the payload body, each of which is used for a different payment action. 

Polymorphism for our Payments API is based on request types (the requestType field), which enables you to distinguish between customer payments based on the type of transaction (sale, refund, cancellation etc.) and the payment method the customer wants to use (credit or debit cart, digital wallet, SEPA, Paypal etc.). You'll use the same end point to execute a payment for all of these variations, but the requestType value you submit, and the other objects in the payload, will vary dependent on the type of customer payment you are trying to take.

As an example, you'll use the paymentCardSaleTransaction requestType to take a normal card payment when a customer wants to check out. You can then use a secondary transaction requestType to refund or void the transaction.


# /payments

The /payments API allows you to create, inquire about, and finalize payment transactions. It will also enable you to void a previous transaction, to refund a previous transaction or to execute partial refunds or voids. Finally, and where necessary, it will enable you to pre-authorise transactions that can be completed later.

The API enables you to make payments using different payment instruments, via credit or debit cards, tokens or through local payments methods such as SEPA DD or Paypal account. All of these methods are explained below. You can also use the API to accept wallet transactions via payment methods such as Apple Pay or Google Pay. It will allow you to use extra authentication protocols to ensure customer payments are safer, and to protect you from Fraud.

Our Payments API has two main uses - Primary and Secondary Transactions. Primary transactions are typical sale transactions, pre-authorisations or credits. Secondary Transactions let you refund a transaction, void a transaction or complete a pre-authorisation. 

## Primary Transactions

Primary transactions are used to execute a customer payment, pre-authorisation or credit transaction without reference to a prior transaction. For this walkthrough, we’re going to use the PaymentCardSaleTransaction request type (all request types should have links), which you can use for a normal credit or debit card payment transaction without reference to a previous transaction. Each of the different request types, the scenarios in which you might want to use them, and the differences in the JSON for each request type, are explained within this guide.

The PaymentCardSaleTransaction request type requires the following fields to post a Primary Transaction. 

```json http
{
  "method": "post",
  "url": "https://prod.api.firstdata.com/ipp/payments-gateway/v2/payments",
  "headers": {
    "Content-Type": "application/json",
    "Client-Request-Id": "",
    "Api-Key": "",
    "Timestamp": "",
    "Message-Signature": ""
  },
  "body": {
    "requestType": "PaymentCardSaleTransaction",
    "transactionAmount": {
      "total": "12.04",
      "currency": "USD"
    },
    "paymentMethod": {
      "paymentCard": {
        "number": "5424180279791732",
        "securityCode": "977",
        "expiryDate": {
          "month": "12",
          "year": "24"
        }
      }
    }
  }
}
```

### Request Types

Different payment actions require different requestType values. The table below explains the situations in which you might want to use the different requestType values. The technical detail for each of these requestTypes is included in the Schema section of the API explorer. 

Payment Type | requestType | Payment Scenario
---------|----------|---------
Single Card Payment |	[PaymentCardSaleTransaction](https://docs.fiserv.com/docs/payments/reference/Payments.v1.yaml/components/schemas/PaymentCardSaleTransaction) | Use this requestType to execute a normal customer payment transaction with a credit or debit card.
Refund | [PaymentCardCreditTransaction](https://docs.fiserv.com/docs/payments/reference/Payments.v1.yaml/components/schemas/PaymentCardCreditTransaction) |	Use this requestType to execute an original credit payment transaction to a customer’s credit or debit card. This means you’ll refund this amount to the customer’s card without 
Pre-auth (taking a deposit) |	[PaymentCardPreAuthTransaction](https://docs.fiserv.com/docs/payments/reference/Payments.v1.yaml/components/schemas/PaymentCardPreAuthTransaction) |	Use this requestType to pre-authorise an amount against a card, for completion at a later point.
Payment with a token	| [PaymentTokenSaleTransaction](https://docs.fiserv.com/docs/payments/reference/Payments.v1.yaml/components/schemas/PaymentTokenSaleTransaction) |	Use this requestType to execute a normal customer payment transaction with a token generated previously.
Token Refund | [PaymentTokenCreditTransaction](https://docs.fiserv.com/docs/payments/reference/Payments.v1.yaml/components/schemas/PaymentTokenCreditTransaction) |	Use this requestType to execute an original credit payment transaction to a customer’s credit or debit card using a token generated previously.
Token pre-auth |	[PaymentTokenPreAuthTransaction](https://docs.fiserv.com/docs/payments/reference/Payments.v1.yaml/components/schemas/PaymentTokenPreAuthTransaction) |	Use this requestType to pre-authorise an amount against a token, for completion at a later point.
What is sepa |	[SepaSaleTransaction](https://docs.fiserv.com/docs/payments/reference/Payments.v1.yaml/components/schemas/SepaSaleTransaction) |	Use this requestType to take payment from a customer via SEPA.
Wallet Sale	| [WalletSaleTransaction](https://docs.fiserv.com/docs/payments/reference/Payments.v1.yaml/components/schemas/WalletSaleTransaction)	| Use this requestType to execute a normal customer payment transaction with a wallet payment method.
Wallet Pre-Auth |	[WalletPreAuthTransaction](WalletPreAuthTransaction) |	Use this requestType to execute a Pre-Authorisation using a Wallet Payment Method

For Primary Transactions, the decision matrix for defining which requestType to use is laid out below:



The differences that are required in the JSON for each of these request types are explained through the rest of this guide.

### Payment Methods

To create a customer payment using a Primary Transaction, you must include the paymentMethod object in the JSON call. Dependent on the requestType, you’ll make some changes to the paymentMethod object to reflect different payment instruments and transaction types. 

To take a Card Payment, use the PaymentCardPaymentMethod:

```json yaml
{
  "paymentCard": {
    "number": "5424180279791732",
    "expiryDate": {
      "month": "03",
      "year": "21"
    },
    "securityCode": "977"
  }
}
```

To take a Token Payment, use the PaymentTokenPaymentMethod:

```json yaml
{
  "paymentToken": {
    "value": "1235325235236",
    "function": "DEBIT",
    "securityCode": "977"
  }
}
```

Within the paymentMethod object, you’ll provide different values dependent on the payment method type being used for payment. This defines the payment instrument the customer is using to make the transaction. The available options available are defined below. These will be regularly updated as more payment methods become available. Further information on the payload changes associated with each of these methods is accessed via clicking the paymentMethodType. 

paymentMethodType |	Description
---------|----------
[PAYMENT_CARD](https://docs.fiserv.com/docs/payments/reference/Payments.v1.yaml/components/schemas/PaymentCardPaymentMethod) |	The Payment Card payment method covers all Credit and Debit Card types and networks. Your merchant acquiring contract will tell you which of the payment networks you can accept payments for.
[PAYMENT_TOKEN](https://docs.fiserv.com/docs/payments/reference/Payments.v1.yaml/components/schemas/PaymentTokenPaymentMethod) |	The Payment Token payment method enables you to use a token created by us, instead of the actual payment method details. More information is provided on using Tokens here.
[SEPA](https://docs.fiserv.com/docs/payments/reference/Payments.v1.yaml/components/schemas/SepaPaymentMethod) |	A SEPA transaction enables the customer to pay you via a bank-to-bank request over the European SEPA network.
[WALLET](https://docs.fiserv.com/docs/payments/reference/Payments.v1.yaml/components/schemas/WalletPaymentMethod) |	Enables payment via a Wallet transaction. Wallet transactions require a specific paymentMethod object. More information on this object is provided in the Wallet Payments section here.

## Tokens

Tokens are useful when storing the details of a customer’s payment instruments to secure and speed up checkout later. When you collect a customer's payment methods, you can request a token, which you can then use in place of a customer’s card details to execute future payments. You will need to store the token on your platform, mapped to the customer’s account details. 

To create a token for a customer’s payment instrument, see the Tokens section [here](url).

Tokens are useful as they reduce the risk of attacks on your and our payments systems being successful. Tokenisation also reduces the PCI Requirement for you, as it means you don't need to store card details within your systems, which should reduce your costs associated with PCI Compliance.  

To generate a Token for future use at the same time as submitting a payment, use the createToken object to set a Token up for multiple use (set reusable to True). In addition, you can create your own token for the merchant, send it to us within the object, and set rules as to whether you want to decline payments with duplicate payment details.

```json yaml
{
  },
  "createToken": {
    "value": optional - define your own token,
    "reusable": true / false,
    "declineDuplicates": true / false
  }
}
```

Once the token is created and you have stored it against the customer’s account detail, you can use it to execute payments for the customer. To use a tokenised payment instrument, use the relevant PaymentToken* requestTypes. If you supply a token value, we will store that, otherwise we'll generate a token value and pass it back to you in the response.

When a customer is checking out, and you've previously tokenised their payment details (and you’ve stored the token with the customer’s account record in your systems), you can request token details to enable the customer to confirm they want to pay with the stored payment instrument. To do this, use the GET call against [this end point](https://docs.fiserv.com/docs/payments/reference/Payments.v1.yaml/paths/~1payment-tokens~1%7Btoken-id%7D/get), providing the `tokenid` to receive a [PaymentTokenDetails](https://docs.fiserv.com/docs/payments/reference/Payments.v1.yaml/components/schemas/PaymentTokenDetails) response. Our suggestion is you use the `last4` value and `brand` to enable the customer to correctly identify their payment instrument.

You can then use the Payment Token in the relevant Primary Payment request type to execute the customer payment. To Update or Delete customer payment tokens, see the Token section [here](url).

## SEPA Transactions

To use the SepaSaleTransaction request type, the paymentMethod object is replaced by the sepa object. The sepa object includes all data points required to execute a sepa transaction. Sepa stands for the “Single European Payments Area”, and enables payments to be executed immediately between bank accounts at low cost. This capability is only enabled for domestic payments in Germany at present. Additional countries will be added in the near future.

You must be signed up for SEPA payments and have a mandate reference ID to supply in order to populate the mandate object.

The SEPA PaymentMethod Object is shown below:

```json yaml
{
  "sepa": {
    "iban": "DE34500100600032121604",
    "name": "Alan Turner",
    "country": "DEU",
    "email": "AlanTurner@Bonn.com",
    "mandate": {
      "reference": "3JLCSTIG",
      "url": "https://www.fiserv.com",
      "signatureDate": "2020-10-15",
      "type": "SINGLE"
    }
  }
}
```

## Wallet Transactions

We support Apple Pay and Google Pay transactions via our REST APIs. We can accept transactions with payload data encrypted, or unencrypted, should you have the capability to decrypt wallet payload data yourself. The following sections will enable you to setup Apple Pay and Google Pay.

To use the Wallet request types, use the abstract class walletPaymentMethod, defining one of the abstract class’s children to create the walletPaymentMethod using the walletType value. For example, use the encryptedApplePayWalletPaymentMethod to execute the payment using encrypted Apple Pay payment data. An example of the walletPaymentMethod for this type of payment is show below. If you want to decrypt the wallet payment data yourself and provide the unencrypted data, that can be achieved via the dencryptedApplePayWalletPaymentMethod object.

To see step-by-step guides for implementing Apple and Google Pay wallet payment methods, please see this [section](docs/Implementing-Wallet-Payment-Methods.md).

## 3DSecure Transactions

A major part of accepting ecommerce transactions is the requirement to authenticate consumers and their payment instruments via the 3DSecure ecosystem. This enables each consumer to authenticate themselves with their card issuer, and provides for both a high security payments experience, and a frictionless payment experience (dependent on the data the consumer provides as part of the authentication process).

How often does 3DSecure happen?
Explain what SCA is


