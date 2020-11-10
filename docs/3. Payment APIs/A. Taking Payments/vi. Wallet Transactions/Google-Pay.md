# Google Pay

## 1. Merchant Boarding

Please reach out to your local Integration Support team for getting your account enabled. The Integration team will share a Sample Application to generate Google Encrypted payloads.

## 2. Prerequisites to build and run the sample Application

Developers wishing to use the Fiserv Google Pay sample application will need the following software and hardware:

- Google Play Services version 18.0.0 
- A physical device or an emulator to use for developing and testing. Google Play services can only be installed on an emulator with an AVD that runs Google APIs platform based on Android 4.4 or higher.
- The latest version of Android Studio. This includes:
  - The latest version of the AndroidSDK, including the SDK Tools component. The SDK is available from the
  - Android SDK Manager
  - JavaJRE(JDKfordevelopment) as per Android SDK requirements

Your project should be able to compile against Android 4.4 (KITKAT) or higher.

For more details, please refer https://developers.google.com/pay/api/android/guides/setup

## 3. Changes to be made in the Application 

The following parameters need to be defined 

- Merchant ID
- Merchant Token
- APIKey
- APISecret

Define the Fiserv Object Parameters: Parameters must be updated in the following files: 

- Constants.java
- EnvData.java

Update the Constants.java file with the Merchant ID and Gateway Tokenization parameters. Note that:

- The Merchant ID will be shared by the Integration team and
- The Gateway Tokenization parameter defaults to ‘firstdata’. 

In the EnvData.java file, set the following environment variables, which will be shared by the Integration Team:

- APIKey
- Token 
- APISecret 
 
envMap.put("CERT", new EnvPropertiesImpl( "CERT",

 "https://cert.api.firstdata.com/gateway/v1/payments",

 "---------", "----------", "-----------"));

gatewayMerchantId and the APIGEE credentials will be provided by the Integration Team

## 4. Credit Card for Testing

Note that for testing purposes; the credit card information used in the app must be attached to an active account. The standard test cards will not be validated by Google and will fail in processing, our integration team will provide google pay-specific test cards.

## 5. Execute Authorize and Purchase Request

The Authorization parameter, required as part of the Header for a Fiserv API transaction, needs to be created. Construct the data param by appending the following parameters in the order shown:

- apikey – the developer’s API key
- nonce – A secure random number
- timestamp – Epoch timestamp in milliseconds
- token – the Merchant Token 
- payload – The actual body content passed as the POST request 

## 6. Google Pay APK Installation to Device

- Once the downloaded code for the sample app is built successfully in Android Studio, build the APK and install it on your device. 
- Once the APK is installed, select the Open option to access the application. 
- The sample application should now be installed on the test device (nonPROD environment), and the response from a payment processed in the First Data nonPROD environment/Google test environment available

## 7. Example Payload

Sample Google Pay Request within the `walletPaymentMethod` object for a `WalletPreAuthTransaction`:

```json yaml
{

  "requestType": "WalletPreAuthTransaction",

  "walletPaymentMethod": {

    "walletType": "EncryptedGooglePayWalletPaymentMethod",

    "encryptedGooglePay": {

      "data": {

        "encryptedMessage": "8nxjB9mr2tWZeDRQRcGN91UUnb7AioGp3oRo8kmQ6lyvJZiqD7PJlbRCYElNqUmr6Z8zK7b2gO9MKOjpnTCqH0qAe2vuIlwNXB60M2Lh7Qfl3bVgWzwF/FfFcenVW381hoItYi8AjWnWoydz1XMTEv2qhqUG03mEnRXdMyDyk6KKZXoW8Qc0p1F1thbxxu8weU8CZbZsWGGTjB42cilIqLVbribcOAG8Oas1AcgefFsu2hwp4gdSuOg7wmeSV7XKsGQzzVy85qtjuqET2XYzJE3K/Wh9QKkhu5P9Ms5s1+Smr2IjRyidqQa88SxQplrVoo9+PvT0bxFcMspBmO3pLkuaZSUBy++dL2fefcxNJvGCFfFhdxW9DojuuQxgpeu7RAQUsGLyFmr/4ZfBxt882xTmpX9MRx5CAudl9qUgBfKdwWwMX35qSbDTm1ju5XXzNh94VebjD3bB9Zj8qgbmUOr/+6OQLhoFJyBCXgx3EEH8hBwNVFrss/SLwQvFhZh62eO6lOtnmbOtP1yTDDVqGDBfai5SwAmM+KTcc9SGv/xDC+cWe8ck+aCBkG4HoRPapUVMZ3JIgV7yzTsVLJE\\u003d",

        "ephemeralPublicKey": "BGH3fRFdoAobYrAlxnZOCYzkH84Cna92IZxtgsU36CMDaqSaDYb9/LsY8qw+vMtlBnwsUg/YVMOeeKp+qDkOWb4\\u003d",

        "tag": "nvmOUNpnOTZULLhMxT/hWCHzH/4f7gGpfvQgwl3p8ng\\u003d"

      },

      "signature": "MEUCIFWTRWUZAOM5nfJC79FtJm56olnbwG4H5uWWxAUWAquiAiEA24j/BcOroeISsdJzYsyoVi8wzu4tnmKw+jdsGfuvPko=",

      "version": "ECv1"

    }

  },

  "transactionAmount": {

    "total": "1200",

    "currency": "USD"

  }

}
```