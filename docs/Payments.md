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


## /payments

The /payments API allows you to create, inquire about, and finalize payment transactions. It will also enable you to void a previous transaction, to refund a previous transaction or to execute partial refunds or voids. Finally, and where necessary, it will enable you to pre-authorise transactions that can be completed later.

The API enables you to make payments using different payment instruments, via credit or debit cards, tokens or through local payments methods such as SEPA DD or Paypal account. All of these methods are explained below. You can also use the API to accept wallet transactions via payment methods such as Apple Pay or Google Pay. It will allow you to use extra authentication protocols to ensure customer payments are safer, and to protect you from Fraud.

Our Payments API has two main uses - Primary and Secondary Transactions. Primary transactions are typical sale transactions, pre-authorisations or credits. Secondary Transactions let you refund a transaction, void a transaction or complete a pre-authorisation. 

### Primary Transactions

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

Different payment actions require different requestType values. The table below explains the situations in which you might want to use the different requestType values. The technical detail for each of these requestTypes is included in the Schema section of the API explorer. 

Payment Type | requestType | Payment Scenario
---------|----------|---------
Single Card Payment |	PaymentCardSaleTransaction | Use this requestType to execute a normal customer payment transaction with a credit or debit card.
Refund | PaymentCardCreditTransaction |	Use this requestType to execute an original credit payment transaction to a customer’s credit or debit card. This means you’ll refund this amount to the customer’s card without 
Pre-auth (taking a deposit) |	PaymentCardPreAuthTransaction |	Use this requestType to pre-authorise an amount against a card, for completion at a later point.
Payment with a token	| PaymentTokenSaleTransaction |	Use this requestType to execute a normal customer payment transaction with a token generated previously.
Token Refund | PaymentTokenCreditTransaction |	Use this requestType to execute an original credit payment transaction to a customer’s credit or debit card using a token generated previously.
Token pre-auth |	PaymentTokenPreAuthTransaction |	Use this requestType to pre-authorise an amount against a token, for completion at a later point.
What is sepa |	SepaSaleTransaction |	Use this requestType to take payment from a customer via SEPA.
Wallet Sale	| WalletSaleTransaction	| Use this requestType to execute a normal customer payment transaction with a wallet payment method.
Wallet Pre-Auth |	WalletPreAuthTransaction |	Use this requestType to execute a Pre-Authorisation using a Wallet Payment Method

For Primary Transactions, the decision matrix for defining which requestType to use is laid out below:



The differences that are required in the JSON for each of these request types are explained through the rest of this guide.

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

Other PaymentMethods are defined within the 

To take a payment from a consumer paying with a credit or debit card, include the paymentMethod object in the PaymentCardSaleTransaction request. The paymentMethod object should include the paymentCard payment instrument object. The paymentMethod object for this type of transaction is shown below, and the schema for the full payload can be found here.



