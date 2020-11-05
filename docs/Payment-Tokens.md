# Payment-Tokens

## /payment-tokens

Payment tokens allow you to store customer payment cards within your systems without having to store card data, which creates security and compliance requirements such as PCI. You can either submit a request to tokenise a payment card as part of a payment, or you can tokenise the card separately. Tokenising a payment card as part of a payment is covered as part of the /payments guide documentation in the Payments section. 

### Create a Payment Token

To tokenise a payment card separately to a payment, POST the payload below to /payment-tokens (example provided for card, but use the relevant paymentMethod object for the instrument of your choice). 

```json YAML
{
  "requestType": "PaymentCardPaymentTokenizationRequest",
  "paymentCard": {
    "number": "4012000033330026",
    "expiryDate": {
      "month": "12",
      "year": "24"
    }
  },
  "createToken": {
    "value": 123abc456def
    "reusable": true,
    "declineDuplicates": false
  },
  "accountVerification": false,
  "additionalDetails": {
    "operatorId": "OPERATOR_ID_123XXX",
    "salesSystemId": "W-EU-H3866-FLS2"
  }
}
```

If you want to set your own value for the token, include the `value` attribute in the createToken object. If you don't include this, we'll define the token value and return it in the response as `value` attribute in the paymentToken object. The `reuseable` attribute is boolean. If set to true, the token can be reused. If false, it has only a single use. The `declineDuplicates` attribute is also boolean. If you've provided your own token value for payment instrument, and a payment is submitted with the same payment card, but untokenised, the system will decline the payment (this is a fraud control you can set at token level).

### Updating Payment Tokens

You can update one or more Payment Tokens at a time, and change the settings associated with the Token, or the payment card associated with the Token. To make these updates, make a PATCH to /payment-tokens using requestType PaymentCardPaymentTokenUpdateRequest. Note - the PaymentTokens object is a list, and can include multiple payment tokens. See the example below to see how to construct the payload. Each of the token objects below includes the updates required - these will automatically be written to the token record on our systems if the request is processed successfully.

```json YAML
{
  "requestType": "PaymentCardPaymentTokenUpdateRequest",
  "paymentTokens": [
    {
      "value": "1751905117310026",
      "reusable": true,
      "declineDuplicates": false,
      "paymentCard": {
        "number": "5424180279791732",
        "expiryDate": {
          "month": "03",
          "year": "21"
        },
        "securityCode": "977"
      }
    },
    {
      "value": "9877hkhk68688888ffgh",
      "reusable": true,
      "declineDuplicates": false,
      "paymentCard": {
        "number": "4773410012347324",
        "expiryDate": {
          "month": "12",
          "year": "26"
        },
        "securityCode": "123"
      }
    }
  ]
}
```

If one of the updates fails, this will be specified in the response - see below for an example of a response to the above in which the second update failed. 

```json YAML
{
  "requestStatus": "PARTIAL_SUCCESS",
  "requestTime": "1554308829345",
  "errors": {
    "details": [
      {
        "message": "HOSTED_DATA_ID4773410012347324. Invalid credit card number: CreditCard [cardNumber=4773410...7324, expirationMonth=12, expirationYear=2026"
      },
    ]
  }
}
```

### Other Payment Token Functions.

A payment token can be deleted by sending a DELETE to /payment-tokens/{token-id}. 

You can retrieve the payment card data associated with a token, and the token settings, by sending a GET to /payment-tokens/{token-id}. 




