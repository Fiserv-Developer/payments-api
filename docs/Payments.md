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

### Apple Pay

Apple Pay enables secure, simple checkouts in your app or on your website.

Your customer will tap the Apple Pay button in the app or on the website, selects the payment card they want to use and uses the Touch-ID to complete the transaction.

1. Your App communicates with the your server and creates a transaction ID
2. Your App obtains the encrypted transaction payload (The tokenized card data "DPAN", Cryptogram, and transaction details) from Apple's Pass Kit Framework
3. Your App sends the encrypted transaction payload to our /payments API using the Apple Pay SDK
4. Our payments platform decrypts the encrypted transaction payload and processes the transaction
5. Our /paymennts API responds back to the Merchant App (through the SDK) with either an approval or decline

To accept Apple Pay, you'll need us to generate a Certificate Signing Request. Please reach out to your local Integration Support team for getting your account enabled. Our Integration team will generate the CSR and share it with you along with the Merchant Identifier.

To accept Apple Pay, you'll need to follow these steps:

1. Generate Payment Processing Certificate from Apple Portal

- Login to apple 'Account' in developer.apple.com
- In-order to generate certificate you must have a paid apple developer account or an organization account. New users must follow the prompts to set up a developer account
  - Select Certificates, Identifiers and Profile :
    - Certificates, Identifiers and Profile 
      - Select Identifier's and in the dropdown in the upper-right corner select merchant id
        - Enter a unique merchant id, and upload a valid CSR in .pem format
- Once successfully uploaded, apple provides the payment processing certificate and the merchant id will come up in the certificate section.
- Download payment processing certificate and install in your computer by double clicking it.
- The certificate should show up in the 'Key chain access' of your Macbook.

2. Upload and register the apple development certificate for your machine

- Request a new certificate from your keychain access
- Request certificate
- Follow the prompt and request the certificate to be saved on file
- In the 'Certificate' section in the apple portal, click on the '+' and follow the prompt to request apple developer certificate for 'IOS' development
- Upload the requested certificate
- Download and install the apple pay developer certificate
- Your machine is now setup for programming IOS app using Xcode

3. Set-up Provision Profile for the application

- In the 'Certificate, Identifier and Profile' section in the apple portal navigate to profile.
- Click the '+' sign to create a new profile.
- Choose, 'IOS' development and follow the prompts
- In the capabilities select 'Apple Pay' and 'In-App Purchases'
- You can also register the test devices here using, uuid. (Xcode also does this when the app is built on the phone)
- Download the provision profile on to your mac

4. Set-up Project in Xcode

- Select new xcode project with a single view or import an existing project.
- Register the app using the app-id in the apple portal under Identifier->appId
- Go to Xcode->Preferences,->accounts and install the downloaded provision profile, your profile is now linked to your Xcode.
- Click on your project, go to Signing & Capabilities select 'Automatically manage signing'.
- '+ capabilities' add apple pay.
- Under 'Apple Pay' click '+' and add the merchant id's registered in the portal, which in turn will be added to the entitlements file.
- Now the Xcode is set-up for coding.

5. In the SDK enter the URL, api key and api secret and build the app:

- Merchant id: Enter any valid merchant id registered in the apple portal. This gives the capability for a single user to use multiple merchant id's
- Amount: Enter the amount of the transaction
- Transaction type: Select PreAuth or Sale.
- Apple Pay Button: Click this to produce payment sheet and fingerprint authentication for the transaction.

Once the user authenticates the transaction the apple returns the payment token, then the SDK generates the following payload:

```json yaml
{

   "walletPaymentMethod":{

      "walletType":"EncryptedApplePayWalletPaymentMethod",

      "encryptedApplePay":{

         "data":"hbreWcQg980mUoUCfuCoripnHO210lvtizOFLV6PTw1DjooSwik778bH/qgK2pKelDTiiC8eXeiSwSIfrTPp6tq9x8Xo2H0KYAHCjLaJtoDdnjXm8QtC3m8MlcKAyYKp4hOW6tcPmy5rKVCKr1RFCDwjWd9zfVmp/au8hzZQtTYvnlje9t36xNy057eKmA1Bl1r9MFPxicTudVesSYMoAPS4IS+IlYiZzCPHzSLYLvFNiLFzP77qq7B6HSZ3dAZm244v8ep9EQdZVb1xzYdr6U+F5n1W+prS/fnL4+PVdiJK1Gn2qhiveyQX1XopLEQSbMDaW0wYhfDP9XM/+EDMLaXIKRiCtFry9nkbQZDjr2ti91KOAvzQf7XFbV+O8i60BSlI4/QRmLdKHmk/m0rDgQAoYLgUZ5xjKzXpJR9iW6RWuNYyaf9XdD8s2eB9aBQ=",

         "header":{

            "applicationDataHash":"94ee059335e587e501cc4bf90613e0814f00a7b08bc7c648fd865a2af6a22cc2",

            "ephemeralPublicKey":"MFkwEwYHKoZIzj0CAQYIKoZIzj0DAQcDQgAEvR+anQg6pElOsCnC3HIeNoEs2XMHQwxuy9plV1MfRRtIiHnQ6MyOS+1FQ7WZR2bVAnHFhPFaM9RYe7/bynvVvg==",

            "publicKeyHash":"KRsyW0NauLpN8OwKr+yeu4jl6APbgW05/TYo5eGW0bQ=",

            "transactionId":"31323334353637"

         },

         "signature":"MIAGCSqGSIb3DQEHAqCAMIACAQExDzANBglghkgBZQMEAgEFADCABgkqhkiG9w0BBwEAAKCAMIIB0zCCAXkCAQEwCQYHKoZIzj0EATB2MQswCQYDVQQGEwJVUzELMAkGA1UECAwCTkoxFDASBgNVBAcMC0plcnNleSBDaXR5MRMwEQYDVQQKDApGaXJzdCBEYXRhMRIwEAYDVQQLDAlGaXJzdCBBUEkxGzAZBgNVBAMMEmQxZHZ0bDEwMDAuMWRjLmNvbTAeFw0xNTA3MjMxNjQxMDNaFw0xOTA3MjIxNjQxMDNaMHYxCzAJBgNVBAYTAlVTMQswCQYDVQQIDAJOSjEUMBIGA1UEBwwLSmVyc2V5IENpdHkxEzARBgNVBAoMCkZpcnN0IERhdGExEjAQBgNVBAsMCUZpcnN0IEFQSTEbMBkGA1UEAwwSZDFkdnRsMTAwMC4xZGMuY29tMFkwEwYHKoZIzj0CAQYIKoZIzj0DAQcDQgAErnHhPM18HFbOomJMUiLiPL7nrJuWvfPy0Gg3xsX3m8q0oWhTs1QcQDTT+TR3yh4sDRPqXnsTUwcvbrCOzdUEeTAJBgcqhkjOPQQBA0kAMEYCIQDrC1z2JTx1jZPvllpnkxPEzBGk9BhTCkEB58j/Cv+sXQIhAKGongoz++3tJroo1GxnwvzK/Qmc4P1K2lHoh9biZeNhAAAxggFSMIIBTgIBATB7MHYxCzAJBgNVBAYTAlVTMQswCQYDVQQIDAJOSjEUMBIGA1UEBwwLSmVyc2V5IENpdHkxEzARBgNVBAoMCkZpcnN0IERhdGExEjAQBgNVBAsMCUZpcnN0IEFQSTEbMBkGA1UEAwwSZDFkdnRsMTAwMC4xZGMuY29tAgEBMA0GCWCGSAFlAwQCAQUAoGkwGAYJKoZIhvcNAQkDMQsGCSqGSIb3DQEHATAcBgkqhkiG9w0BCQUxDxcNMTkwNjA3MTg0MTIxWjAvBgkqhkiG9w0BCQQxIgQg0PLaZU4YWZqtP9t/ygv9XIS/5ngU6FlGjpvyK6VFXVMwCgYIKoZIzj0EAwIERjBEAiBTNmQEPyc3aMm4Mwa0riD3dNdSc9aAhslj65Us8b3aKwIgNSc/y+CWpsr8qDln0fZK6ZD/LWPMxofQedlPy7Q6gY8AAAAAAAA=",

         "version":"EC_v1",

         "applicationData":"VEVTVA==",

         "merchantId":"merchant.com.fapi.tcoe.applepay"

      }

   },

   "transactionAmount":{

      "total":"12.99",

      "currency":"USD"

   }

}
```

### Google Pay

Google Payment happens after your customer taps the "Google Pay" button, then selects a payment method and shipping address.

If the purchase originates from a third-party site

1. Your server issues a credential request with the Merchant ID and Processor Name as First Data to Google
2. Google returns response with encrypted payment credentials signed with the First Data key to the merchant server
3. You send the encrypted payload to our /payments API
4. We decrypt and validate the payload, then process the transaction and respond back to you with either an approval or decline response

If the purchase originates from a Google site:

1. Google initiates a purchase request to you after the consumer confirms the order
2. Your server issues a request with your Merchant ID and Processor Name as First Data to Google
3. Google returns response with encrypted payment credentials signed with the First Data key to your server
4. You send the encrypted payload to our /payments API
5. We decrypt and validate the payload and process the transaction and respond back to you with either an approval or decline response

Google Pay enables developers to add payment processing to Android-compatible apps and on Chrome on Android. These APIs allow consumers to pay with any credit card they have stored in their Google account, including any cards they may have previously set up on their Android Pay digital wallet. 

Implementing Google Pay

1. Merchant Boarding

Please reach out to your local Integration Support team for getting your account enabled. The Integration team will share a Sample Application to generate Google Encrypted payloads.

2. Prerequisites to build and run the sample Application

Developers wishing to use the Fiserv Google Pay sample application will need the following software and hardware:

- Google Play Services version 18.0.0 
- A physical device or an emulator to use for developing and testing. Google Play services can only be installed on an emulator with an AVD that runs Google APIs platform based on Android 4.4 or higher.
- The latest version of Android Studio. This includes:
  - The latest version of the AndroidSDK, including the SDK Tools component. The SDK is available from the
  - Android SDK Manager
  - JavaJRE(JDKfordevelopment) as per Android SDK requirements

Your project should be able to compile against Android 4.4 (KITKAT) or higher.

For more details, please refer https://developers.google.com/pay/api/android/guides/setup

3. Changes to be made in the Application 

The following parameters need to be defined 

- [ ] Merchant ID
- [ ] Merchant Token
- [ ] APIKey
- [ ] APISecret

Define the Fiserv Object Parameters: Parameters must be updated in the following files: 

- Constants.java
- EnvData.java

Update the Constants.java file with the Merchant ID and Gateway Tokenization parameters. Note that:

- The Merchant ID will be shared by the Integration team and
- The Gateway Tokenization parameter defaults to ‘firstdata’. 

In the EnvData.java file, set the following environment variables, which will be shared by the Integration Team:

- APIKey
- Token 
- APISecret 
 
envMap.put("CERT", new EnvPropertiesImpl( "CERT",

 "https://cert.api.firstdata.com/gateway/v1/payments",

 "---------", "----------", "-----------"));

gatewayMerchantId and the APIGEE credentials will be provided by the Integration Team

4. Credit Card for Testing

Note that for testing purposes; the credit card information used in the app must be attached to an active account. The standard test cards will not be validated by Google and will fail in processing, our integration team will provide google pay-specific test cards.

5. Execute Authorize and Purchase Request

The Authorization parameter, required as part of the Header for a Fiserv API transaction, needs to be created. Construct the data param by appending the following parameters in the order shown:

- apikey – the developer’s API key
- nonce – A secure random number
- timestamp – Epoch timestamp in milliseconds
- token – the Merchant Token 
- payload – The actual body content passed as the POST request 

6. Google Pay APK Installation to Device

- Once the downloaded code for the sample app is built successfully in Android Studio, build the APK and install it on your device. 
- Once the APK is installed, select the Open option to access the application. 
- The sample application should now be installed on the test device (nonPROD environment), and the response from a payment processed in the First Data nonPROD environment/Google test environment available

Sample Google Pay Request within the `walletPaymentMethod` object for a `WalletPreAuthTransaction`:

```json yaml
{

  "requestType": "WalletPreAuthTransaction",

  "walletPaymentMethod": {

    "walletType": "EncryptedGooglePayWalletPaymentMethod",

    "encryptedGooglePay": {

      "data": {

        "encryptedMessage": "8nxjB9mr2tWZeDRQRcGN91UUnb7AioGp3oRo8kmQ6lyvJZiqD7PJlbRCYElNqUmr6Z8zK7b2gO9MKOjpnTCqH0qAe2vuIlwNXB60M2Lh7Qfl3bVgWzwF/FfFcenVW381hoItYi8AjWnWoydz1XMTEv2qhqUG03mEnRXdMyDyk6KKZXoW8Qc0p1F1thbxxu8weU8CZbZsWGGTjB42cilIqLVbribcOAG8Oas1AcgefFsu2hwp4gdSuOg7wmeSV7XKsGQzzVy85qtjuqET2XYzJE3K/Wh9QKkhu5P9Ms5s1+Smr2IjRyidqQa88SxQplrVoo9+PvT0bxFcMspBmO3pLkuaZSUBy++dL2fefcxNJvGCFfFhdxW9DojuuQxgpeu7RAQUsGLyFmr/4ZfBxt882xTmpX9MRx5CAudl9qUgBfKdwWwMX35qSbDTm1ju5XXzNh94VebjD3bB9Zj8qgbmUOr/+6OQLhoFJyBCXgx3EEH8hBwNVFrss/SLwQvFhZh62eO6lOtnmbOtP1yTDDVqGDBfai5SwAmM+KTcc9SGv/xDC+cWe8ck+aCBkG4HoRPapUVMZ3JIgV7yzTsVLJE\\u003d",

        "ephemeralPublicKey": "BGH3fRFdoAobYrAlxnZOCYzkH84Cna92IZxtgsU36CMDaqSaDYb9/LsY8qw+vMtlBnwsUg/YVMOeeKp+qDkOWb4\\u003d",

        "tag": "nvmOUNpnOTZULLhMxT/hWCHzH/4f7gGpfvQgwl3p8ng\\u003d"

      },

      "signature": "MEUCIFWTRWUZAOM5nfJC79FtJm56olnbwG4H5uWWxAUWAquiAiEA24j/BcOroeISsdJzYsyoVi8wzu4tnmKw+jdsGfuvPko=",

      "version": "ECv1"

    }

  },

  "transactionAmount": {

    "total": "1200",

    "currency": "USD"

  }

}
```
