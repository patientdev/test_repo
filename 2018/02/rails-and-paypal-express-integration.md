# Rails and PayPal Express Checkout Integration

We'll be using Rails for *server-side integration* with PayPal's javascript library.

1. [Install PayPal's _checkout.js_ javascript library](#install-checkout)
2. [Install PayPal's _PayPal-Ruby-SDK_](#install-sdk)
3. [Configure _paypal.Button({})_](#paypal-button)
4. [Create a _PaypalController_](#paypal-controller)
5. Tests

## <a name="install-checkout"></a> Install _checkout.js_

Load `checkout.js` directly from PayPal using `https://www.paypalobjects.com/api/checkout.js`:

```ruby
= javacript_include_tag 'https://www.paypalobjects.com/api/checkout.js'
```
    
## <a name="install-sdk"></a> Install _PayPal-Ruby-SDK_

Include `PayPal-Ruby-SDK` in your Gemfile:
    
```ruby
gem 'paypal-ruby-sdk'
```

## <a name="paypal-button"></a> Configure _paypal.Button({})_

```javascript
const button_config = {
    env: 'sandbox', // for testing; 'production' for a live environment
};

paypal.Button.render(button_config)
```

## <a name="paypal-controller"></a> Create a _PaypalController_

```ruby
class PayPalController < ::ActionController::Base

    def create_payment
        payment = ::PayPal::BuildPayment.new.perform
        if payment.create
          head 200
        else
          render json: payment.error, status: 400
        end
    end
    
    def execute_payment
      ::PayPal::ExecutePayment.new.perform
    end
end
```



