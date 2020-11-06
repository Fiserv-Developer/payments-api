## Tokens

Tokens are useful when storing the details of a customer’s payment instruments to secure and speed up checkout later. When you collect a customer's payment methods, you can request a token, which you can then use in place of a customer’s card details to execute future payments. You will need to store the token on your platform, mapped to the customer’s account details. 

To create a token for a customer’s payment instrument, see the Tokens section [here](url).

Tokens are useful as they reduce the risk of attacks on your and our payments systems being successful. Tokenisation also reduces the PCI Requirement for you, as it means you don't need to store card details within your systems, which should reduce your costs associated with PCI Compliance.  

To generate a Token for future use at the same time as submitting a payment, use the createToken object to set a Token up for multiple use (set reusable to True). In addition, you can create your own token for the merchant, send it to us within the object, and set rules as to whether you want to decline payments with duplicate payment details.

```json yaml
{
  },
  "createToken": {
    "value": optional - define your own token,
    "reusable": true / false,
    "declineDuplicates": true / false
  }
}
```

Once the token is created and you have stored it against the customer’s account detail, you can use it to execute payments for the customer. To use a tokenised payment instrument, use the relevant PaymentToken* requestTypes. If you supply a token value, we will store that, otherwise we'll generate a token value and pass it back to you in the response.

When a customer is checking out, and you've previously tokenised their payment details (and you’ve stored the token with the customer’s account record in your systems), you can request token details to enable the customer to confirm they want to pay with the stored payment instrument. To do this, use the GET call against [this end point](https://docs.fiserv.com/docs/payments/reference/Payments.v1.yaml/paths/~1payment-tokens~1%7Btoken-id%7D/get), providing the `tokenid` to receive a [PaymentTokenDetails](https://docs.fiserv.com/docs/payments/reference/Payments.v1.yaml/components/schemas/PaymentTokenDetails) response. Our suggestion is you use the `last4` value and `brand` to enable the customer to correctly identify their payment instrument.

You can then use the Payment Token in the relevant Primary Payment request type to execute the customer payment. To Update or Delete customer payment tokens, see the Token section [here](url).

