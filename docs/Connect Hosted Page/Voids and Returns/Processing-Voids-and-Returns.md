## Voiding or Returning a Transaction

If your customer has canceled the order or you detect suspicious order details, you can void the transaction referencing the original Transaction ID.

To do this you'll need to use our REST API to perform the function. Voids can only be executed under certain circumstances:

 - For a sale transaction, the same business day as the sale
 - For a Pre-Authorisation, at any time

If you need to cancel a sale transaction, but it's a subsequent business day, process a return.

Steps to implement the return REST API call can be found [here](../../Payment APIs/Taking Payments/PostAuths and Returns/PostAuths-and-Returns.md)

Steps to implement a void REST API call can be found [here](../../Payment APIs/Taking Payments/Taking Customer Payments/The-Payments-API.md)