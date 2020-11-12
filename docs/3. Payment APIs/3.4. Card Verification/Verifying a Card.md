# Card Verification

Use the Card Verification service to check a card is available for charging before a customer uses it to pay. Submit the payload below using a POST method to the /card-verification API and we'll perform a zero value authorisation against the card to ensure it isn't fraudulent, blacklisted, expired or blocked. Essentially it lets us check whether the card and account are in good standing and available to make payment.

```json http
{ 
  method: "POST",
  url: 'https://prod.api.firstdata.com/ipp/payments-gateway/v2/card-verification',
  headers: {
    "content-type": 'application/json',
    "client-request-id": '',
    "api-key": '',
    "timestamp": '',
    "message-signature": ''
  }
  "body": {
    "paymentCard": {
      "number": '4035874000424977',
      "expiryDate": {
        "month": '12', 
        "year": '20'
      },
      "securityCode": '977'
    },
    "billingAddress": {
      "address1": '5565 Glenridge Conn',
      "city": 'Atlanta',
      "postalCode": '30342',
      "country": 'USA'
    }
  }
}
```

<!-- theme: success -->

> You can find the model for /card-verification [here](https://docs.fiserv.com/docs/payments/reference/Payments.v1.yaml/paths/~1card-verification/post).

Below is an example of a card verification payload (body only).

```json YAML
{
  "paymentCard": {
    "number": "4035874000424977",
    "expiryDate": {
      "month": "12",
      "year": "20"
    },
    "securityCode": "977"
  },
  "billingAddress": {
    "address1": "5565 Glenridge Conn",
    "city": "Atlanta",
    "postalCode": "30342",
    "country": "USA"
  }
}
```


