# SEPA Transactions

To use the SepaSaleTransaction request type, the paymentMethod object is replaced by the sepa object. The sepa object includes all data points required to execute a sepa transaction. Sepa stands for the “Single European Payments Area”, and enables payments to be executed immediately between bank accounts at low cost. This capability is only enabled for domestic payments in Germany at present. Additional countries will be added in the near future.

You must be signed up for SEPA payments and have a mandate reference ID to supply in order to populate the mandate object.

The SEPA PaymentMethod Object is shown below:

```json yaml
{
  "sepa": {
    "iban": "DE34500100600032121604",
    "name": "Alan Turner",
    "country": "DEU",
    "email": "AlanTurner@Bonn.com",
    "mandate": {
      "reference": "3JLCSTIG",
      "url": "https://www.fiserv.com",
      "signatureDate": "2020-10-15",
      "type": "SINGLE"
    }
  }
}
```

