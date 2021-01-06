# Overview

Fiserv's Payment.js allows merchants working with various Fiserv APIs and gateways to tokenize payment credentials for later transactions without collecting, processing, or otherwise being able to view those payment credentials in their untokenized form, thus lowering their PCI compliance requirements.

Payment.js accomplishes this by injecting iframes into a parent form where customers can enter their data as though it were a normal form field styled however the merchant sees fit.

In form submission the client library, loaded into the parent window, sends one of the iframes a clientToken (for authentication with the service) and a RSA public key (asymmetric key pair). This iframe then collects the data hidden in the other iframes and encrypts the card number, expiration date, and cvv (the other fields are transferred without data layer encryption due to RSA message limits). This iframe then makes an API call to the Payment.js service for tokenization.

Assuming the customer is using a browser with modern cross-origin security controls, and these controls are not compromised by a browser defect, it will not be possible for non-Payment.js code to steal the data hidden in these iframes as the card number, expiration date, and cvv in particular never escape into the parent window in an unencrypted form.

When the tokenization request is sent out, only the already encrypted data will appear in the browser's network log.

The following gateways are supported:
- IPG
- Payeezy
- CardConnect (CardPointe/CardSecure)
- Bluepay

> ### Note
> The merchant's gateway credentials are never sent to the browser (encrypted or otherwise); The Payment.js client library utilizes a "clientToken" to associate the tokenization api call sent from the browser with credentials passed directly from merchant server to Payment.js server

## Additional Security Settings Recommended

The following recommendations are to limit potential for fraudulent activity on your hosted payment page.

Recommendations
- Enable Re-Captcha
- Authentication to payment page
- Limit response back to the browsers/customer
- Enable appropriate Fraud Tools for your business type or payment flow
