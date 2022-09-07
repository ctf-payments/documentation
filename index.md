---
layout: default
---


# CTF Payments - Documentation


This document contains information about REST API for the CTF Payments system.


### Signing form parameters
The algorithm to calculate signature is SHA-512

### Algorithm for signing parameters in the form
1. Sort all the fields available in the form in the alphabetic, ascending order according to the names of parameters name attribute of the input element).

2. Concatenate the values of all the fields in the indicated order.

3. Assign your private key to this sequence of characters (key was send via email after registration).

4. Use SHA-512 hash function.

### Pseudo-code of an algorithm for signing parameters in the form

```js
generate_signature(form, key, algorithm, posId) {

  sortedValues = sortValuesByItsName(form)

  foreach value in sortedValues {
    content = content + value
  }
  content = content + secondKey

  signature = algorithm.apply(content)

  return signature
}
```

### Example form
Example valid generated form for key value: ```a32b60563dcd51bed223bec8eea2403d637e5371f38e93719a8576407a63021b1697bc919b2352f336fd06def3ba308a89fb4900d79d2661731ce972a0164dd0```

```html
<form method="POST" action="http://{your_ctf_instance}/payments">
    <input type="hidden" name="merchantPosId" value="12385843">
    <input type="hidden" name="currencyCode" value="PLN">
    <input type="hidden" name="totalAmount" value="30000">
    <input type="hidden" name="lastName" value="Doe">
    <input type="hidden" name="productName" value="Registration">
    <input type="hidden" name="firstName" value="John">
    <input type="hidden" name="email" value="john.doe@gmail.com">
    <input type="hidden" name="cardCvv" value="123">
    <input type="hidden" name="cardDate" value="12/24">
    <input type="hidden" name="cardNumber" value="4444333322221111">
    <input type="hidden" name="signature" value="830058444036de72ac3b536e12f9175c722d47532b096609bc618619841faeb1865cbaea7746850f2245b891f4c083110651a8cf683146376ded7bf9d3c3b380">
    <button type="submit">Pay</button>
</form>
```


### Fields

| Parameter        | Required | Description                                           |
|:-------------|:---------|:------------------------------------------------------|
| cardCvv               | yes      | Card cvv                                              |
| cardDate | yes      | Card expiration date                                  |
| cardNumber           | yes      | Card number                                           |
| currencyCode           | yes      | Currency code compliant with ISO 4217 (e.g PLN, EUR). |
|      email      | yes      | Buyer's email address                                 |
|       firstName     | yes      | Buyer's first name                                    
|      lastName      | yes      | Buyer's last name                                     |
| productName           | yes      | Name of the product                                   |
|     notifyUrl       | no       | Address for sending notifications                     |
|     totalAmount       | yes      | Total price of the order in pennies                   |
|  merchantPosId          | yes      | Point of sale ID.                                     |
|   signature         | yes      | Calculated signature for all of field values          |
