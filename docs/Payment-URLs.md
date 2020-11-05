# Payment-URLs

## Using Payment URLs

You can use the /payment-url API to generate a URL that you can send to a customer, which they can then use to pay you. This is useful when sending a customer an invoice - you are more likely to get paid sooner than if provide your bank details and ask for a transfer. When the customer clicks on the payment URL, they are directed to our hosted payment page solution to make a payment, and the payment is routed to your store on our systems for processing. 

Using this API you can Create, Delete and interrogate the status of an existing payment URL.

## Creating Payment URLs

Use POST to make a call to /payment-url and specify the values for the payload as defined in the following table.

Attribute | Explanation 
---------|----------
transactionType | Text
transactionNotificationURL | Text
expiration | Text
authenticateTransaction | Text
dynamicMerchantName | Text
invoiceNumber | Text
purchaseOrderNumber | Text
ip | Text


```json YAML
{
  "transactionAmount": {
    "total": "42.42",
    "currency": "EUR"
  },
  "transactionType": "SALE",
  "transactionNotificationURL": "https://showmethepaymentresult.com",
  "expiration": "4102358400",
  "authenticateTransaction": true,
  "dynamicMerchantName": "MyWebsite",
  "invoiceNumber": "96126098",
  "purchaseOrderNumber": "123055342",
  "ip": "264.31.73.24"
}
```