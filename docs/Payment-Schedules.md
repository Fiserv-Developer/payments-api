# Payment Schedules

## /payment-schedule

The payment schedules API allows you to create and manage a recurring payment schedule for an individual consumer. This API covers creation, update, cancellation and reporting capabilities. As an example, you'd use this API to setup monthly subscription payments against a payment method if you are providing ongoing products or services to a customer. 

The payment schedules API uses the same header and most of the same objects as the /payments API, so if you've already implemented for that API, this should be easy to extend to.

### Creating a Payment Schedule

You can find the PaymentSchedulesRequest schema [here](https://docs.fiserv.com/docs/payments/reference/Payments.v1.yaml/components/schemas/PaymentSchedulesRequest).

To create a payment schedule when the customer is checking out, capture the customers payment details for whatever payment method they are using as you would when processing a normal, one-off payment. As well as the standard payments information, you need to add further attributes to successfully setup the payment schedule:


Attribute | Explanation 
---------|----------
startDate | The start date indicates the date of the first payment. This can be future dated. This value sets the baseline for the calculation of the next payment, dependent on the `every` and `unit` values defined in the `frequency` object. 
numberofPayments | This defines the total number of instances of taking a payment in the schedule. 
maximumFailures | This sets the retry limit for the payment. If it fails once, you can set our platform to auto-retry up to 999 times.
every | Sets the increment for the execution of the recurring payments, in partnership with the value in the `unit` field
unit | Sets the frequency - either DAY, WEEK, MONTH or YEAR

So, if you set the `startDate` as the 1st December 2020, the `numberofPayments` to 12, the `every` to 1 and the `unit` to MONTH, the last payment will be taken on 1st November 2021, with a payment taken on the 1st of every month in between.


```json YAML
{
  "requestType": "PaymentMethodPaymentSchedulesRequest",
  "startDate": "2020-11-16",
  "numberOfPayments": 10,
  "maximumFailures": 1,
  "invoiceNumber": "96126098",
  "purchaseOrderNumber": "123055342",
  "transactionOrigin": "ECOM",
  "dynamicMerchantName": "MyWebsite",
  "frequency": {
    "every": 1,
    "unit": "MONTH"
  },
  "paymentMethod": {
    "paymentCard": {
      "number": "5426064000424979",
      "expiryDate": {
        "month": "12",
        "year": "20"
      },
      "securityCode": "147",
      "cardFunction": "CREDIT"
    }
  },
  "transactionAmount": {
    "total": "25.24",
    "currency": "GBP"
  },
  "clientLocale": {
    "language": "EN",
    "country": "UK"
  }
}
```

You can also use payment schedules to set single future-dated payments, by setting the numberofPayments to 1 and the startDate to a future date. As long as the `every` value is set to 1, this is the same as setting a single future-dated payment.

### Using a reference order to create a payment schedule

To create a payment schedule using a single payment already submitted, submit the payload as per the below to the /payment-schedule API, but include the `referenceOrderId` attribute as the orderId from the response of the original payment. This is useful when a customer buys something as a single purchase, but then elects to continue their access as a subscription to your product or service. 

``` json YAML
{
  "requestType": "ReferencedOrderPaymentSchedulesRequest",
  "referencedOrderId": "R-f9c2c198-b7cc-491a-a711-93d22fd0e589",
  "startDate": "2020-11-16",
  "numberOfPayments": 10,
  "maximumFailures": 1,
  "invoiceNumber": "96126098",
  "purchaseOrderNumber": "123055342",
  "transactionOrigin": "ECOM",
  "dynamicMerchantName": "MyWebsite",
  "frequency": {
    "every": 1,
    "unit": "MONTH"
  },
  "transactionAmount": {
    "total": "25.24",
    "currency": "GBP"
  },
  "clientLocale": {
    "language": "EN",
    "country": "USA"
  }
}
```

### Updating a Payment Schedule

To update a payment schedule, use PATCH to make a call to /payment-schedules/{order-id} where the {order-id} value is the `orderId` value from the response of the original PaymentSchedulesRequest POST. Use the same payload structure as for setting up the payment schedule, but identify the payment schedule for update via specifying the `orderId` in the response from the original PaymentSchedulesRequest in /payment-schedules/{order-id}. Make updates to the Payment Schedule, Payment Method or Transaction Amount as necessary in the PATCH call payload.

### Cancelling a Payment Schedule

Cancel a Payment Schedule by making a DELETE call to /payment-schedules/{order-id} where the {order-id} value is the `orderId` value from the response of the original PaymentSchedulesRequest POST.

### Retrieving Payment Schedule data

Retrieve Payment Schedule data by making a GET call to /payment-schedules/{order-id} where the {order-id} value is the `orderId` value from the response of the original PaymentSchedulesRequest POST. 


