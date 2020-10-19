---
tags: [Payments]
---
# IPG_Payments_Documentation

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

Polymorphism for our Payments API is based on request types (the requestType field), which enables you to distinguish between customer payments based on the type of transaction (sale, refund, cancellation etc.) and the payment method the customer wants to use (credit or debit cart, digital wallet, SEPA, Paypal etc.). 

## /payments

The /payments API allows you to create, inquire about, and finalize payment transactions. It will also enable you to void a previous transaction, to refund a previous transaction or to execute partial refunds or voids. Finally, and where necessary, it will enable you to pre-authorise transactions that can be completed later.

The API enables you to make payments using different payment instruments, via credit or debit cards, tokens or through local payments methods such as SEPA DD or Paypal account. All of these methods are explained below. You can also use the API to accept wallet transactions via payment methods such as Apple Pay or Google Pay. It will allow you to use extra authentication protocols to ensure customer payments are safer, and to protect you from Fraud.

Our Payments API has two main uses - Primary and Secondary Transactions. Primary transactions are typical sale transactions, pre-authorisations or credits.  

### Primary Transactions

💰💰💰







