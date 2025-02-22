https://faucetpay.io/api/v1/balance
https://faucetpay.io/api/v1/getbalance
api_keyrequiredA valid API Key from the website
currencyoptionalDefaults to "BTC", can be explicitly set
currencyThe currency you set while making the request
balanceYour balance in satoshis (10^8)
POST API 
https://faucetpay.io/page/user-admin/wallet?modal=deposit
https://faucetpay.io/api/v1/getbalance
READ CAREFULLY:
If you want to pay using FaucetPay.io to your faucet users, you don't need any approval and this page isn't for you. Please refer to  API Documentation  for more information.
Do you want to pay your users with FaucetPay.io? No approval needed! Click on  Faucet Owner Dashboard  to add and set up your faucet.
SPAMMING THIS FORM WILL LEAD TO IMMEDIATE SUSPENSION OF YOUR FAUCETPAY ACCOUNT. BEWARE!

The first part would be to set up the HTML form which is used to take the payment parameters.

Form End-Point (Action): https://faucetpay.io/merchant/webscr

Form Method: POST

Accepted Values for currency1: USD, BTC, ETH, DOGE, LTC, BCH, DASH, DGB, TRX, USDT, FEY, ZEC, BNB, SOL, XRP, MATIC, ADA, TON, XLM, USDC, XMR, TARA

Accepted Values for currency2: "", BTC, ETH, DOGE, LTC, BCH, DASH, DGB, TRX, USDT, FEY, ZEC, BNB, SOL, XRP, MATIC, ADA, TON, XLM, USDC, XMR, TARA
HTML Form:
  <form action="https://faucetpay.io/merchant/webscr" method="post">
    <input type="text" name="merchant_username" value="[YOUR USERNAME]">
    <br>
    <input type="text" name="item_description" value="[ITEM DESCRIPTION]">
    <br>
    <input type="text" name="amount1" value="[AMOUNT]">
    <br>
    <input type="text" name="currency1" value="USD">
    <br>
    <input type="text" name="currency2" value="BTC">
    <br>
    <input type="text" name="custom" value="[IDENTIFICATION VALUE]">
    <br>
    <input type="text" name="callback_url" value="[CALLBACK URL]">
    <br>
    <input type="text" name="success_url" value="[SUCCESS URL]">
    <br>
    <input type="text" name="cancel_url" value="[CANCEL URL]">
    <br>
    <input type="submit" name="submit" value="Make Payment">
  </form>
  Sample Response:
{
  "valid": true,
  "transaction_id": "ed3649c51977f58562faa6986bb86a5432d93ed5",
  "merchant_username": "google",
  "amount1": "10.00000000",
  "currency1": "DOGE",
  "amount2": "10.00000000",
  "currency2": "DOGE",
  "custom": "order1234"
}
Full Callback Code:
<?php

  $token = $_POST['token'];
  
  $payment_info = file_get_contents("https://faucetpay.io/merchant/get-payment/" . $token);
  $payment_info = json_decode($payment_info, true);
  $token_status = $payment_info['valid'];
  
  $merchant_username = $payment_info['merchant_username'];
  $amount1 = $payment_info['amount1'];
  $currency1 = $payment_info['currency1'];
  $amount2 = $payment_info['amount2'];
  $currency2 = $payment_info['currency2'];
  $custom = $payment_info['custom'];
  
  $my_username = "faucetpay";
  
  if ($my_username == $merchant_username && $token_status == true) {
      echo 'Process the payment if the amount1 and currency1 match.';
  } else {
      echo 'Someone is trying to hack you.';
  }
  
