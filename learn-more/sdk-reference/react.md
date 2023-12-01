---
description: To customize your payments experience
---

# React

{% hint style="info" %}
This section covers different methods and their params of the Hyperswitch React SDK
{% endhint %}

Hyperswitch's SDK come with many methods which you can use to customize your payments experience.

### useHyper()

This hook gives you access to methods in Hyper API, which you can call.

```js
let hyper = useHyper();
```

#### 1. `hyper.confirmPayment(options)`

Use `hyper.confirmPayment` to confirm a PaymentIntent using data collected by the Payment Element. When called, `hyper.confirmPayment` will attempt to complete any required actions, such as authenticating your user by displaying a 3DS dialog or redirecting them to a bank authorization page. Your user will be redirected to the return\_url you pass once the confirmation is complete.

| options (Required)     | Description                                                                                                                                                                                                                                                                                        |
| ---------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| elements (object)      | **Required**. The Elements instance that was used to create the Payment Element.                                                                                                                                                                                                                   |
| confirmParams (object) | Parameters that will be passed on to the Hyper API.                                                                                                                                                                                                                                                |
| redirect (string)      | **Can be either 'always' or 'if\_required'** By default, `hyper.confirmPayment` will always redirect to your `return_url` after a successful confirmation. If you set redirect: "if\_required", then hyper.confirmPayment will only redirect if your user chooses a redirect-based payment method. |

**ConfirmParams object**

| confirmParams                  | Description                                                                                                                                                                                                                                                                                                               |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| return\_url(string)            | **Required**. The url your customer will be directed to after they complete payment.                                                                                                                                                                                                                                      |
| shipping (object)              | The shipping details for the payment, if collected.                                                                                                                                                                                                                                                                       |
| payment\_method\_data (object) | When you call `hyper.confirmPayment`, payment details are collected from the `HyperElement` and passed to the PaymentIntents confirm endpoint as the `payment_method_data` parameter. You can also include additional payment\_method\_data fields, which will be merged with the data collected from the `HyperElement`. |

#### 2. `hyper.elements(options)`

This method creates an Elements instance, which manages a group of elements.

| options (Required)    | Description                                                                                                                                                                                                                |
| --------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| font (array)          | An array of custom fonts, which elements created from the Elements object can use. Fonts can be specified as CssFontSource or CustomFontSource objects.                                                                    |
| locale (string)       | A locale to display placeholders and error strings in. Default is auto (HyperSwitch detects the locale of the browser).                                                                                                    |
| clientSecret (string) | **Required** Required to use with the Unified Checkout and Hyper Widgets.                                                                                                                                                  |
| appearance (object)   | Match the design of your site with the appearance option. The layout of each Element stays consistent, but you can modify colors, fonts, borders, padding, and more.                                                       |
| loader (variants)     | **Can be either 'auto', 'always' or 'never'** Display skeleton loader UI while waiting for Elements to be fully loaded, after they are mounted. Default is 'auto' (HyperSwitch determines if a loader UI should be shown). |

#### 3.`hyper.confirmCardPayment(clientSecret,data?,options?)`

Use `hyper.confirmCardPayment` when the customer submits your payment form. When called, it will confirm the PaymentIntent with data you provide and carry out 3DS or other next actions if they are required.

`clientSecret` is a required string.

| data                    | Description                                                                          |
| ----------------------- | ------------------------------------------------------------------------------------ |
| payment\_method(object) | An object containing data to create a PaymentMethod with.                            |
| shipping (object)       | The shipping details for the payment, if collected.                                  |
| return\_url (string)    | **Required**. The url your customer will be directed to after they complete payment. |

| options                | Description                                               |
| ---------------------- | --------------------------------------------------------- |
| handleActions(boolean) | An object containing data to create a PaymentMethod with. |

`payment_method` example

```js
  payment_method: {
    card: "card",
    billing_details: {
      name: "Jenny Rosen",
    },
  },
```

#### 3. `hyper.retrievePaymentIntent(clientSecret)`

Retrieve a PaymentIntent using its client secret.

#### 4. `hyper.paymentRequest(options)`

Use `hyper.paymentRequest` to create a PaymentRequest object. Creating a PaymentRequest requires that you configure it with an options object.

| options (Required)          | Description                                                                                                                                                                                                                                         |
| --------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| country (string)            | **Required**. The two-letter country code of your HyperSwitch account (e.g., US).                                                                                                                                                                   |
| currency (string)           | **Required**. Three character currency code (e.g., USD).                                                                                                                                                                                            |
| total (object)              | **Required**. A PaymentItem object. This PaymentItem is shown to the customer in the browser’s payment interface.                                                                                                                                   |
| displayItems (array)        | An array of PaymentItem objects. These objects are shown as line items in the browser’s payment interface. Note that the sum of the line item amounts does not need to add up to the total amount above.                                            |
| requestPayerName (boolean)  | **recommended** By default, the browser‘s payment interface only asks the customer for actual payment information. A customer name can be collected by setting this option to true. This collected name will appears in the PaymentResponse object. |
| requestPayerEmail (boolean) | Request the payer's email-id.                                                                                                                                                                                                                       |
| requestPayerPhone (boolean) | request payer's phone number                                                                                                                                                                                                                        |
| requestShipping (boolean)   | request payer's shipping information                                                                                                                                                                                                                |
| shippingOptions (array)     | An array of ShippingOption objects. The first shipping option listed appears in the browser payment interface as the default option.                                                                                                                |
| disableWallets (array)      | An array of wallet strings. Can be one or more of applePay, googlePay, link, and browserCard. Use this option to disable Apple Pay, Google Pay, Link, and/or browser-saved cards.                                                                   |

`clientSecret` is a required string.

### useElements()

This hook gives you access to methods in Elements API, which you can call.

```js
let elements = useElements();
```

#### 1. `elements.getElement(type)`

This method retrieves a previously created Payment Element. Here the `type` is `payment` for Unified Checkout.

elements.getElement('payment') returns one of the following:

* An instance of a Unified Checkout.
* `null`, when no Unified Checkout has been created.

### Unified Checkout

The Unified Checkout provides developers a high level of customizability which enables them to shape their customer's payment journey their own way. The snippet below explains all the options that are available to use for Unified Checkout.

```js
  <UnifiedCheckout id="checkout" options={options}/>


  let options = {   // Options for creating the UnifiedCheckout.
    defaultValues: {  // Initial customer information can be passed to UnifiedCheckout that will be displayed / auto filled in the form. If not provided the form will render with empty fields.
        billingDetails: {    // To expedite the checkout process, you can enter the customer's billing information ahead of time, including their name, email, phone number, and address as needed for the payment method. By pre-populating the necessary fields with the customer's billing details, you can simplify and make the checkout process more efficient.
            name: string,
            email: string,
            phone: string,
            address: {
                line1: string,
                line2: string,
                city: string,
                state: string,
                country: string,
                postal_code: string,
            }
        }
    },
    layout: {  // [Type : String('tabs' | 'accordion') | Object] You can define the layout for the UnifiedCheckout in two ways. First, by simply passing a layout type such as 'accordion' or 'tabs', the UnifiedCheckout will be displayed using the default values associated with that layout. Alternatively, you can pass an object containing additional configuration parameters to further customize the layout of the UnifiedCheckout.
        type: string, // [Type : String('tabs' | 'accordion')] *REQUIRED*
        defaultCollapsed: boolean,  //This option allows you to choose whether the UnifiedCheckout should initially render in a collapsed state, with no payment method selected by default.
        radios: boolean, //For the accordion layout, the UnifiedCheckout renders each available Payment Method with a radio input next to its logo. The selected Payment Method is visually indicated by the corresponding radio button. Note that this property only applies to the accordion layout.
        spacedAccordionItems: boolean,  //When set to true, the available Payment Methods in the UnifiedCheckout are displayed as standalone buttons with spacing in between. Please note that this property is only relevant for the accordion layout.
    },
    business: {
        name: string, // You can enter your business details to be displayed in the UnifiedCheckout. If you don't provide this information, the UnifiedCheckout will retrieve the details from your HyperSwitch account. It's worth noting that if you choose not to provide this information now, your business name may be used in the future.
    },
    customerPaymentMethods: // [Type : Array<customer_payment_methods>] The customer_payment_methods array contains the saved payment information of a customer that is provided during the creation of a payment intent. The UnifiedCheckout can then display this list of saved card/payment methods, making the checkout process faster and easier without any unnecessary hassle.
    paymentMethodOrder: array<string>, //[Possible Values: 'card' | 'affirm' | 'klarna' | 'afterpay_clearpay' | 'sofort' | 'giropay' | 'ideal' | 'eps'  | 'google_pay'  | 'apple_pay' | 'paypal'] By default, the UnifiedCheckout uses a dynamic ordering of payment methods that is optimized for each user. However, you can manually set the order in which payment methods are displayed by providing a list of payment method types.
    fields: {  //The UnifiedCheckout is designed to collect all the necessary information required to process a payment by default. However, for certain payment methods, the checkout may request information that you may have already collected from the user, such as name or email. If this is the case, you can prevent the UnifiedCheckout from collecting this data by using the fields option. If you disable the collection of a specific field using the fields option, you must manually pass that same data string `payment intent` creation or the payment will be rejected.
        billingDetails: { //[Type : String('never' | 'auto') | Object] To avoid collecting any billing details in the UnifiedCheckout, you can set the appropriate option to "never". Alternatively, you can choose to disable the collection of specific billing details by passing an object that specifies which fields you would like to disable collection for. By default, the setting for each field or object is "auto".
            name: string, //[Type : String('never' | 'auto')]
            email: string, //[Type : String('never' | 'auto')]
            phone: string, //[Type : String('never' | 'auto')]
            address: {  //[Type : String('never' | 'auto') | Object]
                line1: string, //[Type : String('never' | 'auto')]
                line2: string,  //[Type : String('never' | 'auto')]
                city: string, //[Type : String('never' | 'auto')]
                state: string, //[Type : String('never' | 'auto')]
                country: string, //[Type : String('never' | 'auto')]
                postal_code: string, //[Type : String('never' | 'auto')]
            }
        }
    },
    readOnly: boolean, //The readOnly option applies a read-only state to the UnifiedCheckout, preventing any changes to payment details. The default setting for this option is false. Enabling the readOnly option does not change the appearance of the UnifiedCheckout. If you want to customize the appearance of the checkout, you can use the Appearance object.
    wallets: {  //The UnifiedCheckout will, by default, display all payment methods associated with the underlying Payment Intent. This feature enables you to customize and display wallets based on your specific requirements.
        walletReturnUrl: string, //The walletReturnUrl is the return URL for all wallets. As the confirmPayment method handles other payment methods, wallets are displayed as a button and require a return URL to redirect back to your website for payment confirmation.
        applePay: string, //[Type : String('never' | 'auto')] To exclude a specific wallet from being displayed as a payment option, you can set its property to never. The default value of this property is auto.
        googlePay: string, //[Type : String('never' | 'auto')] To exclude a specific wallet from being displayed as a payment option, you can set its property to never. The default value of this property is auto.
        paypal: string, //[Type : String('never' | 'auto')] To exclude a specific wallet from being displayed as a payment option, you can set its property to never. The default value of this property is auto.
        style: {  //Style you wallet payment methods as per your requirements in terms of type, theme and height.
            type_: string, //[Possible Values: 'checkout' | 'pay' | 'buy' | 'installment' | 'pay' | 'default' | 'book' | 'donate'  | 'order'  | 'addmoney' | 'topup' | 'rent' | 'subscribe' | 'reload' | 'support' | 'tip' | 'contribute'] The type property in UnifiedCheckout allows customization of the button type, and the text on the button will be displayed accordingly. However, it's important to note that not all wallets support the same button type. If a type is provided that is not supported by a particular wallet, it will fallback to its default type.
            theme: string, //[Possible Values: 'dark' | 'light' | 'outline' ]
            height: int, //You can customize the height of the wallet button in the UnifiedCheckout using the height property. However, not all wallets support different height options. If you provide a height value that is outside the supported range of a particular wallet, it will fallback to either the maximum or minimum height value based on the value provided.
        }


    },
  }



  {
  "defaultValues": {
    "billingDetails": {
      "name": "string",
      "email": "string",
      "phone": "string",
      "address": {
        "line1": "string",
        "line2": "string",
        "city": "string",
        "state": "string",
        "country": "string",
        "postal_code": "string"
      }
    }
  },
  "layout": {
    "type": "string",
    "defaultCollapsed": "boolean",
    "radios": "boolean",
    "spacedAccordionItems": "boolean"
  },
  "business": {
    "name": "string"
  },
  "customerPaymentMethods": "array<customer_payment_methods>",
  "paymentMethodOrder": "array<string>",
  "fields": {
    "billingDetails": {
      "name": "string",
      "email": "string",
      "phone": "string",
      "address": {
        "line1": "string",
        "line2": "string",
        "city": "string",
        "state": "string",
        "country": "string",
        "postal_code": "string"
      }
    }
  },
  "readOnly": "boolean",
  "wallets": {
    "walletReturnUrl": "string",
    "applePay": "string",
    "googlePay": "string",
    "paypal": "string",
    "style": {
      "type": "string",
      "theme": "string",
      "height": "int"
    }
  }
  }




```