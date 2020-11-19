# /orders/{order-id}

The /orders API enables you to post a secondary transaction using your own order ID. Instead of submitting the POST with a return or postauth requestType and the `ipgTransactionId`, you can use your `orderId` value, the one you provided to us in the Order object in the primary transaction submitted to /payments. In addition, you can use GET to pull details of the primary transaction using the same `orderId` reference.

```json
{
  "method": "get",
  "url": "https://prod.api.firstdata.com/ipp/payments-gateway/v2/payments/IPG001001001",
  "query": {},
  "headers": {
    "Content-Type": "application/json",
    "Client-Request-Id": "",
    "Api-Key": "",
    "Timestamp": "",
    "Message-Signature": ""
  }
}
```

You can find the model for the /orders API [here](https://docs.fiserv.com/docs/payments/reference/Payments.v1.yaml/paths/~1orders~1%7Border-id%7D/post).