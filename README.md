# Koala Take Home Test - Naresh Siramsetty

## Puzzle Game

Following the Puzzle #1 example, write the execution flow based on the puzzle given.

#### Puzzle #1

```js
doSomething()
  .then(function() {
    return doSomethingElse();
  })
  .then(finalHandler);
```

Answer:

```
doSomething
|-----------------|
                  doSomethingElse(undefined)
                  |------------------|
                                     finalHandler(resultOfDoSomethingElse)
                                     |------------------|
```

#### Puzzle #2

```js
doSomething()
  .then(function() {
    doSomethingElse();
  })
  .then(finalHandler);
```

Answer:

```js
doSomething
|-----------------|
                  doSomethingElse(undefined)
                  |------------------|
                                     finalHandler(undefined)
                                     |------------------|
```

#### Puzzle #3

```js
doSomething()
  .then(doSomethingElse())
  .then(finalHandler);
```

Answer:

```js
doSomething
|-----------------|
                  doSomethingElse(undefined)
                  |------------------|
                                     finalHandler(resultOfDoSomethingElse)
                                     |------------------|
```

#### Puzzle #4

```js
doSomething()
  .then(doSomethingElse)
  .then(finalHandler);
```

Answer:

```js
doSomething
|-----------------|
                  doSomethingElse(resultOfDoSomething)
                  |------------------|
                                     finalHandler(resultOfDoSomethingElse)
                                     |------------------|
```

## Quick challenges

### Shell/CLI
1. What is happening in these Linux commands?  Describe as much as you know about what each symbol means and the effect it will have in the execution of the command, and the named programs.

   a) `(for i in {1..100}; do echo $i; done;) | grep 3 | grep -v 1 | paste -s -d+ - | bc`

   ```
   Output : 748

   Explanantion :

   - (for i in {1..100}; do echo $i; done;) => This will print numbers from 1 to 100
    1
    2
    3
    4
    .
    .
    . 
    100
   - grep 3         => This will filter only numbers which consists '3'
    3
    13
    23
    30
    31
    32
    33
    34
    35
    36
    37
    38
    39
    43
    53
    63
    73
    83
    93
   - grep -v 1      => This will filter only numbers not having '1'
    3
    23
    30
    32
    33
    34
    35
    36
    37
    38
    39
    43
    53
    63
    73
    83
    93
   - paste -s -d+ - => This will concatenate all lines by replacing all new lines with '+', '-' stands for reading standard input instead of files
   3+23+30+32+33+34+35+36+37+38+39+43+53+63+73+83+93
   - bc             => Calculates the Expression
   748
   ```
   
   b) `[ ! -f /var/lock/myscript.lock ] && touch /var/lock/myscript.lock && (yum -y update >> /var/log/mylog.log 2>&1; ) && rm -f /var/lock/myscript.lock`

   ```
   Output : dependes on the file system and existence of directories and return codes of each statement

   Explanantion: 
   The above command is chian of commands/actions and return code (0 for success, other values for fail) of every action will determine whether the next action/command will be executed or not
   [ ! -f /var/lock/myscript.lock ] => This will check if /var/lock/myscript.lock file is present or not
   touch /var/lock/myscript.lock    => If the /var/lock/myscript.lock is not present, it will create /var/lock/myscript.lock, this will not execute if the file is present already
   (yum -y update >> /var/log/mylog.log 2>&1; ) => Updates yum packages where output is sent to /var/log/mylog.log and errors to stdout
   rm -f /var/lock/myscript.lock => forcefully removes /var/lock/myscript.lock file
   ```

2. There is a directory, containing a large tree of subdirectories and files.  Scattered throughout these files are Australian phone numbers, and we want to harvest them – we want to end up with a simple list of the phone numbers.

   a) Write a regular expression to match Australian phone numbers.  The numbers will be in a mixture of the forms 02xxxxxxxx and +612xxxxxxxx, and there will also be the common usage of hyphens, spaces, and parentheses, so all of those common possibilities must be supported. eg. 02 xxxx xxxx, (02) xxxx-xxxx, +61 2 xxxx xxxx, +61 02 xxxxxxxx, +61 (0)2 xxxx-xxxx

   Solution

   ```js
   ^\({0,1}((0|\+61)(\ |-)?(0)?(\(0\))?(2|4|3|7|8)){0,1}\){0,1}(\ |-){0,1}[0-9]{2}(\ |-){0,1}[0-9]{2}(\ |-){0,1}[0-9]{1}(\ |-){0,1}[0-9]{3}$
   ```

   b) Write an example phone number, for each specific phone number format that your regex would match.

   ```js
    +61 2 5746 1234
    +61-2-5746-1234
    +61 (0)2 57461234
    +61 02 57461234
    +61257461234
    02-5746-1234
    02 5746 1234
    0257461234
    (02)57461234
    (02) 57461234
    +61257461234
    0403 792427
    0403792427
    04 0379 2427
    0403-792-427
    0403 792 427
    04 0379 2427
    +61 403 792427
    +61-403792427
   ```

   c) Imagine that the full path of the directory is
   > /var/www/site1/uploads/phnumbers/
   
   Write a single-line or simple command that you could run from the shell (ideally Linux), to apply this regular expression to the files in the directory tree, and result in the simple list of phone numbers, one phone number per line.

   Solution

   ```bash
   find /var/www/site1/uploads/phnumbers/ -type f | xargs egrep "^\({0,1}((0|\+61)(\ |-)?(0)?(\(0\))?(2|4|3|7|8)){0,1}\){0,1}(\ |-){0,1}[0-9]{2}(\ |-){0,1}[0-9]{2}(\ |-){0,1}[0-9]{1}(\ |-){0,1}[0-9]{3}$"
   ```


### Software development

1.	Write a function/method/subroutine to determine if a string starts with an upper-case letter A-Z, *without* using any sort of isUpper()/isLower() or toUpper()/toLower() etc helper function provided by the language.  Your choice of language.

Solution

```php
<?php $str= 'Naresh';
 $pattern = "/^[A-Z](.*)$/";
 echo preg_match($pattern, $str);
 // If output is 1, Starts with Capital letter , else not.
 ```

2. Consider this statement:
   ```
   $a = implode(',',array_map(function($b,$c) {
     return str_replace(array('-','_',','), '', $b) . "x{$c}";
   },array_keys($d),$d));
   ```
   a) what language is it written in?

   ```php
   PHP
   ```
   
   b) at the point when this statement is executed, which (if any) pre-existing variable(s) does this statement use or rely on?

   ```php
   $d
   ```
   
   c) after this statement has executed, which (if any) variable(s) have been initialised or modified by the statement?

   ```php
   $a
   ```
   
   d) taking your answer from b), give simple example value(s) for each used/relied-upon variable.  There is not a single correct answer, rather you should make an educated guess based on your interpretation of what the statement is doing.

   ```php
   $d=array(
    "Naresh-Test,345"=> "Siramsetty",
    "Naresh_Test-567,9"=> "Siramsetty");
    ```
   
   e) what would be the output or effect of the statement, if you used your example value(s) from d) ?

   ```php
   // Since we are not using any print statement, there will be no output. But if we print $a which is returned from implode, you will get below output.
   NareshTest345xSiramsetty,NareshTest5679xSiramsetty
   ```
   
   f) describe what is happening in this statement

   ```php
   // There are 4 major funtions working here
   // array_map => takes two arrays as parameters, and executes the callback/handler in the first paramater and returns another array with changes executed in callback/handler. Here it took one array which is list of keys of $d and another array which is $d itself
   // array_keys => returns keys in an array as array
   // str_replace => replaces any occurences of provided strings in array (first param), with second param in the actual given string ( third parameter)
   // impolde => concatenates elements of array with the string provided
   ```

3. Write a function in Go which returns the top two most frequent numbers from a list, in order of frequency first. For example:
   ```
   Given the list [1, 3, 3, 5, 5, 6, 6, 5, 3, 3]
   It should return [3, 5]
   ```

   Solution:

   ```js
   // I have not worked on Go, so solving this in Javascript

    let data =[1, 3, 3, 5, 5, 6, 6, 5, 3, 3];
    const getFrequentNumbers = function(finalOutput, currentValue){
        let index = finalOutput.findIndex(val=> val.key === currentValue);
        if(index >= 0){
            finalOutput[index].count +=1;
        } else{
            finalOutput.push({
                key: currentValue,
                count:1
            });
        }
        return finalOutput;
    };
    let frequentEntries = data.reduce(getFrequentNumbers,[]);
    let result = frequentEntries.sort((a,b)=> b.count - a.count).splice(0,2).map((x)=>x.key);
    console.log(result);
    ```

4. Go to one of the Koala websites (au.koala.com, jp.koala.com)
   a) can you find our Shopify Storefront API key?  If so, what is it?

   Answer
   ```js
   // I could see the below configuration in checkout page, and there are many tokens..not sure which one is the API key
   // I belive this one 
   
   // ShopifyPay.apiToken = "bWlPeHN0NnpPZTNzdUVlU1Q2L0gwa0FxWGZUVXpyM3E4N09SNUM2UllPQ1g5cGkrOUNKRWFRSnMxY3NXYVZWNS0tSHcyOFFUUWJmc0lJU0V5czRXS2NKQT09--5d7f2001f5956fa161edce4899c562e1007417f2";

    Shopify = window.Shopify || {};
    ShopifyExperiments = window.ShopifyExperiments || {};
    ShopifyPay = window.ShopifyPay || {};

    if (window.opener) {
      window.opener.postMessage(JSON.stringify({"current_checkout_page": "\/checkout\/contact_information"}), "*");
    }

    Shopify.Checkout = Shopify.Checkout || {};
    Shopify.Checkout.Autocomplete = true;
    Shopify.Checkout.apiHost = "koala-mattress.myshopify.com";
    Shopify.Checkout.assetsPath = "\/\/cdn.shopify.com\/shopifycloud\/shopify";
    Shopify.Checkout.DefaultAddressFormat = "{firstName}{lastName}_{company}_{address1}_{address2}_{city}_{country}{province}{zip}_{phone}";
    Shopify.Checkout.geolocatedAddress = {"lat":-33.85910000000001,"lng":151.2002};
    Shopify.Checkout.i18n = {"orders":{"order_updates":{"title":"Order updates"},"complete_order":"Complete order","pay_now":"Pay now"},"shipping_line":{"pickup_in_store_label":"Pickup","no_pickup_location":"Your order isn't available for pickup. Enter a shipping address.","shipping_label":"Shipping","shipping_default_value":"Calculated at next step","shipping_free_value":"Free"},"continue_button":{"continue_to_shipping_method":"Continue to shipping","continue_to_payment_method":"Continue to payment"},"qr_code":{"title":"Download the Shop app","subtitle":"Scan the code with your phone's camera.","send_link_to_phone":"Or send a link to your phone"}};
    Shopify.Checkout.locale = "en";
    Shopify.Checkout.normalizedLocale = "en";
    Shopify.Checkout.page = "show";
    Shopify.Checkout.releaseStage = "production";
    Shopify.Checkout.deployStage = "production";
    Shopify.Checkout.requiresShipping = true;
    Shopify.Checkout.step = "contact_information";
    Shopify.Checkout.token = "6fe973f65ddc6f7885e7598107c8e609";
    Shopify.Checkout.currency = "AUD";
    Shopify.Checkout.estimatedPrice = 1250.00;
    Shopify.Checkout.applePayConfig = null;
    Shopify.Checkout.dynamicCheckoutPaymentInstrumentsConfig = {"paymentInstruments":{"accessToken":"e1bdb956634c55ed5afb840a317f06b6","amazonPayConfig":null,"applePayConfig":null,"checkoutConfig":{"domain":"koala-mattress.myshopify.com","shopId":8101077},"shopifyPayConfig":null,"currency":"AUD","googlePayConfig":null,"locale":"en","paypalConfig":{"domain":"koala-mattress.myshopify.com","environment":"production","merchantId":"P7B6RU4JWFUKC","buttonVersion":"v3","venmoSupported":false,"locale":"en_US","shopId":8101077},"offsiteConfigs":null,"supportsDiscounts":true,"supportsGiftCards":true,"checkoutDisabled":false}};
    ShopifyExperiments.AutocompleteServiceApiHost = "https:\/\/autocomplete-service.shopifycloud.com";
    ShopifyExperiments.googleAutocompleteAllCountries = false;

    ShopifyPay.enabled = false;
    ShopifyPay.apiHost = "shop.app\/pay";
    ShopifyPay.apiToken = "bWlPeHN0NnpPZTNzdUVlU1Q2L0gwa0FxWGZUVXpyM3E4N09SNUM2UllPQ1g5cGkrOUNKRWFRSnMxY3NXYVZWNS0tSHcyOFFUUWJmc0lJU0V5czRXS2NKQT09--5d7f2001f5956fa161edce4899c562e1007417f2";
    ShopifyPay.transactionParams = null;
    ShopifyPay.signUpButtonEnabled = false;
  ```
   b) based on what you found in a), is this an acceptable state-of-affairs for a modern eCommerce website?  Why or why not?

   Answer

   ```js
   Since we are integrating with shopify from web page directly, it was needed there. But in an ideal scenario, I would expect the coummnication to shopify should be via a middleware API microservice where we could store all the secret information and prevent it from being exposed to users
   ```

5. What will this PHP statement print?
   ```
   echo implode(' = ',['9 times 5','4' + '5']);
   ```

   Solution

   ```
   Output : 9 times 5 = 9

   // '4' + '5' is converted to 9
   ```

6. Using the students array below, write a javascript function to return an object containing:

  - The name of the class as the key.
  - The total attended lessons for each class.
  - The average amount of attended lessons for each class.

`students.json`

```json
{
  "students": [
    {
      "name": "Lulu Gearside",
      "class": "art",
      "attended": 35
    },
    {
      "name": "Matthew Milham",
      "class": "art",
      "attended": 11
    },
    {
      "name": "Dany Dufner",
      "class": "biology",
      "attended": 12
    },
    {
      "name": "Jeremy Doyle",
      "class": "biology",
      "attended": 3
    },
    {
      "name": "Tim O'Connor",
      "class": "biology",
      "attended": 10
    },
    {
      "name": "Charlie Wang",
      "class": "french",
      "attended": 12
    }
  ]
}
```

Expected output:

```js
{
  "art": {
    "total": 46,
    "average": 23,
  },
  "biology": {
    "total": 25,
    "average": 8,
  },
  "french": {
    "total": 12,
    "average": 12,
  },
}
```

Solution:

```js
let data = {
    "students": [
        {
            "name": "Lulu Gearside",
            "class": "art",
            "attended": 35
        },
        {
            "name": "Matthew Milham",
            "class": "art",
            "attended": 11
        },
        {
            "name": "Dany Dufner",
            "class": "biology",
            "attended": 12
        },
        {
            "name": "Jeremy Doyle",
            "class": "biology",
            "attended": 3
        },
        {
            "name": "Tim O'Connor",
            "class": "biology",
            "attended": 10
        },
        {
            "name": "Charlie Wang",
            "class": "french",
            "attended": 12
        }
    ]
};
const getSumAndAverage = function(finalOutput, currentValue){
    let key = currentValue.class;
    if(finalOutput.hasOwnProperty(key)){
        let existingValue = {...finalOutput[key]};
        finalOutput[key] = {
            sum : currentValue.attended+existingValue.sum,
            avarage: (currentValue.attended+existingValue.sum)/(existingValue.count+1),
            count: existingValue.count+1
        };
    } else{
        finalOutput[key] = {
            sum : currentValue.attended,
            avarage: currentValue.attended,
            count: 1,
        };
    }
    return finalOutput;
};

let results = data.students.reduce(getSumAndAverage,{});
// We can remove the unwanted key "count" from the output if really needed or keep it.
Object.keys(results).forEach((key)=>{
    delete results[key].count;
});
console.log(results);

```
