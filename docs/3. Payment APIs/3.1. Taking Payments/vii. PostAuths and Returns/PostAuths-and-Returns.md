
# /Payments/{Transaction-id}

## Secondary Transactions

Use secondary transactions to Void an original transaction, Return against an original transaction or to complete a Post-Auth transaction. The transactionId Parameter, populated for the original transaction that requires a secondary action, must be populated for each of these request types. 

To cancel the original transaction (same day as the original transaction), use the voidTransaction requestType. To return (reverse on subsequent day) an original transaction, use returnTransaction as the requestType. To complete a Pre-Authorised transaction using a Post-Authorisation transaction, reference the original transaction in the parameter data, then place a POST using the PostAuth schema. 

An updated version of the Decision Matrix diagram provided earlier is shown below, with the secondary transaction requestTypes now included. 

INSERT DIAGRAM

Secondary transactions are also based on requestTypes. The table below provides links to the requestType schemas and provides the method to use. In all of these transactions, the transaction-id attribute must be populated with the value returned in the 200 response message in the `ipgTransactionId` field for the relevant primary transaction. 

To retrieve the status of a transaction you’ve already submitted, place a GET call to the /PAYMENTS/{transaction-id} end point. The gateway will return the details and state of the transaction you submitted. The below sample response generator uses a ReturnTransaction requestType.

```json http
{
  "method": "post",
  "url": "https://prod.api.firstdata.com/ipp/payments-gateway/v2/payments/1001-1001-1001-1001",
  "query": {},
  "headers": {
    "Content-Type": "application/json",
    "Client-Request-Id": "",
    "Api-Key": "",
    "Timestamp": "",
    "Message-Signature": ""
  }
  "body": {
  "requestType": "ReturnTransaction",
  "transactionAmount": {
    "total": 3,
    "currency": "USD"
  }
  }
}
```

requestType | Method | Description
---------|----------|---------
 [VoidTransaction](https://docs.fiserv.com/docs/payments/reference/Payments.v1.yaml/components/schemas/VoidTransaction) | POST | The VoidTransaction requestType enables you to cancel a transaction you submitted earlier the same day
 [VoidPreAuthTransaction](https://docs.fiserv.com/docs/payments/reference/Payments.v1.yaml/components/schemas/VoidPreAuthTransaction) | POST | The VoidTransaction requestType enables you to cancel a PreAuthorisation Transaction
 [PostAuthTransaction](https://docs.fiserv.com/docs/payments/reference/Payments.v1.yaml/components/schemas/PostAuthTransaction) | POST | The PostAuthTransaction requestType enables you to complete a Pre-Authorisation Transaction against the same 
 [ReturnTransaction](https://docs.fiserv.com/docs/payments/reference/Payments.v1.yaml/components/schemas/ReturnTransaction) | POST | The ReturnTransaction requestType enables you to complete a return against a transaction taken prior to the current day
 [Transaction Inquiry](https://docs.fiserv.com/docs/payments/reference/Payments.v1.yaml/paths/~1payments~1%7Btransaction-id%7D/get) | GET | execute a simple GET call against the end point with the `ipgtransactionid` value from the transaction you want to inquire against 

## Additional Payment Scenarios

There are a number of business scenarios that require combinations of different calls to the same, or different end points. The examples below demonstrate the way in which the different requestTypes and calls can be made to the /payments API to generate different payments outcomes.

### Incrementing or decrementing a Pre-Auth

To increase or decrease the value of a pre-authorisation transaction, submit another pre-auth for the same cardholder referencing the `orderId` field value from the reponse associated with the original Pre-Auth transaction:

    
```json YAML
{​​​​​​​​
  "requestType": "PaymentCardPreAuthTransaction",
  "transactionAmount": {​​​​​​​​
    "total": "17.00",
    "currency": "EUR"
  }​​​​​​​​,
  "paymentMethod": {​​​​​​​​
    "paymentCard": {​​​​​​​​
      "number": "4149011500000147",
      "securityCode": "147",
      "expiryDate": {​​​​​​​​
        "month": "12",
        "year": "20"
      }​​​​​​​​
    }​​​​​​​​
  }​​​​​​​​,
  "order": {​​​​​​​​
    "orderId": "{​​​​​​​​{​​​​​​​​lastOrderId}​​​​​​​​}​​​​​​​​"
  }​​​​​​​​
}​​​​​​​​
```

The original Pre-Authorisation transaction will then be incremented/decremented as set in your request.

## Completing and voiding pre-auth transactions

To complete a Pre-Auth, POST a postAuth transaction to complete the Pre-Authorisation, post a secondary transaction to /payments/{transaction-id} stating the `orderId` field value from the reponse associated with the original Pre-Auth transaction in {transaction-id}. The splitShipment object enables multiple partial Post-Authorisations in scenarios in which there are multiple shipments against a single original Pre-Authorisation.  

```json YAML
{
  "requestType": "PostAuthTransaction",
  "transactionAmount": {
    "total": "12.04",
    "currency": "USD"
  },
  "splitShipment": {
    "totalCount": 1,
    "finalShipment": true
  }
}
```

To void the Post-Auth, thereby re-opening the Pre-Auth, POST a voidTransaction request type as a secondary transaction setting the `orderId` value from the postAuth response as the {transaction-id}. To void the Pre-Authorisation transaction, POST a voidPreAuthTransaction request type as a secondary transaction setting the `orderId` value from the Pre-Authorisation response as the {transaction-id}. 

