API for GDAX, A service provided by coinbase. (An Unofficial API)


Quick Use Sections:
* [Installation](#installation)
* [Usage Examples](#usage-examples)
* [Api Summary](#api-summary)


The API methods in the Api class start at "getAccounts". Each method has
basic usage information along with a reference link to the GDAX Api
documentaiton on the internet.


The ApiInterface class has a cleaner represenation of methods and 
parameters without comments.


### License
MIT - MIT License
File: [LICENSE](LICENSE)

### Installation
###### Composer
```
composer require mrteye/gdax
```

## Usage Examples
The following three examples show how to use the mrteye\GDAX api. From basic to
Advanced usage examples: public acces, private access, and extending the API
into your own class.

### Example #1 Basic. Public access, no authentication required.
```php
<?php
use mrteye\Gdax\Api as Api;
use mrteye\Gdax\Auth as Auth;

// Get the GDAX API and start making calls.
$gdax = new Gdax('https://api-public.sandbox.gdax.com');



// Example usage of public calls.
$products = $gdax->getProducts();
$productOrderBook = $gdax->getProductOrderBook('BTC-USD', $param = [
    'level' => 1
]);
$productTrades = $gdax->getProductTrades($productId, $param = [
    'before' => 1,
    'limit' => 100
]);
```

### Example #2 Basic. Private access, authentication is required. 
```php
use mrteye\Gdax\Api as Api;
use mrteye\Gdax\Auth as Auth;

// Create authentication credentials on the GDAX site for Private API calls.
$key = 'web';  
$secret = 'web';
$pass = 'web';

// Create a GDAX object; the GDAX time API is optional.
$gdax_time_api = 'https://api-public.sandbox.gdax.com/time';
$auth = new Auth($key, $secret, $pass, $gdax_time_api);

// Get the GDAX API and start making calls.
$api_url = 'https://api-public.sandbox.gdax.com"';
$gdax = new Gdax($api_url, $auth);


// Usage examples with some private calls.
$accounts = $gdax->getAccounts();

$account = $gdax->getAccount($accounts[0]->id);

$order = $gdax->createOrder([
    // Common Order Parameters
    'type' => 'limit',
    'side' => 'buy',
    'product_id' => 'BTC-USD',
    // Limit Order Parameters
    'price' => ".01",
    'size' => ".01"
]);

$orders = $gdax->getOrders($param = [
    'status' => 'open',
    'product_id' => '',
    'before' => 0,
    'after' => 1000,
    'limit' => 100
]);

$uuids = $gdax->cancelOrder($orders[0]->id);

$uuids = $gdax->cancelAllOrders($param = [
    'product_id' => 'BTC-USD'
]);
```


### Example #3 Advanced. Extend the Api class.
```php
<?php
use mrteye\Gdax\Api as Api;
use mrteye\Gdax\Auth as Auth;

class AppGdaxApi extends Api {
  function __construct($private = false) {

    // Create an authentication object if necessary.
    $auth = false;
    if ($private) {
      // TODO: Reminder; define values for key, secret, pass and gdax_time_api.
      // These values should be stored in an external file or other source.
      // - OR - you could simply hard code them here.
      $auth = new Auth($key, $secret, $pass, $gdax_time_api);
    }

    // Load the Gdax API.
    parent::__construct($api_url, $auth);
  }

  function setDebug($bool) {
    $this->_setDebug($bool);
  }

  function getError() {
    return (empty($this->_error) ? []: $this->_error);
  }


}

// Example usage of the AppGdaxApi class
$gdax = new AppGdaxApi(true);

// Turn on detailed error logging.
$gdax->setDebug(true);

// Get all accounts and products.
$accounts = $gdax->getAccounts();
$products = $gdax->getProducts();

// Get any errors if they exists.
$errors = $gdax->getError();
```

## API Summary
All $param properties are associate arrays with either API parameters
or pagination parameters or both.  The parameters for each method are documented 
in the Api class file, the ApiInterface file, and on the internet at the provided url.

###### Get accounts. https://docs.gdax.com/#list-accounts
```
public function getAccounts()
```

###### Get an account. https://docs.gdax.com/#get-an-account
```
public function getAccount($accountId)
```

###### Get account activity. https://docs.gdax.com/#get-account-history
```
This API is paginated.
public function getAccountHistory($accountId, $param)
```

###### Get order holds. https://docs.gdax.com/#get-holds
```
This API is paginated.
public function getAccountHolds($accountId, $param)
```

###### Place a new order. https://docs.gdax.com/#place-a-new-order
```
public function createOrder($param)
```

###### Cancel an order. https://docs.gdax.com/#cancel-an-order
```
public function cancelOrder($orderId)
```

###### Cancel all orders. https://docs.gdax.com/#cancel-all
```
public function cancelAllOrders($param)
```

###### Get open and unsettled orders. https://docs.gdax.com/#list-orders
```
This API is paginated.
public function getOrders($param)
```

###### Get a GDAX order. https://docs.gdax.com/#get-an-order
```
public function getOrder($orderId)
```

###### Get a list of fills. https://docs.gdax.com/#list-fills
```
This API is paginated.
public function getFills($param)
```

###### Get fundings. https://docs.gdax.com/#funding
```
This API is paginated.
public function getFundings($param)
```

###### Repay a funding record. https://docs.gdax.com/#repay
```
public function repay($param)
```

###### Transfer funds between a profile and a margin profile. https://docs.gdax.com/#margin-transfer
```
public function marginTransfer($param)
```

###### Get an overview of your profile. https://docs.gdax.com/#position
```
public function getPosition()
```

###### Close Position TODO: Identify this API. https://docs.gdax.com/#close
```
public function closePosition($param)
```

###### Deposit funds from a payment method. https://docs.gdax.com/#payment-method
```
public function deposit($param)
```

###### Deposit funds from a coinbase account. https://docs.gdax.com/#coinbase
```
public function depositCoinbase($param)
```

###### Widthdraw funds to a payment method. https://docs.gdax.com/#payment-method53
```
public function withdraw($param)
```

###### Withdraw funds to a coinbase account. https://docs.gdax.com/#coinbase54
```
public function withdrawCoinbase($param)
```

###### Withdraw funds to a crypto address. https://docs.gdax.com/#crypto
```
public function withdrawCrypto($param)
```

###### Get a list of your payment methods. https://docs.gdax.com/#payment-methods
```
public function getPaymentMethods()
```

###### Get a list of your coinbase accounts. https://docs.gdax.com/#list-accounts59
```
public function getCoinbaseAccounts()
```

###### Create a report. https://docs.gdax.com/#create-a-new-report
```
public function createReport($param)
```

###### Get report status. https://docs.gdax.com/#get-report-status
```
public function getReportStatus($reportId)
```

###### Get 30 day trailing volume. https://docs.gdax.com/#trailing-volume
```
public function getTrailingVolume()
```

###### Get available currency trading pairs. https://docs.gdax.com/#get-products
```
public function getProducts()
```

###### Get product order book. https://docs.gdax.com/#get-product-order-book.
```
public function getProductOrderBook($productId, $param)
```

###### Get product ticker. https://docs.gdax.com/#get-product-ticker
```
This API is paginated.
public function getProductTicker($productId)
```

###### Get the trades for a specific product. https://docs.gdax.com/#get-trades
```
This API is paginated.
public function getProductTrades($productId, $param)
```

###### Get historic rates for a product. Max 200 data points. https://docs.gdax.com/#get-historic-rates
```
public function getProductHistoricRates($productId, $param)
```

###### Get 24 hour statistics for a prdoduct. https://docs.gdax.com/#get-24hr-stats
```
public function getProduct24HrStats($productId)
```

###### Get list of known currencies. https://docs.gdax.com/#get-currencies
```
public function getCurrencies()
```

###### Get GDAX server time. https://docs.gdax.com/#time
```
public function getTime()
```

### Contents
| Resource | Description |
| -------- | ----------- |
|  | |

### Contributions
Suggestions and code modifications are welcome.  Create a pull/merge request, and tell me what you are thinking.
