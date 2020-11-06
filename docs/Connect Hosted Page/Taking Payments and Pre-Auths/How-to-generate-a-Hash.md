## How to generate a hash?

If you are using a HTML form to initiate the sale or preauth transaction, your request needs to include a security hash in order to allow us to verify the message integrity.

Hash needs to be calculated using all request parameters in ascending order of the parameter names (parameter hashExtended). When you are using Direct Post, there is also an option where you do not need to know the card details (PAN, CVV and Expiry Date) for the hash calculation. This will be managed with a specific setting performed on your store. Please contact your local support team if you want to enable this feature.

## Creating the hash with all parameters (hashExtended)

Transaction request values used for the hash calculation:

Field | Example 
---------|----------
chargetotal | 13.00
currency | 978
paymentMethod | M
responseFailURL | https://localhost:8643/webshop/response_failure.jsp
responseSuccessURL | https://localhost:8643/webshop/response_success.jsp
sharedsecret | sharedsecret
storename | 10123456789
timezone | Europe/Berlin
transactionNotificationURL | https://localhost:8643/webshop/transactionNotification
txndatetime | 2020:04:17-17:32:41
txntype | sale
 
1. Extended hash needs to be calculated using all request parameters in ascending order of the parameter names

- Join the parameters’ values to one string with pipe separator (use only parameters’ values and not the parameters’ names).

    stringToExtendedHash = 13.00|978|M|https://localhost:8643/webshop/response_failure.jsp|https://localhost:8643/webshop/response_success.jsp|10123456789|Europe/Berlin|https://localhost:8643/webshop/transactionNotification|2020:04:17-17:32:41|sale

- Corresponding hash string does not include ‘sharedsecret’, which has to be used as the secret key for the HMAC instead.

2. Pass the created string to the HMACSHA256 algorithm and using shared secret as a key for calculating the hash value.

    HmacSHA256(stringToExtendedHash, sharedsecret)

3. Use the value returned by the HMACSHA256 algorithm and submit it to our payment gateway in the given form.

    8CVD62a88mwr/Nfc+t+CWB+XG0g5cqmSrN8JhFlQJVM=
    <input type="hidden" name="hashExtended" value=" 8CVD62a88mwr/Nfc+t+CWB+XG0g5cqmSrN8JhFlQJVM="/>

<!--
type: tab
title: PHP Example
-->

<!--
title: "PHP HASH snippet"
lineNumbers: true
-->

```php
<?php
    // Timezeone needs to be set
    date_default_timezone_set('Europe/Berlin');
    $dateTime = date("Y:m:d-H:i:s");

    function getDateTime() {
        global $dateTime;
        return $dateTime;
    }

    /*
        Function that calculates the hash of the following parameters:
        - Store Id
        - Date/Time(see $dateTime above)
        - chargetotal
        - currency (numeric ISO value)
        - shared secret
    */
    function createExtendedHash($chargetotal, $currency) {
        // Please change the store Id to your individual Store ID
        $storeId = "10123456789";
        // NOTE: Please DO NOT hardcode the secret in that script. For example read it from a database.
        $sharedSecret = "sharedsecret";
        $separator = "|";
        
        $stringToHash = $storeId . $separator .  getDateTime() . $separator . $chargetotal . $separator . $currency;

        $hashSHA256 = CryptoJS.HmacSHA384(hashWithAllStrings, sharedSecret);
        $hash = CryptoJS.enc.Base64.stringify($hashSHA256);

        return $hash;
    }
?>
```

<!--
type: tab
title: ASP Example
-->

<!--
title: "ASP HASH snippet"
lineNumbers: true
-->
```net
  <!-- google CryptoJS for HMAC -->
  <script LANGUAGE=JScript RUNAT=Server src="script/cryptoJS/crypto-js.min.js"></script>
  <script LANGUAGE=JScript RUNAT=Server src="script/cryptoJS/enc-base64.min.js"></script>
  <script LANGUAGE=JScript RUNAT=Server>
      var today = new Date();
      var txndatetime = today.formatDate("Y:m:d-H:i:s");

      /*
          Function that calculates the hash of the following parameters:
  - chargetotal
  - currency
  - paymentMethod
  - responseFailURL
  - responseSuccessURL
  - sharedsecret
  - storename
  - timezone
  - transactionNotificationURL
  - txndatetime
  - txntype    
      */

      function createExtendedHash(chargetotal, currency) {
          // Please change the storename to your individual Store Name
          var storename = "10123456789";
          // NOTE: Please DO NOT hardcode the secret in that script. For example read it from a database.
          var stringToExtendedHash = chargetotal|currency|paymentMethod|responseFailURL|responseSuccessURL|storename|timezone|transactionNotificationURL|txndatetime|txntype;

          var hashHMACSHA256 = CryptoJS.HmacSHA256(stringToExtendedHash, sharedSecret);
      var extendedhash = CryptoJS.enc.Base64.stringify(hashHMACSHA256);

          Response.Write(extendedhash);
      }

      function getDateTime() {
          Response.Write(txndatetime);
      }
  </script>
```
<!-- type: tab-end -->