# /orders/{order-id}

The /orders API enables you to post a secondary transaction using your own order ID. Instead of submitting the POST with a return or postauth requestType and the `ipgTransactionId`, you can use your `orderId` value, the one you provided to us in the Order object in the primary transaction submitted to /payments. In addition, you can use GET to pull details of the primary transaction using the same `orderId` reference.



