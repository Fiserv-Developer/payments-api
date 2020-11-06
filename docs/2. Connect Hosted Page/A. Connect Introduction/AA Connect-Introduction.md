## Fiserv's Connect Hosted Page

### Adding Payment Capabilities to a Website

The easiest way to add payment capabilities to your website is to implement a HTML form that you post to our Gateway URL.

This type of integration allows you to let our Gateway manage the customer redirections that are required in the checkout process of many payment methods or authentication mechanisms and gives you the option to use secure hosted pages which can reduce the burden of compliance with the Data Security Standard of the Payment Card Industry (PCI DSS).

Application Programming Interface
For steps where no direct consumer interaction is required and no sensitive cardholder data is getting processed, it's best to use our Application Programming Interface (API). Please note that if you plan to collect your customer's card details within your environment and send it to our API, you must ensure that your system components are PCI DSS compliant.

### Key Steps

The following five steps provide an overview of a standard online store integration with links to alternative options where applicable:

<!-- theme: success -->

> Step 1: Checkout Process in Your Webstore (Pre-Authorization or Sale)

<!-- theme: success -->

> Step 2: Receiving the Transaction Result
 
<!-- theme: success -->

> Step 3: Completion When Goods Get Shipped (Post-Authorization)

<!-- theme: success -->

> Step 4: Voiding a Transaction

<!-- theme: success -->

> Step 5: Giving Money Back When Goods Have Been Returned (Refunds and Voids)