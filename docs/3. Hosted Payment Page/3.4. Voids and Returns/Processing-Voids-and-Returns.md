# Voiding or Returning a Transaction

If your customer has cancelled the order or you detect suspicious order details, you can void the transaction referencing the original Transaction ID.

To do this you'll need to use our API to perform the function.
 
Voids can only be executed under certain circumstances:

 - For a sale transaction, the same business day as the sale
 - For a Pre-Authorisation, at any time

If you need to cancel a sale transaction, but it's a subsequent business day, process a return.

Steps to implement the return API call can be found in section `3.1.VII Post Auths & Returns`

Steps to implement a void REST API call can be found in section `3.1.II Payments Resource`