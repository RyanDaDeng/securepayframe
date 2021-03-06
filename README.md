# SecurePayFrame

A SecureFrame solution for SecurePay.

# Installation

## Add the following line to the require section of composer.json:
```sh
{
    "require": {
        "ryandadeng/securepay-secureframe": "dev-master"
    }
}
```

## Set up for Laravel <= 5.4
1. In /config/app.php, add the following to providers:
```sh
Ryandadeng\Securepayframe\SecurePayFrameServiceProvider::class
```
2. run `php artisan vendor:publish --provider="Ryandadeng\Securepayframe\SecurePayFrameServiceProvider"`

## Set up for Laravel >= 5.5
Just run `php artisan vendor:publish --provider="Ryandadeng\Securepayframe\SecurePayFrameServiceProvider"`

## Guideline - How to use
1. Create an implmentation class for SecurePayCustomDataInterface
2. Implement your own custom dynamic data
3. For return URL, please provide your own route url.
4. Create a controller to send and receive data from SecurePay

Example:

```sh
namespace App\Http\Controllers;


use Illuminate\Http\Request;
use Ryandadeng\Securepayframe\Services\SecurePayFactory;

class SecurePayFrameController
{


    // GET - used to populate SecurePay by using SecureFrame
    public function send()
    {
        $send = SecurePayFactory::send(new SecurePaySecurePayCustomData());
        return view('welcome', ['sender' => $send]);
    }


    // POST - This route will be hooked when SecurePay return response. This request should not be accessed publicly which means it should not be have any auth middleware attached.
    public function receive(Request $request)
    {
        // test received data
        $receiver = SecurePayFactory::receive($request->all());

        if ($receiver->fails()) {
            return $receiver->errors();
        } else {
            return 'success';
        }
    }
}
```
5. Register your two routes for send and receive request respectively.
