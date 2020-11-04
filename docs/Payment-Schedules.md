# Payment Schedules

## /payment-schedule

The payment schedules API allows you to create and manage a recurring payment schedule. It enables you to create a gateway payment schedule and to view, cancel and to update existing gateway payment schedule.
Schedule creation
You can use this request type to create recurring payments for new or existing consumers. For this walkthrough, we’re going to use PaymentMethodPaymentSchedulesRequest which you can use to schedule recurring payments for new consumers. Different recurring payment scenarios require specific requestType values. The table below explains the situations in which you might want to use the different requestType values. The technical detail for each of these requestTypes is included in the Schema section of the API explorer. Click the requestType to navigate to the schema.
requestType	Payment Scenario
PaymentMethodPaymentSchedulesRequest	You can create the  recurring payment, and set the future date for the initial payment
ReferencedOrderPaymentSchedulesRequest	You can reference already existing order (using the OrderID) and schedule recurring payment


The mandatory parameters in Schedule creation are Content-Type, Client-Request-Id, Api-Key, Timestamp and order-id ( only if using ReferencedOrderPaymentSchedulesRequest) , startDate, transactionAmount and frequency .
The following mandatory parameters are specific to the payment schedule API
Parameter	Description
frequency 	You can define the numerical value of the rate of frequency and choose the appropriate unit (e.g. DAY, WEEK, MONTH, YEAR)
startDate	You can select the start date of the mandate which is the date of the first scheduled payment transaction
transactionAmount	This allows you to define the currency using ISO 4217 currency code and the total amount of the transaction. You can also use the additional subcomponents and define the subtotal, VAT, tax, shipping, cashback and tip amount (note: Sub component values must add up to total amount.)

The paymentMethod object allows you to create recurring payments using the following payment methods: PAYMENT_CARD,PAYMENT_TOKEN,SEPA,SOFORT and WALLET
Schedule inquiry
You can use this request type (Click the requestType to navigate to the schema.) to view an existing gateway payment schedule.  Besides the mandatory parameters of Schedule creation, you will also have to provide the order-id parameter.(
Schedule cancel
You can use this request type(Click the requestType to navigate to the schema.) to cancel an existing gateway payment schedule. Besides the mandatory parameters of Schedule creation, you will also have to provide the order-id parameter.
Schedule Update 
You can use this request type (Click the requestType to navigate to the schema.) to update an existing gateway payment schedule. The mandatory parameters are the same as in Schedule creation and the available request types are PaymentMethodPaymentSchedulesRequest and ReferencedOrderPaymentSchedulesRequest



