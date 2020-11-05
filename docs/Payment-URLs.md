# Payment-URLs

## Using Payment URLs

You can use the /payment-url API to generate a URL that you can send to a customer, which they can then use to pay you. This is useful when sending a customer an invoice - you are more likely to get paid sooner than if provide your bank details and ask for a transfer. When the customer clicks on the payment URL, they are directed to our hosted payment page solution to make a payment, and the payment is routed to your store on our systems for processing. 

Using this API you can Create, Delete and interrogate the status of an existing payment URL.

## Creating Payment URLs

Use POST to make a call to /payment-url and specify the values for the payload as defined in the following table.

Attribute | Explanation 
---------|----------
`transactionType` | this attribute tells our platform what type of transaction to execute when the customer completes the payment via the URL. To take a payment, use SALE. To create a pre-authorisation, use PREAUTH. To credit the customer, use CREDIT.  
`transactionNotificationURL` | Set the URL to which you would like notification posted when the payment has been completed by the customer
`expiration` | This is the date on which the URL will expire and no longer be useable by the customer
`authenticateTransaction` | set to TRUE to have the transaction authenticated by 3DSecure (this will happen within our hosted page, not via the REST API driven 3DSecure flow)
`dynamicMerchantName` | This sets the merchant name that will appear on the customers card statement, so you can set this to whatever you want to appear there.
`invoiceNumber` | Put your invoice number here. 
`purchaseOrderNumber` | Put the customers Purchase Order number here.
`hostedPaymentPageText` | This sets the text that will appear on the hosted payment page that the customer uses to make payment

The example below creates a URL for a sale payment to send to a customer.

```json YAML
{
  "transactionAmount": {
    "total": "42.42",
    "currency": "EUR"
  },
  "transactionType": "SALE",
  "transactionNotificationURL": "https://Bayswaterlibraryoutstandingpayments.com/Leamas",
  "expiration": "4102358400",
  "authenticateTransaction": true,
  "dynamicMerchantName": "MyWebsite",
  "invoiceNumber": "96126098",
  "purchaseOrderNumber": "123055342",
  "hostedPaymentPageText": "Dear Mr Leamas, Please pay our invoice 123456. Many Thanks, Bayswater Library"
}
```

### Other Payment URL functions

To delete a Payment URL, add your `storeId` and the `transactionId` from the Payment URL creation request response (`ipgTransactionId`) to the /payment-url header, and use the DELETE method to call the /payment-url API.

You can retrieve the data associated with a payment URL by sending a GET to /paymentURL. Add `storeId` and the `transactionId` from the Payment URL creation request response (`ipgTransactionId`) to the /payment-url header to retrieve the details for a specific Payment URL, or set `storeId`, `fromDate` and `toDate` to call a response with all Payment URLs and their details created within a specific time range.
