# Currency Conversion

Use our Currency Conversion services to offer your customers the ability to pay in different currencies, typically their own domestic currency when ordering from overseas.

## Currency Conversion Types

We provide 2 types of Currency Conversion, one in which you'll request the optimal currency and rate from our exchange rate provider, who will then determine which currency to provide a price and exchange rate in (a DCC Exchange Rate Request), and another in which you'll be provided with a list of currencies and prices. You can then choose certain prices and currencies to present to the customer, or customer can select the currency they would like to pay in (a Dynamic Pricing Exchange Rate Request).

## Using Currency Conversion

To use our currency conversion services, use the POST method to /exchange-rates to request a price and exchange rate from our exchange rate providers. As with our other services, this resource is polymorphic. This means you'll need to submit a request type to tell the Exchange Rates resource how to treat your request.

Currency Conversion Type | requestType | Currency Conversion Scenario
---------|----------|---------
Exchange Rate Request |	[DCCExchangeRateRequest](https://docs.fiserv.com/docs/payments/reference/Payments.v1.yaml/paths/~1exchange-rates/post) | Use this Request Type to request a currency, price and exchange rate for a specific transaction, applicable to the customer for which you are requesting a rate. 
Dynamic Pricing Request | [DynamicPricingExchangeRateRequest](https://docs.fiserv.com/docs/payments/reference/Payments.v1.yaml/paths/~1exchange-rates/post) |	Use this requestType to request all available currencies and rates for a transaction.

## Payloads

### Exchange Rate Request

To request a specific rate for a customer, use a POST method against the /exchange-rates resource as per the sample payload below:

```json YAML
{
  "requestType": "DCCExchangeRateRequest",
  "baseAmount": "12.32",
  "bin": "411111"
}
```
The BIN value is important as this tells the exchange rate provider the home currency of the customer's card, which enables them to provide a rate and price tailored to that customer. The BIN value is the first 6 digits of the customer's card number.

The response from the /exchange-rates resource will present as follows:

```json YAML
{
  "clientRequestId": "30dd879c-ee2f-11db-8314-0800200c9a66",
  "apiTraceId": "rrt-0bd552c12342d3448-b-ea-1142-12938318-7",
  "requestTime": 1592912760,
  "exchangeRateDetails": {
    "inquiryRateId": 49150,
    "foreignCurrency": "NOK",
    "foreignAmount": 130.33,
    "exchangeRate": 10.2968,
    "dccOffered": "true",
    "exchangeRateSourceTimestamp": "2015-06-23T13:46:00.000+02:00",
    "expirationTimestamp": "2015-06-23T13:46:00.000+02:00",
    "marginRatePercentage": "3",
    "exchangeRateSourceName": "REUTERS WHOLESALE INTERBANK"
  }
}
```

To use this rate in a transaction, include the DCC object in the /payments POST request as follows, using the inquiryRateId from the response to the /exchange-rates POST request. Use the value and currency from the /exchange-rates POST response in the transactionAmount object.

```json YAML
{
  "requestType": "PaymentCardSaleTransaction",
  "transactionAmount": {
    "total": "130.33",
    "currency": "NOK"
  },
  "paymentMethod": {
    "paymentCard": {
      "number": "5424180279791732",
      "securityCode": "977",
      "expiryDate": {
        "month": "12",
        "year": "24"
      }
    }
  }
  "currencyConversion": {
    "conversionType": "Dcc",
    "inquiryRateId": "49150",
    "dccApplied": true
  }
}
```



