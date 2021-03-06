# Laravel Cashier
# 라라벨 캐셔

- [Introduction](#introduction)
- [시작하기](#introduction)
- [Upgrading Cashier](#upgrading-cashier)
- [캐셔 업그레이드하기](#upgrading-cashier)
- [Installation](#installation)
- [설치하기](#installation)
- [Configuration](#configuration)
- [설정하기](#configuration)
    - [Billable Model](#billable-model)
    - [Billable 모델](#billable-model)
    - [API Keys](#api-keys)
    - [API Keys](#api-keys)
    - [Currency Configuration](#currency-configuration)
    - [통화 설정하기](#currency-configuration)
    - [Logging](#logging)
    - [로깅](#logging)
- [Customers](#customers)
- [고객](#customers)
    - [Retrieving Customers](#retrieving-customers)
    - [고객 조회](#retrieving-customers)
    - [Creating Customers](#creating-customers)
    - [고객 생성](#creating-customers)
    - [Updating Customers](#updating-customers)
    - [고객 업데이트](#updating-customers)
    - [Billing Portal](#billing-portal)
    - [빌링 포탈](#billing-portal)
- [Payment Methods](#payment-methods)
- [결제 수단](#payment-methods)
    - [Storing Payment Methods](#storing-payment-methods)
    - [결제 수단 저장](#storing-payment-methods)
    - [Retrieving Payment Methods](#retrieving-payment-methods)
    - [결제 수단 조회](#retrieving-payment-methods)
    - [Determining If A User Has A Payment Method](#check-for-a-payment-method)
    - [사용자에게 결제 수단이 있는지 확인](#check-for-a-payment-method)
    - [Updating The Default Payment Method](#updating-the-default-payment-method)
    - [기본 결제 수단 업데이트](#updating-the-default-payment-method)
    - [Adding Payment Methods](#adding-payment-methods)
    - [결제 수단 추가](#adding-payment-methods)
    - [Deleting Payment Methods](#deleting-payment-methods)
    - [결제 수단 삭제](#deleting-payment-methods)
- [Subscriptions](#subscriptions)
- [정기 구독 모델](#subscriptions)
    - [Creating Subscriptions](#creating-subscriptions)
    - [새로운 정기 구독 생성하기](#creating-subscriptions)
    - [Checking Subscription Status](#checking-subscription-status)
    - [정기 구독 상태 확인하기](#checking-subscription-status)
    - [Changing Plans](#changing-plans)
    - [정기 구독 유형 변경하기](#changing-plans)
    - [Subscription Quantity](#subscription-quantity)
    - [Multiplan Subscriptions](#multiplan-subscriptions)
    - [멀티 플랜 구독](#multiplan-subscriptions)
    - [정기 구독 수량 변경하기](#subscription-quantity)
    - [Subscription Taxes](#subscription-taxes)
    - [정기 구독의 세금계산](#subscription-taxes)
    - [Subscription Anchor Date](#subscription-anchor-date)
    - [정기 구독의 고정일](#subscription-anchor-date)
    - [Cancelling Subscriptions](#cancelling-subscriptions)
    - [정기 구독 취소하기](#cancelling-subscriptions)
    - [Resuming Subscriptions](#resuming-subscriptions)
    - [정기 구독 재개](#resuming-subscriptions)
- [Subscription Trials](#subscription-trials)
- [구독 평가기간-trial](#subscription-trials)
    - [With Payment Method Up Front](#with-payment-method-up-front)
    - [결제 수단 사전 등록](#with-payment-method-up-front)
    - [Without Payment Method Up Front](#without-payment-method-up-front)
    - [결제 수단없이 사전 등록](#without-payment-method-up-front)
    - [Extending Trials](#extending-trials)
    - [평가기간 연장](#extending-trials)
- [Handling Stripe Webhooks](#handling-stripe-webhooks)
- [Stripe webook 처리하기](#handling-stripe-webhooks)
    - [Defining Webhook Event Handlers](#defining-webhook-event-handlers)
    - [webook 이벤트 핸들러 정의하기](#defining-webhook-event-handlers)
    - [Failed Subscriptions](#handling-failed-subscriptions)
    - [실패한 정기구독](#handling-failed-subscriptions)
    - [Verifying Webhook Signatures](#verifying-webhook-signatures)
    - [Webhook의 서명 검증](#verifying-webhook-signatures)
- [Single Charges](#single-charges)
- [한번만 결제하기](#single-charges)
    - [Simple Charge](#simple-charge)
    - [기본 결제](#simple-charge)
    - [Charge With Invoice](#charge-with-invoice)
    - [청구서와 같이 결제](#charge-with-invoice)
    - [Refunding Charges](#refunding-charges)
    - [환불 수수료](#refunding-charges)
- [Invoices](#invoices)
- [청구서](#invoices)
    - [Retrieving Invoices](#retrieving-invoices)
    - [인보이스 검색](#retrieving-invoices)
    - [Generating Invoice PDFs](#generating-invoice-pdfs)
    - [인보이스 PDF 생성](#generating-invoice-pdfs)
- [Handling Failed Payments](#handling-failed-payments)
- [결제 실패 처리](#handling-failed-payments)
- [Strong Customer Authentication (SCA)](#strong-customer-authentication)
- [강력한 고객 인증 (SCA)](#strong-customer-authentication)
    - [Payments Requiring Additional Confirmation](#payments-requiring-additional-confirmation)
    - [추가 확인이 필요한 결제](#payments-requiring-additional-confirmation)
    - [Off-session Payment Notifications](#off-session-payment-notifications)
    - [세션 외 결제 알림](#off-session-payment-notifications)
- [Stripe SDK](#stripe-sdk)
- [Stripe SDK](#stripe-sdk)
- [Testing](#testing)
- [테스팅](#testing)

<a name="introduction"></a>
## Introduction
## 시작하기

Laravel Cashier provides an expressive, fluent interface to [Stripe's](https://stripe.com) subscription billing services. It handles almost all of the boilerplate subscription billing code you are dreading writing. In addition to basic subscription management, Cashier can handle coupons, swapping subscription, subscription "quantities", cancellation grace periods, and even generate invoice PDFs.

라라벨 캐셔는 [Stripe](https://stripe.com)에 의해서 제공되는 손쉽고 편리한 구독(정기 과금) 서비스를 위한 인터페이스를 제공합니다. 라라벨 캐셔는 여러분이 작성하는데 어려움을 겪는 구독을 위한 청구서에서 필요한 거의 모든 관용구문들을 다룹니다.  기본적인 구독 관리 외에도, 캐셔를 통해서 쿠폰 관리, 구독 변경, 구매 수량 변경, 취소 유예 기간, 그리고 청구서를 PDF로 생성할 수도 있습니다.

<a name="upgrading-cashier"></a>
## Upgrading Cashier
## 캐셔 업그레이드하기

When upgrading to a new version of Cashier, it's important that you carefully review [the upgrade guide](https://github.com/laravel/cashier-stripe/blob/master/UPGRADE.md).

Cashier의 새 버전으로 업그레이드 할 때는 [업그레이드 가이드](https://github.com/laravel/cashier-stripe/blob/master/UPGRADE.md)를 주의 깊게 검토해야합니다.

> {note} To prevent breaking changes, Cashier uses a fixed Stripe API version. Cashier 12 utilizes Stripe API version `2020-03-02`. The Stripe API version will be updated on minor releases in order to make use of new Stripe features and improvements.

> {note} 변경을 방지하기 위해 Cashier는 Stripe API 버전을 고정으로 사용합니다. Cashier 12은 Stripe API 버전 `2020-03-02`를 사용합니다. Stripe API 버전은 새로운 Stripe 기능과 개선 사항을 사용하기 위해 마이너 릴리스에서 업데이트됩니다.

<a name="installation"></a>
## Installation
## 설치하기

First, require the Cashier package for Stripe with Composer:
    
먼저 Stripe를 위한 캐셔 패키지를 의존성에 추가하십시오.

    composer require laravel/cashier

> {note} To ensure Cashier properly handles all Stripe events, remember to [set up Cashier's webhook handling](#handling-stripe-webhooks).

> {note} Cashier가 모든 스트라이프 이벤트를 올바르게 처리하도록하려면 [Cashier의 웹훅 처리](#handling-stripe-webhooks)를 설정해야합니다.

#### Database Migrations
#### 데이터베이스 마이그레이션

The Cashier service provider registers its own database migration directory, so remember to migrate your database after installing the package. The Cashier migrations will add several columns to your `users` table as well as create a new `subscriptions` table to hold all of your customer's subscriptions:

Cashier 서비스 프로바이더는 자체 데이터베이스 마이그레이션 디렉토리를 등록하므로, 패키지를 설치 한 후 데이터베이스를 마이그레이션해야합니다. Cashier 마이그레이션은 `users`테이블에 여러 컬럼을 추가하고 모든 고객의 구독을 보유 할 `subscriptions`테이블을 생성합니다.

    php artisan migrate

If you need to overwrite the migrations that ship with the Cashier package, you can publish them using the `vendor:publish` Artisan command:

Cashier 패키지와 함께 제공된 마이그레이션을 덮어 써야하는 경우 `vendor:publish` 아티잔 명령어을 사용하여 마이그레이션 파일을 내보낼 수 있습니다.

    php artisan vendor:publish --tag="cashier-migrations"

If you would like to prevent Cashier's migrations from running entirely, you may use the `ignoreMigrations` provided by Cashier. Typically, this method should be called in the `register` method of your `AppServiceProvider`:

Cashier의 마이그레이션이 실행되지 않게하려면 Cashier가 제공하는 `ignoreMigrations`를 사용하면 됩니다. 일반적으로 이 메소드는 `AppServiceProvider`의 `register` 메소드에서 호출합니다.

    use Laravel\Cashier\Cashier;

    Cashier::ignoreMigrations();

> {note} Stripe recommends that any column used for storing Stripe identifiers should be case-sensitive. Therefore, you should ensure the column collation for the `stripe_id` column is set to, for example, `utf8_bin` in MySQL. More info can be found [in the Stripe documentation](https://stripe.com/docs/upgrades#what-changes-does-stripe-consider-to-be-backwards-compatible).

> {note} Stripe은 Stripe 식별자를 저장하는 데 사용되는 모든 컬럼은 대소문자를 구분해야합니다. 따라서 예를 들자면 MySQL에서 `stripe_id` 컬럼에 대한 컬럼 데이터 컬렉션이 `utf8_bin`으로 설정되어 있는지 확인해야합니다. 자세한 내용은 [스트라이프 설명서](https://stripe.com/docs/upgrades#what-changes-does-stripe-consider-to-be-backwards-compatible)에서 확인할 수 있습니다.

<a name="configuration"></a>
## Configuration
## 설정하기

<a name="billable-model"></a>
### Billable Model
### Billable 모델

Before using Cashier, add the `Billable` trait to your model definition. This trait provides various methods to allow you to perform common billing tasks, such as creating subscriptions, applying coupons, and updating payment method information:

Cashier를 사용하기전, `Billable` 트레이트-trait를 모델에 추가합니다. 이 트레이트-trait는 정기구독을 새롭게 생성하거나, 쿠폰을 적용하거나, 결제 수단 정보를 업데이트 하는 것과 같은 결제와 관련된 공통의 작업들을 제공합니다.

    use Laravel\Cashier\Billable;

    class User extends Authenticatable
    {
        use Billable;
    }

Cashier assumes your Billable model will be the `App\User` class that ships with Laravel. If you wish to change this you can specify a different model in your `.env` file:

Cashier는 Billable 모델이 Laravel과 함께 제공되는 `App\User` 클래스라고 가정합니다. 이것을 바꾸고 싶다면 `.env` 파일에서 다른 모델을 지정할 수 있습니다.

    CASHIER_MODEL=App\User

> {note} If you're using a model other than Laravel's supplied `App\User` model, you'll need to publish and alter the [migrations](#installation) provided to match your alternative model's table name.

> {note} Laravel에서 제공 한 `App\User` 모델 이외의 모델을 사용하는 경우, 대체 모델의 테이블 이름과 일치하도록 제공된 [migrations](#installation)을 퍼블리싱하고 변경해야합니다.

<a name="api-keys"></a>
### API Keys
### API Key

Next, you should configure your Stripe keys in your `.env` file. You can retrieve your Stripe API keys from the Stripe control panel.

다음으로 `.env` 파일에서 Stripe 키를 설정해야합니다. Stripe 컨트롤 패널에서 Stripe API 키를 찾을 수 있습니다.

    STRIPE_KEY=your-stripe-key
    STRIPE_SECRET=your-stripe-secret

<a name="currency-configuration"></a>
### Currency Configuration
### 통화 설정하기

The default Cashier currency is United States Dollars (USD). You can change the default currency by setting the `CASHIER_CURRENCY` environment variable:

Cashier의 기본 통화는 미국 달러(USD)입니다. `CASHIER_CURRENCY` 환경 변수를 설정하여 기본 통화를 변경할 수 있습니다.

    CASHIER_CURRENCY=eur

In addition to configuring Cashier's currency, you may also specify a locale to be used when formatting money values for display on invoices. Internally, Cashier utilizes [PHP's `NumberFormatter` class](https://www.php.net/manual/en/class.numberformatter.php) to set the currency locale:

Cashier의 통화 구성 외에도 송장에 표시 할 돈의 값을 포매팅-formatting 할 때 사용할 로케일-locale을 지정할 수도 있습니다. 내부적으로 Cashier는 [PHP의 `NumberFormatter` 클래스](https://www.php.net/manual/en/class.numberformatter.php)를 사용하여 통화 로케일-locale을 설정합니다.

    CASHIER_CURRENCY_LOCALE=nl_BE

> {note} In order to use locales other than `en`, ensure the `ext-intl` PHP extension is installed and configured on your server.

> {note} `en` 이외의 로케일을 사용하려면 서버에 `ext-intl` PHP 확장모듈이 설치 및 설정되어 있는지 확인하십시오.

<a name="logging"></a>
#### Logging
#### 로깅

Cashier allows you to specify the log channel to be used when logging all Stripe related exceptions. You may specify the log channel using the `CASHIER_LOGGER` environment variable:

Cashier를 사용하면 모든 스트라이프 관련 예외를 기록 할 때 사용할 로그 채널을 지정할 수 있습니다. `CASHIER_LOGGER` 환경 변수를 사용하여 로그 채널을 지정할 수 있습니다.

    CASHIER_LOGGER=stack

<a name="customers"></a>
## Customers
## 고객

<a name="retrieving-customers"></a>
### Retrieving Customers
### 고객 조회

You can retrieve a customer by their Stripe ID using the `Cashier::findBillable` method. This will return an instance of the Billable model:

`Cashier::findBillable` 메서드를 사용하여 스트라이프 ID로 고객을 조회 할 수 있습니다. 그러면 Billable 모델의 인스턴스가 반환됩니다.

    use Laravel\Cashier\Cashier;

    $user = Cashier::findBillable($stripeId);

<a name="creating-customers"></a>
### Creating Customers
### 고객 생성하기

Occasionally, you may wish to create a Stripe customer without beginning a subscription. You may accomplish this using the `createAsStripeCustomer` method:

경우에 따라 구독을 시작하지 않고 Stripe 고객을 만들 수도 있습니다. 이 경우 `createAsStripeCustomer` 메소드를 사용하면 됩니다.

    $stripeCustomer = $user->createAsStripeCustomer();

Once the customer has been created in Stripe, you may begin a subscription at a later date. You can also use an optional `$options` array to pass in any additional parameters which are supported by the Stripe API:

Stripe에서 고객이 생성되면 나중에 구독을 시작할 수 있습니다. 선택적인 `$options` 배열을 사용하여 Stripe API에서 지원하는 추가 파라메터를 전달할 수도 있습니다.

    $stripeCustomer = $user->createAsStripeCustomer($options);

You may use the `asStripeCustomer` method if you want to return the customer object if the billable entity is already a customer within Stripe:

billable 엔티티가 Stripe의 고객일 경우 고객 오브젝트를 반환하려고 한다면 `asStripeCustomer` 메소드를 사용할 수도 있습니다.

    $stripeCustomer = $user->asStripeCustomer();

The `createOrGetStripeCustomer` method may be used if you want to return the customer object but are not sure whether the billable entity is already a customer within Stripe. This method will create a new customer in Stripe if one does not already exist:

`createOrGetStripeCustomer`메서드는 고객 객체를 반환하고 싶지만 청구 가능한 엔터티가 Stripe 내에 이미 고객인지 아닌지 확실하지 않은 경우에 사용할 수 있습니다. 이 메서드는 고객이 존재하지 않는 경우 Stripe에 새 고객을 생성합니다.

    $stripeCustomer = $user->createOrGetStripeCustomer();

<a name="updating-customers"></a>
### Updating Customers
### 고객 업데이트

Occasionally, you may wish to update the Stripe customer directly with additional information. You may accomplish this using the `updateStripeCustomer` method:

때때로 추가 정보를 사용하여 Stripe 고객을 직접 업데이트 할 수 있습니다. `updateStripeCustomer` 메소드를 사용하여 이를 수행 할 수 있습니다.

    $stripeCustomer = $user->updateStripeCustomer($options);

<a name="billing-portal"></a>
### Billing Portal
### 빌링 포탈

Stripe offers [an easy way to set up a billing portal](https://stripe.com/docs/billing/subscriptions/customer-portal) so your customer can manage their subscription, payment methods, and view their billing history. You can redirect your users to the billing portal using the `redirectToBillingPortal` method from a controller or route:

Stripe는 [결제 포탈을 설정하는 쉬운 방법](https://stripe.com/docs/billing/subscriptions/customer-portal) 을 제공하므로 고객이 구독, 결제 수단을 관리하고 결제 내역을 볼 수 있습니다. 컨트롤러 또는 라우트에서 `redirectToBillingPortal` 메소드를 사용하여 사용자를 결제 포탈로 리디렉션 할 수 있습니다.

    use Illuminate\Http\Request;

    public function billingPortal(Request $request)
    {
        return $request->user()->redirectToBillingPortal();
    }

By default, when the user is finished managing their subscription, they can return to the `home` route of your application. You may provide a custom URL the user should return to by passing the URL as an argument to the `redirectToBillingPortal` method:

기본적으로 사용자가 구독 관리를 끝내면 애플리케이션의 `home`경로로 돌아갑니다. `redirectToBillingPortal` 메소드에 URL을 인수로 전달하여 사용자가 돌아가야하는  URL을 지정 할 수 있습니다.

    use Illuminate\Http\Request;

    public function billingPortal(Request $request)
    {
        return $request->user()->redirectToBillingPortal(
            route('billing')
        );
    }

If you would like to only generate the URL to the billing portal, you may use the `billingPortalUrl` method:

빌링 포탈에 대한 URL만 생성하려면 `billingPortalUrl` 메소드를 사용할 수 있습니다.

    $url = $user->billingPortalUrl(route('billing'));

<a name="payment-methods"></a>
## Payment Methods
## 결제 수단

<a name="storing-payment-methods"></a>
### Storing Payment Methods
### 결제 수단 저장

In order to create subscriptions or perform "one off" charges with Stripe, you will need to store a payment method and retrieve its identifier from Stripe. The approach used to accomplish differs based on whether you plan to use the payment method for subscriptions or single charges, so we will examine both below.

Stripe에서 구독을 생성하거나 "일회성" 결제를 실행하려면 결제 수단을 저장하고 Stripe에서 해당 ID를 조회해야합니다. 이를 위해 사용되는 처리 방식은 지불 방법을 구독 또는 단일 청구 중 어디에 사용할지 따라 다르므로 아래에서 두 가지를 모두 확인해보겠습니다.

#### Payment Methods For Subscriptions
#### 구독을 위한 결제 수단

When storing credit cards to a customer for future use, the Stripe Setup Intents API must be used to securely gather the customer's payment method details. A "Setup Intent" indicates to Stripe the intention to charge a customer's payment method. Cashier's `Billable` trait includes the `createSetupIntent` to easily create a new Setup Intent. You should call this method from the route or controller that will render the form which gathers your customer's payment method details:

고객이 나중에 신용 카드를 사용하기 위해 저장하는 경우 Stripe Setup Intents API를 사용하여 고객의 결제 수단 세부 사항을 안전하게 가져와야합니다. "설정 의도-Setup Intent"는 고객의 지불 방법 청구 의도를 스트라이핑한다는 것을 나타냅니다. Cashier의  `Billable` trait에는 새 설정 의도를 쉽게 만들 수 있는 `createSetupIntent`가 포함되어 있습니다. 고객의 결제 수단 세부 정보를 수집하는 폼을 렌더링하는 라우트 또는 컨트롤러에서 이 메서드을 호출해야합니다.

    return view('update-payment-method', [
        'intent' => $user->createSetupIntent()
    ]);

After you have created the Setup Intent and passed it to the view, you should attach its secret to the element that will gather the payment method. For example, consider this "update payment method" form:

설정 의도를 작성하여 뷰에 전달한 후 결제 수단을 수집 할 element에 해당 secret을 추가해야합니다. 예를 들어 다음 "결제 방법 업데이트" 폼을 참고하십시오.

    <input id="card-holder-name" type="text">

    <!-- Stripe Elements Placeholder -->
    <div id="card-element"></div>

    <button id="card-button" data-secret="{{ $intent->client_secret }}">
        Update Payment Method
    </button>

Next, the Stripe.js library may be used to attach a Stripe Element to the form and securely gather the customer's payment details:

다음으로 Stripe.js 라이브러리를 사용하고 스트라이프 element를 폼에 추가하여 고객의 결제 세부 사항을 안전하게 가져 올 수 있습니다.

    <script src="https://js.stripe.com/v3/"></script>

    <script>
        const stripe = Stripe('stripe-public-key');

        const elements = stripe.elements();
        const cardElement = elements.create('card');

        cardElement.mount('#card-element');
    </script>

Next, the card can be verified and a secure "payment method identifier" can be retrieved from Stripe using [Stripe's `confirmCardSetup` method](https://stripe.com/docs/js/setup_intents/confirm_card_setup):

다음으로, [Stripe의 `confirmCardSetup` 메소드](https://stripe.com/docs/stripe-js/reference#stripe-handle-card)를 사용하여 카드를 확인하고 안전한 "결제 방법 식별자"를 Stripe에서 조회 할 수 있습니다.

    const cardHolderName = document.getElementById('card-holder-name');
    const cardButton = document.getElementById('card-button');
    const clientSecret = cardButton.dataset.secret;

    cardButton.addEventListener('click', async (e) => {
        const { setupIntent, error } = await stripe.confirmCardSetup(
            clientSecret, {
                payment_method: {
                    card: cardElement,
                    billing_details: { name: cardHolderName.value }
                }
            }
        );

        if (error) {
            // Display "error.message" to the user...
        } else {
            // The card has been verified successfully...
        }
    });

After the card has been verified by Stripe, you may pass the resulting `setupIntent.payment_method` identifier to your Laravel application, where it can be attached to the customer. The payment method can either be [added as a new payment method](#adding-payment-methods) or [used to update the default payment method](#updating-the-default-payment-method). You can also immediately use the payment method identifier to [create a new subscription](#creating-subscriptions).

Stripe에서 카드를 확인한 후 결과 `setupIntent.payment_method` 식별자를 라라벨 애플리케이션에 전달하여 고객에게 추가 할 수 있습니다. 결제 수단은 [결제 수단 추가](#adding-payment-methods) 또는 [기본 결제 수단 업데이트](#updating-the-default-payment-method) 일 수 있습니다. 결제 수단 식별자를 즉시 ​​사용하여 [새로운 정기 구독 생성하기](#creating-subscriptions)를 할 수도 있습니다.

> {tip} If you would like more information about Setup Intents and gathering customer payment details please [review this overview provided by Stripe](https://stripe.com/docs/payments/save-and-reuse#php).

> {tip} 설정 의도 및 고객 지불 정보 수집에 대한 자세한 내용을 보려면 [Stripe에서 제공하는 개요](https://stripe.com/docs/payments/save-and-reuse#php)를 참고하십시오.

#### Payment Methods For Single Charges
#### 단일 청구에 대한 결제 수단

Of course, when making a single charge against a customer's payment method we'll only need to use a payment method identifier a single time. Due to Stripe limitations, you may not use the stored default payment method of a customer for single charges. You must allow the customer to enter their payment method details using the Stripe.js library. For example, consider the following form:

물론 고객의 결제 수단으로 단일 청구를 할 때는 결제 수단 식별자를 한 번만 사용하면됩니다. 스트라이프 제한으로 인해 단일 청구에 대해 고객의 저장된 기본 결제 방법을 사용할 수 없습니다. 고객이 Stripe.js 라이브러리를 사용하여 결제 수단 세부 사항을 입력하도록 해야합니다. 예를 들어 다음 양식을 참고하십시오.

    <input id="card-holder-name" type="text">

    <!-- Stripe Elements Placeholder -->
    <div id="card-element"></div>

    <button id="card-button">
        Process Payment
    </button>

Next, the Stripe.js library may be used to attach a Stripe Element to the form and securely gather the customer's payment details:

다음으로 Stripe.js 라이브러리를 사용하고 스트라이프 element를 폼에 추가하여 고객의 결제 세부 사항을 안전하게 가져 올 수 있습니다.

    <script src="https://js.stripe.com/v3/"></script>

    <script>
        const stripe = Stripe('stripe-public-key');

        const elements = stripe.elements();
        const cardElement = elements.create('card');

        cardElement.mount('#card-element');
    </script>

Next, the card can be verified and a secure "payment method identifier" can be retrieved from Stripe using [Stripe's `createPaymentMethod` method](https://stripe.com/docs/stripe-js/reference#stripe-create-payment-method):

다음으로, [Stripe의 `createPaymentMethod` 메소드](https://stripe.com/docs/stripe-js/reference#stripe-create-payment)를 사용하여 카드를 확인하고 안전한 "지불 방법 식별자"를 Stripe에서 조회 할 수 있습니다.

    const cardHolderName = document.getElementById('card-holder-name');
    const cardButton = document.getElementById('card-button');

    cardButton.addEventListener('click', async (e) => {
        const { paymentMethod, error } = await stripe.createPaymentMethod(
            'card', cardElement, {
                billing_details: { name: cardHolderName.value }
            }
        );

        if (error) {
            // Display "error.message" to the user...
        } else {
            // The card has been verified successfully...
        }
    });

If the card is verified successfully, you may pass the `paymentMethod.id` to your Laravel application and process a [single charge](#simple-charge).

카드가 성공적으로 확인되면 `paymentMethod.id`를 라라벨 애플리케이션에 전달하고 [단일 청구](#simple-charge)를 처리 할 수 ​​있습니다.

<a name="retrieving-payment-methods"></a>
### Retrieving Payment Methods
### 결제 수단 검색

The `paymentMethods` method on the Billable model instance returns a collection of `Laravel\Cashier\PaymentMethod` instances:

Billable 모델 인스턴스의 `paymentMethods` 메소드는 `Laravel\Cashier\PaymentMethod` 인스턴스의 콜렉션을 리턴합니다.

    $paymentMethods = $user->paymentMethods();

To retrieve the default payment method, the `defaultPaymentMethod` method may be used:

기본 결제 수단을 조회하려면 `defaultPaymentMethod` 메소드를 사용할 수 있습니다.

    $paymentMethod = $user->defaultPaymentMethod();

You can also retrieve a specific payment method that is owned by the Billable model using the `findPaymentMethod` method:

`findPaymentMethod` 메소드를 사용하여 청구 가능 모델이 소유한 특정 지불 수단를 조회 할 수도 있습니다.

    $paymentMethod = $user->findPaymentMethod($paymentMethodId);

<a name="check-for-a-payment-method"></a>
### Determining If A User Has A Payment Method
### 사용자에게 결제 수단이 있는지 확인

To determine if a Billable model has a default payment method attached to their account, use the `hasDefaultPaymentMethod` method:

청구 가능 모델에 계정에 연결된 기본 결제 수단이 있는지 확인하려면 `hasDefaultPaymentMethod`메소드를 사용하세요.

    if ($user->hasDefaultPaymentMethod()) {
        //
    }

To determine if a Billable model has at least one payment method attached to their account, use the `hasPaymentMethod` method:

청구 가능 모델에 계정에 연결된 결제 수단이 하나 이상 있는지 확인하려면 `hasPaymentMethod`메서드를 사용하세요.

    if ($user->hasPaymentMethod()) {
        //
    }

<a name="updating-the-default-payment-method"></a>
### Updating The Default Payment Method
### 기본 결제 수단 업데이트

The `updateDefaultPaymentMethod` method may be used to update a customer's default payment method information. This method accepts a Stripe payment method identifier and will assign the new payment method as the default billing payment method:

`updateDefaultPaymentMethod` 메소드는 고객의 기본 결제 수단 정보를 업데이트하는 데 사용할 수 있습니다. 이 방법은 Stripe 지불 방법 식별자를 허용하며 새 지불 방법을 기본 청구 지불 방법으로 지정합니다.

    $user->updateDefaultPaymentMethod($paymentMethod);

To sync your default payment method information with the customer's default payment method information in Stripe, you may use the `updateDefaultPaymentMethodFromStripe` method:

기본 결제 수단 정보를 Stripe의 고객의 기본 결제 수단 정보와 동기화하려면 `updateDefaultPaymentMethodFromStripe` 메소드를 사용할 수 있습니다.

    $user->updateDefaultPaymentMethodFromStripe();

> {note} The default payment method on a customer can only be used for invoicing and creating new subscriptions. Due to limitations from Stripe, it may not be used for single charges.

> {note} 고객의 기본 결제 수단은 인보이스 발행 및 새 구독 생성에만 사용할 수 있습니다. Stripe의 제한으로 인해 단일 청구에 사용할 수 없습니다.

<a name="adding-payment-methods"></a>
### Adding Payment Methods
### 결제 수단 추가

To add a new payment method, you may call the `addPaymentMethod` method on the billable user, passing the payment method identifier:

새로운 결제 수단을 추가하려면 billable 사용자에게 `addPaymentMethod` 메소드를 호출하여 결제 수단 식별자를 전달하면됩니다.

    $user->addPaymentMethod($paymentMethod);

> {tip} To learn how to retrieve payment method identifiers please review the [payment method storage documentation](#storing-payment-methods).

> {tip} 결제 수단 식별자를 조회하는 방법을 알아 보려면 [결제 수단 저장](#storing-payment-methods)을 확인하십시오.

<a name="deleting-payment-methods"></a>
### Deleting Payment Methods
### 결제 수단 삭제

To delete a payment method, you may call the `delete` method on the `Laravel\Cashier\PaymentMethod` instance you wish to delete:

결제 수단을 삭제하려면 삭제하려는 `Laravel\Cashier\PaymentMethod` 인스턴스에서 `delete` 메소드를 호출하면됩니다.

    $paymentMethod->delete();

The `deletePaymentMethods` method will delete all of the payment method information for the Billable model:

`deletePaymentMethods` 메소드는 Billable 모델에 대한 모든 지불 수단 정보를 삭제합니다.

    $user->deletePaymentMethods();

> {note} If a user has an active subscription, you should prevent them from deleting their default payment method.

> {note} 사용자가 현재 정기 구독중인 경우 기본 결제 수단을 삭제하지 못하게 해야합니다.

<a name="subscriptions"></a>
## Subscriptions
## 정기 구독 모델

<a name="creating-subscriptions"></a>
### Creating Subscriptions
### 새로운 정기 구독 생성하기

To create a subscription, first retrieve an instance of your billable model, which typically will be an instance of `App\User`. Once you have retrieved the model instance, you may use the `newSubscription` method to create the model's subscription:

새로운 정기 구독을 생성하려면, 먼저 청구가 가능한 (일반적으로 `App\User`가 되는 ) 모델 인스턴스를 조회해야 합니다. 모델 인스턴스를 조회하면, 모델에 대한 정기 구독을 생성하기 위해 `newSubscription` 메소드를 사용할 수 있습니다.

    $user = User::find(1);

    $user->newSubscription('default', 'price_premium')->create($paymentMethod);

The first argument passed to the `newSubscription` method should be the name of the subscription. If your application only offers a single subscription, you might call this `default` or `primary`. The second argument is the specific plan the user is subscribing to. This value should correspond to the plan's price identifier in Stripe.

`newSubscription` 메소드에 전달되는 첫번째 인자는 정기 구독의 제목이 되어야 합니다. 애플리케이션이 단 하나의 구독모델을 제공한다면, `default` 또는 `primary` 와 같이 사용할 수 있습니다. 두번째 인자는 사용자가 구독하고자 하는 지정된 plan입니다. 이 값은 Stripe의 plan의 가격 식별자와 일치해야 합니다.

The `create` method, which accepts [a Stripe payment method identifier](#storing-payment-methods) or Stripe `PaymentMethod` object, will begin the subscription as well as update your database with the customer ID and other relevant billing information.

[스트라이프 결제 방법 식별자](#storing-payment-methods) 또는 스트라이프 `PaymentMethod` 오브젝트를 허용하는 `create` 메소드는 구독을 시작하고 고객 ID 및 기타 관련 청구 정보로 데이터베이스를 업데이트합니다.

> {note} Passing a payment method identifier directly to the `create()` subscription method will also automatically add it to the user's stored payment methods.

> {note} 결제 수단 식별자를 `create()` 정기 구독 메소드에 직접 전달하면 자동으로 사용자의 저장된 지불 수단에 추가됩니다.

#### Quantities
#### 수량

If you would like to set a specific quantity for the plan when creating the subscription, you may use the `quantity` method:

구독을 생성 할 때 플랜에 대한 특정 수량을 설정하려면 `quantity` 메서드를 사용할 수 있습니다.

    $user->newSubscription('default', 'price_monthly')
         ->quantity(5)
         ->create($paymentMethod);
         
#### Additional Details
#### 추가적인 상세 정보

If you would like to specify additional customer or subscription details, you may do so by passing them as the second and third arguments to the `create` method:

만약 여러분이 추가적인 사용자 정보나 구독의 세부 사항을 지정하고 싶다면 이러한 정보를 `create` 메소드의 두번째와 세번째 인자로 전달하면 됩니다.

    $user->newSubscription('default', 'price_monthly')->create($paymentMethod, [
        'email' => $email,
    ], [
        'metadata' => ['note' => 'Some extra information.'],
    ]);

To learn more about the additional fields supported by Stripe, check out Stripe's documentation on [customer creation](https://stripe.com/docs/api#create_customer) and [subscription creation](https://stripe.com/docs/api/subscriptions/create).

추가적인 필드에 대한 Stripe의 지원정보를 확인하고자 한다면 Stripe의 [고객 생성에 관한 문서](https://stripe.com/docs/api#create_customer)와 [구독 생성](https://stripe.com/docs/api/subscriptions/create)을 확인하십시오.

#### Coupons
#### 쿠폰

If you would like to apply a coupon when creating the subscription, you may use the `withCoupon` method:

새로운 구독을 생성 할 때 쿠폰을 적용할 수 있게 하려면 `withCoupon` 메소드를 사용하면 됩니다.

    $user->newSubscription('default', 'price_monthly')
         ->withCoupon('code')
         ->create($paymentMethod);

#### Adding Subscriptions
#### 구독 추가

If you would like to add a subscription to a customer who already has a default payment method set you can use the `add` method when using the `newSubscription` method:

이미 기본 결제 수단을 설정 한 고객에게 구독을 추가하려면 `newSubscription` 메서드을 사용할 때 `add` 메서드를 사용할 수 있습니다.

    $user = User::find(1);

    $user->newSubscription('default', 'price_premium')->add();

<a name="checking-subscription-status"></a>
### Checking Subscription Status
### 정기 구독 가입 상태 확인하기

Once a user is subscribed to your application, you may easily check their subscription status using a variety of convenient methods. First, the `subscribed` method returns `true` if the user has an active subscription, even if the subscription is currently within its trial period:

사용자가 정기 구독을 시작하고 난 뒤에, 이에 대한 정보를 확인하는 것은 메소드 하나로 손쉽게 확인이 가능합니다. 먼저 `subscribed` 메소드가 `true` 를 반환한다면 사용자의 정기구독 가입 상태는 무료 평가기간을 포함하여, 활성화 되어 있다는 것을 의미합니다.

    if ($user->subscribed('default')) {
        //
    }

The `subscribed` method also makes a great candidate for a [route middleware](/docs/{{version}}/middleware), allowing you to filter access to routes and controllers based on the user's subscription status:

`subscribed` 메소드는 [라우트 미들웨어](/docs/{{version}}/middleware)에 사용될수 있는 좋은 방법중 하나입니다. 사용자의 구독 상태에 따라서, 라우트 및 컨트롤러에 대한 액세스를 제한할 할 수 있습니다.

    public function handle($request, Closure $next)
    {
        if ($request->user() && ! $request->user()->subscribed('default')) {
            // This user is not a paying customer...
            return redirect('billing');
        }

        return $next($request);
    }

If you would like to determine if a user is still within their trial period, you may use the `onTrial` method. This method can be useful for displaying a warning to the user that they are still on their trial period:

사용자가 현재 무료 평가기간(trial) 을 통해서 이용중인지 아닌지 확인하고자 한다면, `onTrial` 메소드를 사용하면 됩니다. 이 메소드는 사용자들에게 그들이 현재 무료 평가기간을 이용중이라는 정보를 표시하는데 유용하게 사용될 수 있습니다.

    if ($user->subscription('default')->onTrial()) {
        //
    }

The `subscribedToPlan` method may be used to determine if the user is subscribed to a given plan based on a given Stripe Price ID. In this example, we will determine if the user's `default` subscription is actively subscribed to the `monthly` plan:

`subscribedToPlan` 메소드는 사용자가 주어진 Stripe 가격 ID에 대당하는 구독 플랜을 이용하는지 확인할 수 있습니다. 다음 예제는 사용자가 `default` 구독을 `monthly` plan 으로 구독하고 있는지 확인하게 됩니다.

    if ($user->subscribedToPlan('price_monthly', 'default')) {
        //
    }

By passing an array to the `subscribedToPlan` method, you may determine if the user's `default` subscription is actively subscribed to the `monthly` or the `yearly` plan:

`subscribedToPlan` 메소드에 배열을 전달하면 사용자의 `default`구독이 `monthly`또는 `yearly` plan이 활성화된 상태로 가입되어 있는지 확인할 수 있습니다.

    if ($user->subscribedToPlan(['price_monthly', 'price_yearly'], 'default')) {
        //
    }

The `recurring` method may be used to determine if the user is currently subscribed and is no longer within their trial period:

`recurring` 메소드는 사용자가 현재 구독 중이며 더이상 평가기간에 속해 있지 않은지를 확인할 때 사용 할 수 있습니다. 

    if ($user->subscription('default')->recurring()) {
        //
    }

#### Cancelled Subscription Status
#### 정기 구독 취소하기

To determine if the user was once an active subscriber, but has cancelled their subscription, you may use the `cancelled` method:

사용자가 이전에 한번 구독 후, 취소 했는지를 확인 하기 위해서 `cancelled` 메소드를 사용할 수 있습니다.

    if ($user->subscription('default')->cancelled()) {
        //
    }

You may also determine if a user has cancelled their subscription, but are still on their "grace period" until the subscription fully expires. For example, if a user cancels a subscription on March 5th that was originally scheduled to expire on March 10th, the user is on their "grace period" until March 10th. Note that the `subscribed` method still returns `true` during this time:

또한 사용자가 구독을 취소했지만 아직 완전히 만료 되기 전의 "유예 기간" 중인지를 확인할 수 있습니다. 예를 들어, 사용자가 3 월 5 일에 구독을 취소하고 3 월 10 일에 만료 될 경우, 해당 사용자는 3 월 10 일까지가 "유예 기간”입니다. `subscribed` 메소드는 이 기간 동안 여전히 `true` 를 반환하는 것에 유의하십시오.

    if ($user->subscription('default')->onGracePeriod()) {
        //
    }

To determine if the user has cancelled their subscription and is no longer within their "grace period", you may use the `ended` method:

사용자가 구독을 취소했으며 더 이상 "유예 기간"내에 있지 않은지 확인하려면 `ended` 메소드를 사용할 수 있습니다.

    if ($user->subscription('default')->ended()) {
        //
    }

#### Subscription Scopes
#### 구독 범위

Most subscription states are also available as query scopes so that you may easily query your database for subscriptions that are in a given state:

대부분의 구독상태는 쿼리 스코프로도 조회할 수 있으므로 상태에 맞는 구독을 데이터베이스에서 쉽게 조회 할 수 있습니다.

    // Get all active subscriptions...
    $subscriptions = Subscription::query()->active()->get();

    // Get all of the cancelled subscriptions for a user...
    $subscriptions = $user->subscriptions()->cancelled()->get();

A complete list of available scopes is available below:

사용 가능한 스코프의 전체 목록은 아래에서 확인할 수 있습니다.

    Subscription::query()->active();
    Subscription::query()->cancelled();
    Subscription::query()->ended();
    Subscription::query()->incomplete();
    Subscription::query()->notCancelled();
    Subscription::query()->notOnGracePeriod();
    Subscription::query()->notOnTrial();
    Subscription::query()->onGracePeriod();
    Subscription::query()->onTrial();
    Subscription::query()->pastDue();
    Subscription::query()->recurring();

<a name="incomplete-and-past-due-status"></a>
#### Incomplete and Past Due Status
#### 미완료 및 기한 만료 상태

If a subscription requires a secondary payment action after creation the subscription will be marked as `incomplete`. Subscription statuses are stored in the `stripe_status` column of Cashier's `subscriptions` database table.

구독 후 생성 된 보조 결제 수단이 필요한 경우 구독이 `incomplete`로 표시됩니다. 구독 상태는 Cashier의 `subscriptions` 데이터베이스 테이블의 `stripe_status` 컬럼에 저장됩니다.

Similarly, if a secondary payment action is required when swapping plans the subscription will be marked as `past_due`. When your subscription is in either of these states it will not be active until the customer has confirmed their payment. Checking if a subscription has an incomplete payment can be done using the `hasIncompletePayment` method on the Billable model or a subscription instance:

마찬가지로, 플랜을 변경 할 때 보조 결제 수단이 필요한 경우 구독은 `past_due`로 표시됩니다. 구독이 이 상태 중 하나에 있으면 고객이 지불을 확인하기 전까지는 구독이 활성화되지 않습니다. 청구 가능 모델 또는 구독 인스턴스에서 `hasIncompletePayment` 메소드를 사용하여 구독에 완료되지 않은 결제가 있는지 확인하십시오.

    if ($user->hasIncompletePayment('default')) {
        //
    }

    if ($user->subscription('default')->hasIncompletePayment()) {
        //
    }

When a subscription has an incomplete payment, you should direct the user to Cashier's payment confirmation page, passing the `latestPayment` identifier. You may use the `latestPayment` method available on subscription instance to retrieve this identifier:

구독에 결제가 완료되지 않았다면 `latestPayment`식별자를 전달하여 사용자를 Cashier의 결제 확인 페이지로 안내해야합니다. 구독 인스턴스에서 사용 가능한 `latestPayment` 메소드를 사용하여 이 식별자를 조회 할 수 있습니다.

    <a href="{{ route('cashier.payment', $subscription->latestPayment()->id) }}">
        Please confirm your payment.
    </a>

If you would like the subscription to still be considered active when it's in a `past_due` state, you may use the `keepPastDueSubscriptionsActive` method provided by Cashier. Typically, this method should be called in the `register` method of your `AppServiceProvider`:

`past_due`상태 일 때 구독을 계속 활성 상태로 유지하려면 Cashier에서 제공 한 `keepPastDueSubscriptionsActive` 메소드를 사용할 수 있습니다. 일반적으로이 메소드는 `AppServiceProvider`의 `register` 메소드에서 호출해야합니다.

    use Laravel\Cashier\Cashier;

    /**
     * Register any application services.
     *
     * @return void
     */
    public function register()
    {
        Cashier::keepPastDueSubscriptionsActive();
    }

> {note} When a subscription is in an `incomplete` state it cannot be changed until the payment is confirmed. Therefore, the `swap` and `updateQuantity` methods will throw an exception when the subscription is in an `incomplete` state.

> {note} 구독이 `incomplete`상태 인 경우 결제가 확인 될 때까지 구독을 변경할 수 없습니다. 따라서 `swap`및 `updateQuantity`메소드는 구독이 `incomplete`상태 인 경우 예외를 발생시킵니다.

<a name="changing-plans"></a>
### Changing Plans
### 정기 구독 유형 변경하기

After a user is subscribed to your application, they may occasionally want to change to a new subscription plan. To swap a user to a new subscription, pass the plan's price identifier to the `swap` method:

사용자가 애플리케이션을 구독한 뒤에, 구독 플랜을 변경하고자 하는 경우는 자주 있습니다. 사용자를 새로운 구독 플랜으로 변경하게 하려면 `swap` 메소드에 플랜의 가격 id를 전달하면 됩니다.

    $user = App\User::find(1);

    $user->subscription('default')->swap('provider-price-id');

If the user is on trial, the trial period will be maintained. Also, if a "quantity" exists for the subscription, that quantity will also be maintained.

사용자가 평가기간(trial)중이라면, 평가기간은 정상적으로 유지됩니다. 또한 구독의 "수량"이 존재하는 경우에도 이 수량은 유지됩니다.

If you would like to swap plans and cancel any trial period the user is currently on, you may use the `skipTrial` method:

구독 유형을 변경하고 사용자의 현재 구독 평가기간을 취소하려면 `skipTrial` 메소드를 하용하면 됩니다.

    $user->subscription('default')
            ->skipTrial()
            ->swap('provider-price-id');

If you would like to swap plans and immediately invoice the user instead of waiting for their next billing cycle, you may use the `swapAndInvoice` method:

Plan을 바꾸고 다음 청구주기를 기다리는 대신 사용자에게 즉시 송장을 보내려면 `swapAndInvoice` 메소드를 사용할 수 있습니다.

    $user = App\User::find(1);

    $user->subscription('default')->swapAndInvoice('provider-price-id');

#### Prorations
#### 비례 배분

By default, Stripe prorates charges when swapping between plans. The `noProrate` method may be used to update the subscription's without prorating the charges:

기본적으로, 스트라이프는 요금제를 교체 할 때 요금을 비례 배분하여 부과합니다. `noProrate`메소드는 요금을 비례 배분해서 생성하지 않고 구독을 업데이트하는 데 사용할 수 있습니다.

    $user->subscription('default')->noProrate()->swap('provider-price-id');

For more information on subscription proration, consult the [Stripe documentation](https://stripe.com/docs/billing/subscriptions/prorations).

구독 비례 배분에 대한 자세한 내용은 [스트라이프 문서](https://stripe.com/docs/billing/subscriptions/prorations)를 참조하십시오.

> {note} Executing the `noProrate` method before the `swapAndInvoice` method will have no affect on proration. An invoice will always be issued.

> {note} `swapAndInvoice` 메소드 이전에 `noProrate` 메소드를 실행해도 비례 배분에 영향을주지 않습니다. 인보이스는 항상 발행됩니다.

<a name="subscription-quantity"></a>
### Subscription Quantity
### 정기 구독 수량 변경하기

Sometimes subscriptions are affected by "quantity". For example, your application might charge $10 per month **per user** on an account. To easily increment or decrement your subscription quantity, use the `incrementQuantity` and `decrementQuantity` methods:

때로는 구독은 "수량"에 영향을 받을 수 있습니다. 예를 들어 여러분의 애플리케이션에서 하나의 계정의 **사용자마다** 한달에 10달러를 부과하고 있다고 한다면, `incrementQuantity` 와 `decrementQuantity` 메소드를 사용하여 구독 수를 늘리거나 줄일 수 있습니다.

    $user = User::find(1);

    $user->subscription('default')->incrementQuantity();

    // Add five to the subscription's current quantity...
    $user->subscription('default')->incrementQuantity(5);

    $user->subscription('default')->decrementQuantity();

    // Subtract five to the subscription's current quantity...
    $user->subscription('default')->decrementQuantity(5);

Alternatively, you may set a specific quantity using the `updateQuantity` method:

이 대신에, 특정 수량을 지정하고자 한다면, `updateQuantity` 메소드를 사용하면 됩니다.

    $user->subscription('default')->updateQuantity(10);

The `noProrate` method may be used to update the subscription's quantity without prorating the charges:

`noProrate` 메소드를 사용하면 요금에 영향을 주지 않으면서 구독의 수량을 수정 할 수 있습니다.

    $user->subscription('default')->noProrate()->updateQuantity(10);

For more information on subscription quantities, consult the [Stripe documentation](https://stripe.com/docs/subscriptions/quantities).

구독 수량에 대한 보다 자세한 내용은 [Stripe 문서](https://stripe.com/docs/subscriptions/quantities)를 참고하십시오.

> {note} Please note that when working with multiplan subscriptions, an extra "plan" parameter is required for the above quantity methods.

> {note} 멀티 플랜 구독으로 작업 할 때 위의 수량 메서드에 추가적인 "플랜" 파라메터가 필요합니다.

<a name="multiplan-subscriptions"></a>
### Multiplan Subscriptions
### 멀티 플랜 구독

[Multiplan subscriptions](https://stripe.com/docs/billing/subscriptions/multiplan) allow you to assign multiple billing plans to a single subscription. For example, imagine you are building a customer service "helpdesk" application that has a base subscription of $10 per month, but offers a live chat add-on plan for an additional $15 per month:

[멀티 플랜 구독](https://stripe.com/docs/billing/subscriptions/multiplan)을 사용하면 단일 구독에 여러 결제 플랜을 할당 할 수 있습니다. 예를 들어, "헬프 데스크"라는 고객 서비스 애플리케이션을 월 $10의 기본 구독료로 제공하고, 월 $15의 추가 요금으로 라이브 채팅 애드온을 제공한다고 가정 해보십시오.

    $user = User::find(1);

    $user->newSubscription('default', [
        'price_monthly',
        'chat-plan',
    ])->create($paymentMethod);

Now the customer will have two plans on their `default` subscription. Both plans will be charged for on their respective billing intervals. You can also use the `quantity` method to indicate the specific quantity for each plan:

이제 고객은 `default`구독에 대해 두 가지 플랜을 갖게됩니다. 두 플랜 모두 각각의 주기로 요금이 청구됩니다. `quantity`메서드를 사용하여 각 플랜에 특정 수량을 지정할 수도 있습니다.

    $user = User::find(1);

    $user->newSubscription('default', ['price_monthly', 'chat-plan'])
        ->quantity(5, 'chat-plan')
        ->create($paymentMethod);

Or, you may dynamically add the extra plan and its quantity using the `plan` method:

또는 `plan` 메소드를 사용하여 추가 플랜과 수량을 동적으로 추가 할 수 있습니다.

    $user = User::find(1);

    $user->newSubscription('default', 'price_monthly')
        ->plan('chat-plan', 5)
        ->create($paymentMethod);

Alternatively, you may add a new plan to an existing subscription at a later time:

또는 나중에 기존 구독에 새 플랜을 추가 할 수 있습니다.

    $user = User::find(1);

    $user->subscription('default')->addPlan('chat-plan');

The example above will add the new plan and the customer will be billed for it on their next billing cycle. If you would like to bill the customer immediately you may use the `addPlanAndInvoice` method:

위의 예는 새 요금제를 추가하고 고객에게 다음 결제주기에 요금을 청구합니다. 고객에게 즉시 청구하려면 `addPlanAndInvoice` 메소드를 사용할 수 있습니다.

    $user->subscription('default')->addPlanAndInvoice('chat-plan');

If you would like to add a plan with a specific quantity, you can pass the quantity as the second parameter of the `addPlan` or `addPlanAndInvoice` methods:

플랜에 수량을 추가하려면 `addPlan` 또는 `addPlanAndInvoice` 메소드의 두 번째 매개 변수로 수량을 추가하면 됩니다.

    $user = User::find(1);

    $user->subscription('default')->addPlan('chat-plan', 5);

You may remove plans from subscriptions using the `removePlan` method:

`removePlan` 메소드를 사용하여 구독에서 플랜을 제거 할 수 있습니다.

    $user->subscription('default')->removePlan('chat-plan');

> {note} You may not remove the last plan on a subscription. Instead, you should simply cancel the subscription.

> {note} 구독의 마지막 플랜은 제거 할 수 없습니다. 대신 구독을 취소하면 됩니다.

### Swapping
### 교환

You may also change the plans attached to a multiplan subscription. For example, imagine you're on a `basic-plan` subscription with a `chat-plan` add-on and you want to upgrade to the `pro-plan` plan:

멀티 플랜 구독에 첨부 된 플랜을 변경할 수도 있습니다. 예를 들어 `basic-plan`에 `chat-plan` 애드온을 추가하여 구독 중이고 `pro-plan` 플랜으로 업그레이드하려고한다고 가정 해보십시오.

    $user = User::find(1);

    $user->subscription('default')->swap(['pro-plan', 'chat-plan']);

When executing the code above, the underlying subscription item with the `basic-plan` is deleted and the one with the `chat-plan` is preserved. Additionally, a new subscription item for the new `pro-plan` is created.

위 코드를 실행하면 `basic-plan`의 기본 구독 항목이 삭제되고 `chat-plan`이 있는 항목은 유지됩니다. 또한 새로운 `pro-plan`에 대한 새 구독 항목이 생성됩니다.

You can also specify subscription item options. For example, you may need to specify the subscription plan quantities:

구독 항목 옵션을 지정할 수도 있습니다. 예를 들어, 구독 플랜 수량을 지정 할 수 있습니다.

    $user = User::find(1);

    $user->subscription('default')->swap([
        'pro-plan' => ['quantity' => 5],
        'chat-plan'
    ]);

If you want to swap a single plan on a subscription, you may do so using the `swap` method on the subscription item itself. This approach is useful if you, for example, want to preserve all of the existing metadata on the subscription item.

구독에서 단일 요금제를 교체하려면 구독 항목 자체에 `swap`메서드를 사용하면됩니다. 이 접근 방식은, 예를 들어, 구독 항목의 기존 메타 데이터를 모두 보존하려는 경우에 유용합니다.

    $user = User::find(1);

    $user->subscription('default')
            ->findItemOrFail('basic-plan')
            ->swap('pro-plan');

#### Proration
#### 비례 배분

By default, Stripe will prorate charges when adding or removing plans from a subscription. If you would like to make a plan adjustment without proration, you should chain the `noProrate` method onto your plan operation:

기본적으로 Stripe는 구독에서 요금제를 추가하거나 제거 할 때 요금을 비례 배분합니다. 비례 배분없이 플랜을 조정하려면 `noProrate` 메서드를 플랜 작업에 연결해야합니다.

    $user->subscription('default')->noProrate()->removePlan('chat-plan');

#### Quantities
#### 수량

If you would like to update quantities on individual subscription plans, you may do so using the [existing quantity methods](#subscription-quantity) and passing the name of the plan as an additional argument to the method:

개별 구독 플랜의 수량을 업데이트하려면 [수량 변경 메서드](#subscription-quantity)을 사용하고 플랜 이름을 추가 인수로 메소드에 전달하면됩니다.

    $user = User::find(1);

    $user->subscription('default')->incrementQuantity(5, 'chat-plan');

    $user->subscription('default')->decrementQuantity(3, 'chat-plan');

    $user->subscription('default')->updateQuantity(10, 'chat-plan');

> {note} When you have multiple plans set on a subscription the `stripe_plan` and `quantity` attributes on the `Subscription` model will be `null`. To access the individual plans, you should use the `items` relationship available on the `Subscription` model.

> {note} 구독에 여러 플랜이 설정되어있는 경우 `Subscription` 모델의 `stripe_plan` 및 `quantity` 속성은 `null`이 됩니다. 개별 요금제에 액세스하려면 `Subscription`모델에서 사용할 수있는 `items`관계를 사용해야합니다.

#### Subscription Items
#### 구독 항목

When a subscription has multiple plans, it will have multiple subscription "items" stored in your database's `subscription_items` table. You may access these via the `items` relationship on the subscription:

구독에 여러 플랜이있는 경우 데이터베이스의 `subscription_items` 테이블에 여러 구독 "항목-items"이 저장됩니다. 구독의 `items` 관계를 통해 액세스 할 수 있습니다.

    $user = User::find(1);

    $subscriptionItem = $user->subscription('default')->items->first();

    // Retrieve the Stripe plan and quantity for a specific item...
    $stripePlan = $subscriptionItem->stripe_plan;
    $quantity = $subscriptionItem->quantity;

You can also retrieve a specific plan using the `findItemOrFail` method:

`findItemOrFail` 메소드를 사용하여 특정 플랜을 검색 할 수도 있습니다.

    $user = User::find(1);

    $subscriptionItem = $user->subscription('default')->findItemOrFail('chat-plan');

<a name="subscription-taxes"></a>
### Subscription Taxes
### 정기구독의 세금계산

To specify the tax rates a user pays on a subscription, implement the `taxRates` method on your billable model, and return an array with the Tax Rate IDs. You can define these tax rates in [your Stripe dashboard](https://dashboard.stripe.com/test/tax-rates):

사용자가 구독에 대해 지불하는 세율을 지정하려면 청구 가능 모델에 `taxRates`메서드를 세율 ID가 있는 배열을 반환하도록 구현합니다. [스트라이프 대시 보드](http://dashboard.stripe.com/test/tax-rates)에서 다음 세율을 정의 할 수 있습니다.

    public function taxRates()
    {
        return ['tax-rate-id'];
    }

The `taxRates` method enables you to apply a tax rate on a model-by-model basis, which may be helpful for a user base that spans multiple countries and tax rates. If you're working with multiplan subscriptions you can define different tax rates for each plan by implementing a `planTaxRates` method on your billable model:

`taxRates` 메소드는 모델별로 세율을 적용하여 다양한 국가와 세율을 해당 국가의 사용자를 기반으로 적용할 수 이 있습니다. 멀티 플랜 구독으로 작업하는 경우 청구 가능 모델에 `planTaxRates`메소드를 구현하여 각 플랜에 대해 서로 다른 세율을 정의 할 수 있습니다.

    public function planTaxRates()
    {
        return [
            'plan-id' => ['tax-rate-id'],
        ];
    }

> {note} The `taxRates` method only applies to subscription charges. If you use Cashier to make "one off" charges, you will need to manually specify the tax rate at that time.

> {note} `taxRates` 메소드는 정기구독의 결제 시에만 적용됩니다. "한번 결제"에서 캐셔를 사용하는 경우 세율을 직접 적용해야합니다.

#### Syncing Tax Rates
#### 세율 동기화

When changing the hard-coded Tax Rate IDs returned by the `taxRates` method, the tax settings on any existing subscriptions for the user will remain the same. If you wish to update the tax value for existing subscriptions with the returned `taxTaxRates` values, you should call the `syncTaxRates` method on the user's subscription instance:

`taxRates` 메소드에 의해 반환 된 하드 코딩 된 세율의 ID를 변경할 때, 사용자를 위한 기존 구독에 대한 세금 설정은 동일하게 유지됩니다. 기존의 구독에 대한 세금 값을 반환 된 `taxTaxRates` 값으로 업데이트하려면 사용자의 구독 인스턴스에서 `syncTaxRates` 메소드를 호출해야합니다.

    $user->subscription('default')->syncTaxRates();

This will also sync any subscription item tax rates so make sure you also properly change the `planTaxRates` method.

이렇게하면 구독 항목의 세율도 동기화되므로 `planTaxRates` 메서드도 올바르게 변경해야합니다.

#### Tax Exemption
#### 면세

Cashier also offers methods to determine if the customer is tax exempt by calling the Stripe API. The `isNotTaxExempt`, `isTaxExempt`, and `reverseChargeApplies` methods are available on the billable model:

캐셔는 Stripe API를 호출하여 고객이 면세 여부를 확인하는 메서드도 제공합니다. `isNotTaxExempt`, `isTaxExempt` 및 `reverseChargeApplies` 메소드는 청구 가능 모델에서 사용할 수 있습니다.

    $user = User::find(1);

    $user->isTaxExempt();
    $user->isNotTaxExempt();
    $user->reverseChargeApplies();

These methods are also available on any `Laravel\Cashier\Invoice` object. However, when calling these methods on an `Invoice` object the methods will determine the exemption status at the time the invoice was created.

이러한 메서드는 모든 `Laravel\Cashier\Invoice` 객체에서도 사용할 수 있습니다. 그러나 `Invoice` 객체에서 이러한 메서드를 호출하면 메서드가 송장이 생성 된 시점의 면세 상태를 결정합니다.

<a name="subscription-anchor-date"></a>
### Subscription Anchor Date
### 정기 구독의 고정일

By default, the billing cycle anchor is the date the subscription was created, or if a trial period is used, the date that the trial ends. If you would like to modify the billing anchor date, you may use the `anchorBillingCycleOn` method:

일반적으로 과금 주기의 고정일은 정기 구독이 시작 된 날짜 또는 평가기간이 있는 경우 평가기간이 종료되는 날짜입니다. 청구서의 고정일을 수정하려면 `anchorBillingCycleOn` 메소드를 사용할 수 있습니다.

    use App\User;
    use Carbon\Carbon;

    $user = User::find(1);

    $anchor = Carbon::parse('first day of next month');

    $user->newSubscription('default', 'price_premium')
                ->anchorBillingCycleOn($anchor->startOfDay())
                ->create($paymentMethod);

For more information on managing subscription billing cycles, consult the [Stripe billing cycle documentation](https://stripe.com/docs/billing/subscriptions/billing-cycle)

정기 구독의 결제주기 관리에 대한 자세한 내용은 [Stripe billing cycle documentation](https://stripe.com/docs/billing/subscriptions/billing-cycle) 에서 확인이 가능합니다

<a name="cancelling-subscriptions"></a>
### Cancelling Subscriptions
### 정기구독 취소하기

To cancel a subscription, call the `cancel` method on the user's subscription:

정기구독을 취소하기 위해서는, 사용자의 정기구독에 대해서 `cancle` 메소드를 호출하면 됩니다.

    $user->subscription('default')->cancel();

When a subscription is cancelled, Cashier will automatically set the `ends_at` column in your database. This column is used to know when the `subscribed` method should begin returning `false`. For example, if a customer cancels a subscription on March 1st, but the subscription was not scheduled to end until March 5th, the `subscribed` method will continue to return `true` until March 5th.

구독이 취소되면 캐셔는 자동으로 데이터베이스의 `ends_at` 컬럼을 설정합니다. 이 컬럼은 언제 `subscribed` 메소드가 `false`를 반환해야 하는지 알기 위해서 사용되어 집니다. 예를 들어 사용자가 구독을 3월 1일에 취소했지만, 정기 구독이 3월 5일에 종료하도록 예정되어 있다면 `subscribed` 메소드는 3월 5일까지 `true`를 반환할 것입니다.

You may determine if a user has cancelled their subscription but are still on their "grace period" using the `onGracePeriod` method:

사용자가 구독을 취소했지만 아직 "유예 기간"이 남아 있는지 확인하고자 한다면, `onGracePeriod` 메소드를 사용하면 됩니다.

    if ($user->subscription('default')->onGracePeriod()) {
        //
    }

If you wish to cancel a subscription immediately, call the `cancelNow` method on the user's subscription:

정기구독을 즉시 취소하고자 한다면, 사용자의 정기구독 에서 `cancelNow` 메소드를 호출하면 됩니다.

    $user->subscription('default')->cancelNow();

<a name="resuming-subscriptions"></a>
### Resuming Subscriptions
### 정기 구독 재개하기

If a user has cancelled their subscription and you wish to resume it, use the `resume` method. The user **must** still be on their grace period in order to resume a subscription:

만약 사용자가 취소한 구독을 재개하려면, `resume` 메소드를 사용하면 됩니다. 사용자는 구독을 재개하기 위해 **반드시** 자신의 유예 기간에 내에 있어야 합니다.

    $user->subscription('default')->resume();

If the user cancels a subscription and then resumes that subscription before the subscription has fully expired, they will not be billed immediately. Instead, their subscription will be re-activated, and they will be billed on the original billing cycle.

만약 사용자가 구독을 취소하고 다음 정기 구독을 재개하는 경우 그 등록의 만료일이 되기 전까지는 비용이 바로 부과되지는 않습니다. 대신, 사용자의 정기 구독은 재활성화 되며, 원래의 주기에 따라 과금됩니다.

<a name="subscription-trials"></a>
## Subscription Trials
## 구독 평가기간

<a name="with-payment-method-up-front"></a>
### With Payment Method Up Front
### 결제 수단 사전 등록

If you would like to offer trial periods to your customers while still collecting payment method information up front, you should use the `trialDays` method when creating your subscriptions:

고객에게 평가기간을 제공하고, 결제 정보를 사전에 등록 해달라고 요청하고자 한다면, 구독을 생성할 때 `trialDays` 메소드를 해야합니다.

    $user = User::find(1);

    $user->newSubscription('default', 'price_monthly')
                ->trialDays(10)
                ->create($paymentMethod);

This method will set the trial period ending date on the subscription record within the database, as well as instruct Stripe to not begin billing the customer until after this date. When using the `trialDays` method, Cashier will overwrite any default trial period configured for the plan in Stripe.

이 메소드는 데이터베이스 안의 구독 레코드에 평가기간의 종료일을 설정하고, Stripe 에게 이 기간이 지나기 전까지 고객에게 청구하지 않도록 지시합니다. `trialDays` 메소드를 사용할 때 캐셔는 Stripe의 정책에 의해 설정된 기본 평가기간을 덮어 씁니다.

> {note} If the customer's subscription is not cancelled before the trial ending date they will be charged as soon as the trial expires, so you should be sure to notify your users of their trial ending date.

> {note} 평가기간 종료 이전에 고객이 구독을 취소하지 않으면 평가기간이 만료되는 즉시 요금이 부과되므로, 평가기간의 종료일을 사용자에게 공지해야합니다.

The `trialUntil` method allows you to provide a `DateTime` instance to specify when the trial period should end:

`trialUntil` 메소드는 평가기간이 언제 종료되어야 하는지 지정하는 `DateTime` 인스턴스를 인자로 전달받습니다.

    use Carbon\Carbon;

    $user->newSubscription('default', 'price_monthly')
                ->trialUntil(Carbon::now()->addDays(10))
                ->create($paymentMethod);

You may determine if the user is within their trial period using either the `onTrial` method of the user instance, or the `onTrial` method of the subscription instance. The two examples below are identical:

사용자가 현재 평가기간 중인지 확인하려면, 사용자 인스턴스에서 `onTrial` 메소드를 사용하거나, 구독 인스턴스에서 `onTrial` 메소드를 사용하면 됩니다. 다음의 두 예제는 동일합니다.

    if ($user->onTrial('default')) {
        //
    }

    if ($user->subscription('default')->onTrial()) {
        //
    }

#### Defining Trial Days In Stripe / Cashier
#### Stripe / 캐셔에서 평가기간 정의

You may choose to define how many trial days your plan's receive in the Stripe dashboard or always pass them explicitly using Cashier. If you choose to define your plan's trial days in Stripe you should be aware that new subscriptions, including new subscriptions for a customer that had a subscription in the past, will always receive a trial period unless you explicitly call the `trialDays(0)` method.

Stripe 대시 보드에서 플랜의 평가기간를 정의하거나 항상 캐셔를 사용하여 명시적으로 전달할 수 있습니다. Stripe에서 플랜의 평가판 기간을 정의하기로 선택한 경우, 과거에 구독한 고객에 대한 새 구독을 포함하여 새 구독은 명시적으로 `trialDays(0)`메서드를 호출하지 않는 한 항상 평가 기간을 받게됩니다.

<a name="without-payment-method-up-front"></a>
### Without Payment Method Up Front
### 결제 수단없이 사전 등록

If you would like to offer trial periods without collecting the user's payment method information up front, you may set the `trial_ends_at` column on the user record to your desired trial ending date. This is typically done during user registration:

고객에게 신용카드에 대한 결제 정보의 사전등록 없이 평가기간을 부여하고자 한다면, 사용자의 `trial_ends_at` 컬럼에 트리이얼 종료기간을 설정하면 됩니다. 이는 일반적으로 사용자를 등록할 때 설정하게 됩니다.

    $user = User::create([
        // Populate other user properties...
        'trial_ends_at' => now()->addDays(10),
    ]);

> {note} Be sure to add a [date mutator](/docs/{{version}}/eloquent-mutators#date-mutators) for `trial_ends_at` to your model definition.

> {note} 모델 정의에 `trial_ends_at`이 [날짜 mutator](/docs/{{version}}/eloquent-mutators#date-mutators)로 추가되어 있어야 합니다.

Cashier refers to this type of trial as a "generic trial", since it is not attached to any existing subscription. The `onTrial` method on the `User` instance will return `true` if the current date is not past the value of `trial_ends_at`:

기존 구독에 연결되는 것이 아니기 때문에, Cashier에서는 이 타입의 평가기간을 "포괄적 평가기간(generic trial)"이라고 지칭하고 있습니다. `User` 인스턴스의 `onTrial` 메소드는 만약 현재 시간이 아직 `trial_ends_at`을 넘기지 않았다면 `true`를 반환합니다.

    if ($user->onTrial()) {
        // User is within their trial period...
    }

You may also use the `onGenericTrial` method if you wish to know specifically that the user is within their "generic" trial period and has not created an actual subscription yet:

또한 특별히 사용자가 현재 "포괄적" 평가기간 중이며 아직 실제 구독을 생성하지 않았는지 알고자 한다면 `onGenericTrial` 메소드를 사용할 수 있습니다.

    if ($user->onGenericTrial()) {
        // User is within their "generic" trial period...
    }

Once you are ready to create an actual subscription for the user, you may use the `newSubscription` method as usual:

사용자에게 실제 구독을 생성할 준비가 되면, 평소대로 `newSubscription` 메소드를 사용하면됩니다.

    $user = User::find(1);

    $user->newSubscription('default', 'price_monthly')->create($paymentMethod);

<a name="extending-trials"></a>
### Extending Trials
### 평가기간 연장

The `extendTrial` method allows you to extend the trial period of a subscription after it's been created:

`extendTrial` 메소드를 사용하면 구독 생성 후 평가기간을 연장 할 수 있습니다.

    // End the trial 7 days from now...
    $subscription->extendTrial(
        now()->addDays(7)
    );

    // Add an additional 5 days to the trial...
    $subscription->extendTrial(
        $subscription->trial_ends_at->addDays(5)
    );

If the trial has already expired and the customer is already being billed for the subscription, you can still offer them an extended trial. The time spent within the trial period will be deducted from the customer's next invoice.

평가판이 이미 만료되었고 고객이 이미 구독 요금을 청구하는 경우에도 평가판을 연장하여 제공 할 수 있습니다. 평가판 기간에 포함 된 시간은 고객의 다음 청구서에서 공제됩니다.

<a name="handling-stripe-webhooks"></a>
## Handling Stripe Webhooks
## Stripe webhook 처리하기

> {tip} You may use [the Stripe CLI](https://stripe.com/docs/stripe-cli) to help test webhooks during local development.

> {tip} [the Stripe CLI](https://stripe.com/docs/stripe-cli)을 사용하여 로컬 개발 도중에도 웹훅을 테스트 할 수 있습니다.

Stripe can notify your application of a variety of events via webhooks. By default, a route that points to Cashier's webhook controller is configured through the Cashier service provider. This controller will handle all incoming webhook requests.

Stripe은 웹훅을 통해 다양한 이벤트를 애플리케이션에 알릴 수 있습니다. 기본적으로 Cashier의 웹훅 컨트롤러를 가리키는 경로는 Cashier 서비스 프로바이더를 통해 설정됩니다. 이 컨트롤러는 들어오는 모든 웹훅 요청을 처리합니다.

By default, this controller will automatically handle cancelling subscriptions that have too many failed charges (as defined by your Stripe settings), customer updates, customer deletions, subscription updates, and payment method changes; however, as we'll soon discover, you can extend this controller to handle any webhook event you like.

기본적으로 이 컨트롤러는 너무 많은 청구 실패 (스트라이프 설정에 의해 정의 됨), 고객 업데이트, 고객 삭제, 가입 업데이트 및 결제 수단 변경이 있는 가입 취소를 자동으로 처리합니다. 그러나 곧 알게 되겠지만 원하는 웹훅 이벤트를 처리하도록 이 컨트롤러를 확장 할 수 있습니다.

To ensure your application can handle Stripe webhooks, be sure to configure the webhook URL in the Stripe control panel. By default, Cashier's webhook controller listens to the `/stripe/webhook` URL path. The full list of all webhooks you should configure in the Stripe control panel are:

애플리케이션이 Stripe 웹훅을 처리 할 수 있도록하려면 Stripe 제어판에서 webhook URL을 구성하십시오. 기본적으로 캐셔의 웹훅 컨트롤러는 `/stripe/webhook` URL 경로로 수신됩니다. Stripe 제어판에서 구성해야하는 모든 웹훅의 전체 목록은 다음과 같습니다.

- `customer.subscription.updated`
- `customer.subscription.deleted`
- `customer.updated`
- `customer.deleted`
- `invoice.payment_action_required`

> {note} Make sure you protect incoming requests with Cashier's included [webhook signature verification](/docs/{{version}}/billing#verifying-webhook-signatures) middleware.

> {note} 캐셔에 포함된 [웹훅 서명 확인](/docs/{{version}}/billing#verifying-webhook-signatures) 미들웨어를 사용하는 것을 확인하십시오.

#### Webhooks & CSRF Protection
#### Webhook & CSRF 보호

Since Stripe webhooks need to bypass Laravel's [CSRF protection](/docs/{{version}}/csrf), be sure to list the URI as an exception in your `VerifyCsrfToken` middleware or list the route outside of the `web` middleware group:

Stripe webhook은 라라벨의 [CSRF 보호](/docs/{{version}}/csrf)를 우회해야하기 때문에, `VerifyCsrfToken` 미들웨어에서 예외 URI를 등록하거나, 라우트를 `web` 미들웨어 그룹 외부에 정의하십시오:

    protected $except = [
        'stripe/*',
    ];

<a name="defining-webhook-event-handlers"></a>
### Defining Webhook Event Handlers
### Webhook 이벤트 핸들러 정의하기

Cashier automatically handles subscription cancellation on failed charges, but if you have additional webhook events you would like to handle, extend the Webhook controller. Your method names should correspond to Cashier's expected convention, specifically, methods should be prefixed with `handle` and the "camel case" name of the webhook you wish to handle. For example, if you wish to handle the `invoice.payment_succeeded` webhook, you should add a `handleInvoicePaymentSucceeded` method to the controller:

캐셔는 결제가 실패하면 자동으로 구독을 취소처리하지만, 처리하고자 하는 추가적인 webhook을 가지고 있다면, Webhook 컨트롤러를 확장하면 됩니다. 메소드 이름은 캐셔의 컨벤션과 일치해야 하고, 특히 메소드는 `handle` 로 시작해야하며 처리하고자 하는 webhook을 "카멜 케이스"로 된 이름이어야 합니다. 예를 들어 `invoice.payment_succeeded` webhook을 처리하고자 한다면, 컨트롤러에 `handleInvoicePaymentSucceeded` 메소드를 추가해야 합니다.

    <?php

    namespace App\Http\Controllers;

    use Laravel\Cashier\Http\Controllers\WebhookController as CashierController;

    class WebhookController extends CashierController
    {
        /**
         * Handle invoice payment succeeded.
         *
         * @param  array  $payload
         * @return \Symfony\Component\HttpFoundation\Response
         */
        public function handleInvoicePaymentSucceeded($payload)
        {
            // Handle The Event
        }
    }

Next, define a route to your Cashier controller within your `routes/web.php` file. This will overwrite the default shipped route:

다음으로 `routes/web.php` 파일에서 캐셔 컨트롤러에 대한 라우트를 정의하십시오. 이것은 기본으로 전달되는 라우트를 덮어 씁니다.

    Route::post(
        'stripe/webhook',
        '\App\Http\Controllers\WebhookController@handleWebhook'
    );

Cashier emits a `Laravel\Cashier\Events\WebhookReceived` event when a webhook is received, and a `Laravel\Cashier\Events\WebhookHandled` event when a webhook was handled by Cashier. Both events contain the full payload of the Stripe webhook.

Cashier는 웹훅이 수신되면 `Laravel\Cashier\Events\WebhookReceived` 이벤트를, Cashier가 웹훅을 처리하면 `Laravel\Cashier\Events\WebhookHandled` 이벤트를 생성합니다. 두 이벤트 모두 Stripe 웹훅의 전체 페이로드를 포함합니다.

<a name="handling-failed-subscriptions"></a>
### Failed Subscriptions
### 실패한 정기구독

What if a customer's credit card expires? No worries - Cashier's Webhook controller will cancel the customer's subscription for you. Failed payments will automatically be captured and handled by the controller. The controller will cancel the customer's subscription when Stripe determines the subscription has failed (normally after three failed payment attempts).

고객의 신용 카드가 만료되면 어떻게됩니까? 걱정하지 마십시오. Cashier의 웹훅 컨트롤러는 고객의 구독을 취소합니다. 결제 실패는 컨트롤러에 의해 자동으로 캡처 및 처리됩니다. Stripe은 구독이 실패했다고 판단하면 (일반적으로 3 번의 지불 시도 실패 후) 고객의 구독을 취소합니다.

<a name="verifying-webhook-signatures"></a>
### Verifying Webhook Signatures
### Webhook의 서명 검증

To secure your webhooks, you may use [Stripe's webhook signatures](https://stripe.com/docs/webhooks/signatures). For convenience, Cashier automatically includes a middleware which validates that the incoming Stripe webhook request is valid.

안전한 webhook을 사용하기 위해 [Stripe's webhook signatures](https://stripe.com/docs/webhooks/signatures)를 사용할 수 있습니다. Cashier에는 유입되는 Stripe webhook 요청이 유효한지 자동으로 확인하는 미들웨어를 포함하고 있습니다.

To enable webhook verification, ensure that the `STRIPE_WEBHOOK_SECRET` environment variable is set in your `.env` file. The webhook `secret` may be retrieved from your Stripe account dashboard.

웹훅의 검증을 활성화하려면 `.env` 파일에 `STRIPE_WEBHOOK_SECRET` 환경 변수가 설정되어 있는지 확인하십시오. webhook 의 `secret`은 Stripe 계정 대시보드에서 조회 할 수 있습니다.

<a name="single-charges"></a>
## Single Charges
## 한번만 결제하기

<a name="simple-charge"></a>
### Simple Charge
### 간단한 결제

> {note} The `charge` method accepts the amount you would like to charge in the **lowest denominator of the currency used by your application**.

>> {note} `charge` 메소드는 **애플리케이션에서 사용하는 통화의 최저 denominator** 로 청구하려는 금액을 수락합니다.

If you would like to make a "one off" charge against a subscribed customer's payment method, you may use the `charge` method on a billable model instance. You'll need to [provide a payment method identifier](#storing-payment-methods) as the second argument:

가입한 고객의 지불 수단에 대해 "일회성"청구를하려면 청구 가능 모델 인스턴스에서 `charge` 메소드를 사용할 수 있습니다. 두 번째 인수로 [결제 수단 식별자](#storing-payment-methods)를 제공해야합니다.

    // Stripe Accepts Charges In Cents...
    $stripeCharge = $user->charge(100, $paymentMethod);

The `charge` method accepts an array as its third argument, allowing you to pass any options you wish to the underlying Stripe charge creation. Consult the Stripe documentation regarding the options available to you when creating charges:

`charge` 메소드는 배열을 세 번째 인수로 받아들이므로 원하는 모든 옵션을 기본 Strip charge 생성에 전달할 수 있습니다. 청구 작성시 사용 가능한 옵션에 대해서는 Stripe 문서를 참조하십시오.

    $user->charge(100, $paymentMethod, [
        'custom_option' => $value,
    ]);

You may also use the `charge` method without an underlying customer or user:

기본 고객이나 사용자없이 `charge` 메서드를 사용할 수도 있습니다.

    use App\User;

    $stripeCharge = (new User)->charge(100, $paymentMethod);

The `charge` method will throw an exception if the charge fails. If the charge is successful, an instance of `Laravel\Cashier\Payment` will be returned from the method:

충전이 실패하면 `charge` 메소드에서 예외가 발생합니다. 청구가 성공하면 `Laravel\Cashier\Payment` 인스턴스가 메소드에서 리턴됩니다.

    try {
        $payment = $user->charge(100, $paymentMethod);
    } catch (Exception $e) {
        //
    }

<a name="charge-with-invoice"></a>
### Charge With Invoice
### 결제와 청구서

Sometimes you may need to make a one-time charge but also generate an invoice for the charge so that you may offer a PDF receipt to your customer. The `invoiceFor` method lets you do just that. For example, let's invoice the customer $5.00 for a "One Time Fee":

때로는 한 번만 결제를 진행하면서 고객에게 PDF 영수증을 발행하고자 할 수도 있습니다. `invoiceFor` 메소드는 바로 이경우 사용합니다. 예를 들어, 고객에게 "한번의 요금"을 위해서 5달러를 청구해 보겠습니다.

    // Stripe Accepts Charges In Cents...
    $user->invoiceFor('One Time Fee', 500);

The invoice will be charged immediately against the user's default payment method. The `invoiceFor` method also accepts an array as its third argument. This array contains the billing options for the invoice item. The fourth argument accepted by the method is also an array. This final argument accepts the billing options for the invoice itself:

청구서는 사용자의 청구수단에 즉시 발행됩니다. `invoiceFor` 메소드는 세번째 인자로 배열을 받습니다. 이 배열에는 송장 항목에 대한 청구 옵션이 있습니다. 메소드의 네 번째 인자는 배열이며 이 마지막 인자는 인보이스 자체에 대한 청구 옵션을 받습니다.

    $user->invoiceFor('Stickers', 500, [
        'quantity' => 50,
    ], [
        'default_tax_rates' => ['tax-rate-id'],
    ]);

> {note} The `invoiceFor` method will create a Stripe invoice which will retry failed billing attempts. If you do not want invoices to retry failed charges, you will need to close them using the Stripe API after the first failed charge.

> {note} `invoiceFor` 메소드는 결제 시도가 실패한 경우 이를 재시도하는 Stripe 청구서를 생성할 것입니다. 실패한 결제의 재시도를 위한 청구를 수행하지 않으려면, 결제 시도가 실패한 뒤에 Stripe API를 사용하여 청구서를 종료(close) 해야 합니다.

<a name="refunding-charges"></a>
### Refunding Charges
### 환불 수수료

If you need to refund a Stripe charge, you may use the `refund` method. This method accepts the Stripe Payment Intent ID as its first argument:

Stripe의 결제를 환불해야하는 경우 `refund` 메소드를 사용할 수 있습니다. 이 메소드는 첫 번째 인수로 Stripe Payment Intent ID를 입력 받습니다.

    $payment = $user->charge(100, $paymentMethod);

    $user->refund($payment->id);

<a name="invoices"></a>
## Invoices
## 청구서

<a name="retrieving-invoices"></a>
### Retrieving Invoices
### 송장 검색

You may easily retrieve an array of a billable model's invoices using the `invoices` method:

`invoices` 메소드를 사용하여 청구가 가능한 모델의 청구서를 손쉽게 배열 형태로 조회 할 수 있습니다.

    $invoices = $user->invoices();

    // Include pending invoices in the results...
    $invoices = $user->invoicesIncludingPending();

You may use the `findInvoice` method to retrieve a specific invoice:

`findInvoice` 메소드를 사용하여 특정 인보이스를 검색 할 수 있습니다.

    $invoice = $user->findInvoice($invoiceId);

#### Displaying Invoice Information
#### 송장 정보 표시

When listing the invoices for the customer, you may use the invoice's helper methods to display the relevant invoice information. For example, you may wish to list every invoice in a table, allowing the user to easily download any of them:

사용자의 청구서를 나열하는 경우, 청구서 관련 정보를 표시하기 위해서 청구서의 헬퍼 함수를 사용할 수 있습니다. 예를 들어 사용자가 쉽게 다운로드 할 수 있도록 테이블 내의 모든 청구서를 목록으로 보여주고자 할 수도 있습니다.

    <table>
        @foreach ($invoices as $invoice)
            <tr>
                <td>{{ $invoice->date()->toFormattedDateString() }}</td>
                <td>{{ $invoice->total() }}</td>
                <td><a href="/user/invoice/{{ $invoice->id }}">Download</a></td>
            </tr>
        @endforeach
    </table>

<a name="generating-invoice-pdfs"></a>
### Generating Invoice PDFs
### 청구서 PDF 로 생성하기

From within a route or controller, use the `downloadInvoice` method to generate a PDF download of the invoice. This method will automatically generate the proper HTTP response to send the download to the browser:

라우트 또는 컨트롤러 안에서 `downloadInvoice` 메소드를 사용하여 청구서를 PDF 로 생성하고 다운로드 할 수 있습니다. 이 메소드는 자동으로 브라우저서 다운로드 할 수 있는 HTTP 응답을 생성합니다.

    use Illuminate\Http\Request;

    Route::get('user/invoice/{invoice}', function (Request $request, $invoiceId) {
        return $request->user()->downloadInvoice($invoiceId, [
            'vendor' => 'Your Company',
            'product' => 'Your Product',
        ]);
    });

The `downloadInvoice` method also allows for an optional custom filename as the third parameter. This filename will automatically be suffixed with `.pdf` for you:

`downloadInvoice` 메소드는 세 번째 파라메터로 파일이름을 지정할 수도 있습니다. 파일 이름은 `.pdf`가 자동으로 끝에 추가됩니다.

    return $request->user()->downloadInvoice($invoiceId, [
        'vendor' => 'Your Company',
        'product' => 'Your Product',
    ], 'my-invoice');

<a name="handling-failed-payments"></a>
## Handling Failed Payments
## 결제 실패 처리

Sometimes, payments for subscriptions or single charges can fail. When this happens, Cashier will throw an `IncompletePayment` exception that informs you that this happened. After catching this exception, you have two options on how to proceed.

때로는 구독 결제 또는 단일 청구에 실패 할 수 있습니다. 이런 일이 발생하면 캐셔은 이 문제가 발생했음을 알리는 `IncompletePayment`예외를 발생시킵니다. 이 예외를 발생시킨 후 처리 방법에 대한 두 가지 옵션이 있습니다.

First, you could redirect your customer to the dedicated payment confirmation page which is included with Cashier. This page already has an associated route that is registered via Cashier's service provider. So, you may catch the `IncompletePayment` exception and redirect to the payment confirmation page:

첫번째로는, 고객을 Cashier에 포함 된 결제 확인 전용 페이지로 리디렉션 할 수 있습니다. 이 페이지는 Cashier의 서비스 프로바이더를 통해 이미 관련된 경로가 등록되어 있습니다. 따라서 `IncompletePayment`예외가 발생하여 결제 확인 페이지로 리디렉션 될 수 있습니다.

    use Laravel\Cashier\Exceptions\IncompletePayment;

    try {
        $subscription = $user->newSubscription('default', $planId)
                                ->create($paymentMethod);
    } catch (IncompletePayment $exception) {
        return redirect()->route(
            'cashier.payment',
            [$exception->payment->id, 'redirect' => route('home')]
        );
    }

On the payment confirmation page, the customer will be prompted to enter their credit card info again and perform any additional actions required by Stripe, such as "3D Secure" confirmation. After confirming their payment, the user will be redirected to the URL provided by the `redirect` parameter specified above. Upon redirection, `message` (string) and `success` (integer) query string variables will be added to the URL.

결제 확인 페이지에서 고객에게 신용 카드 정보를 다시 입력하라는 메시지가 표시되고 "3D 보안" 확인과 같이 Stripe에 필요한 추가 작업을 수행하라는 메시지가 표시됩니다. 결제를 확인한 후 사용자는 위에 지정된 `redirect` 파라메터가 제공한 URL로 리디렉션됩니다. 리디렉션시 `message` (문자열) 및 `success` (정수) 쿼리스트링 변수가 URL에 추가됩니다.

Alternatively, you could allow Stripe to handle the payment confirmation for you. In this case, instead of redirecting to the payment confirmation page, you may [setup Stripe's automatic billing emails](https://dashboard.stripe.com/account/billing/automatic) in your Stripe dashboard. However, if a `IncompletePayment` exception is caught, you should still inform the user they will receive an email with further payment confirmation instructions.

또는 Stripe이 지불 확인을 처리하도록 허용 할 수 있습니다. 이 경우 결제 확인 페이지로 리디렉션하는 대신 스트라이프 대시 보드에서 [스트라이프 자동 결제 이메일 설정](https://dashboard.stripe.com/account/billing/automatic) 을 할 수 있습니다. 그러나 `IncompletePayment`예외가 발생하더라도 사용자에게 추가 지불 확인 지시 사항이 포함 된 이메일을 수신하도록 알려야합니다.

Payment exceptions may be thrown for the following methods: `charge`, `invoiceFor`, and `invoice` on the `Billable` user. When handling subscriptions, the `create` method on the `SubscriptionBuilder`, and the `incrementAndInvoice` and `swapAndInvoice` methods on the `Subscription` model may throw exceptions.

`Billable` 사용자의 `charge`, `invoiceFor` 및  `invoice`메서드에 대해서는 지불 예외가 발생할 수 있습니다. 구독을 처리 할 때 `SubscriptionBuilder`의`create` 메소드와 `Subscription` 모델의 `incrementAndInvoice` 및 `swapAndInvoice` 메소드에서 예외가 발생할 수 있습니다.

There are currently two types of payment exceptions which extend `IncompletePayment`. You can catch these separately if needed so that you can customize the user experience:

`IncompletePayment`를 확장하는 두 가지 유형의 지불 예외가 있습니다. 사용자 경험을 커스터마이징 할 수 있도록 필요한 경우 이를 별도로 처리할 수 있습니다.


- `PaymentActionRequired`: this indicates that Stripe requires extra verification in order to confirm and process a payment.
- `PaymentFailure`: this indicates that a payment failed for various other reasons, such as being out of available funds.

- `PaymentActionRequired`: 결제를 확인하고 처리하기 위해 Stripe에서 추가 확인이 필요함을 나타냅니다.
- `PaymentFailure`: 사용 가능한 자금이 부족한 등 여러 가지 이유로 결제가 실패했음을 나타냅니다.

<a name="strong-customer-authentication"></a>
## Strong Customer Authentication
## 강력한 고객 인증

If your business is based in Europe you will need to abide by the Strong Customer Authentication (SCA) regulations. These regulations were imposed in September 2019 by the European Union to prevent payment fraud. Luckily, Stripe and Cashier are prepared for building SCA compliant applications.

비즈니스가 유럽에 기반을 둔 경우 강력한 고객 인증 (SCA) 규정을 준수해야합니다. 이 규정은 2019년 9월 유럽 연합에 의해 지불 사기를 방지하기 위해 부과되었습니다. 운 좋게도 Stripe과 Cashier는 SCA 호환 애플리케이션을 구축 할 준비가되어 있습니다.

> {note} Before getting started, review [Stripe's guide on PSD2 and SCA](https://stripe.com/guides/strong-customer-authentication) as well as their [documentation on the new SCA APIs](https://stripe.com/docs/strong-customer-authentication).

> {note} 시작하기 전에 [PS2 및 SCA에 대한 Stripe 안내서](https://stripe.com/guides/strong-customer-authentication)와 [새로운 SCA API에 대한 문서](https://stripe.com/docs/strong-customer-authentication) 를 검토하십시오.

<a name="payments-requiring-additional-confirmation"></a>
### Payments Requiring Additional Confirmation
### 추가 확인이 필요한 결제

SCA regulations often require extra verification in order to confirm and process a payment. When this happens, Cashier will throw a `PaymentActionRequired` exception that informs you that this extra verification is needed. More info on how to handle these exceptions be found [here](#handling-failed-payments).

SCA 규정은 종종 지불을 확인하고 처리하기 위해 추가 검증이 필요합니다. 이런 일이 발생하면 캐셔는 이 추가 확인이 필요하다는 것을 알려주는 `PaymentActionRequired` 예외를 처리합니다. 이러한 예외를 처리하는 방법에 대한 자세한 내용은 [여기](#handling-failed-payments)를 참조하십시오.

#### Incomplete and Past Due State
#### 미완료 및 기한 만료 상태

When a payment needs additional confirmation, the subscription will remain in an `incomplete` or `past_due` state as indicated by its `stripe_status` database column. Cashier will automatically activate the customer's subscription via a webhook as soon as payment confirmation is complete.

결제에 추가 확인이 필요한 경우, 구독은 `stripe_status` 데이터베이스 컬럼에 표시된 것처럼 `incomplete` 또는 `past_due`상태로 유지됩니다. Cashier는 결제 확인이 완료되는 즉시 웹훅을 통해 고객의 구독을 자동으로 활성화합니다.

For more information on `incomplete` and `past_due` states, please refer to [our additional documentation](#incomplete-and-past-due-status).

`incomplete` 와 `past_due` 상태에 대한 자세한 내용은 [추가 문서](#incomplete-and-past-due-status)를 참조하십시오.

<a name="off-session-payment-notifications"></a>
### Off-Session Payment Notifications
### 세션 외 결제 알림

Since SCA regulations require customers to occasionally verify their payment details even while their subscription is active, Cashier can send a payment notification to the customer when off-session payment confirmation is required. For example, this may occur when a subscription is renewing. Cashier's payment notification can be enabled by setting the `CASHIER_PAYMENT_NOTIFICATION` environment variable to a notification class. By default, this notification is disabled. Of course, Cashier includes a notification class you may use for this purpose, but you are free to provide your own notification class if desired:

SCA 규정에 따라 고객은 구독이 활성화되어있는 동안에도 결제 세부 정보를 확인해야하므로 세션 외 지불 확인이 필요한 경우 Cashier가 고객에게 결제 알림을 보낼 수 있습니다. 예를 들어 구독이 갱신 될 때 발생할 수 있습니다. `CASHIER_PAYMENT_NOTIFICATION` 환경 변수에 알림 클래스로 설정하여 Cashier의 지불 알림으로 사용할 수 있습니다. 기본적으로 이 알림은 비활성화되어 있습니다. 물론 Cashier에는 이 목적으로 사용할 수있는 알림 클래스가 포함되어 있지만 원하는 경우 자신의 알림 클래스를 자유롭게 설정 할 수 있습니다.

    CASHIER_PAYMENT_NOTIFICATION=Laravel\Cashier\Notifications\ConfirmPayment

To ensure that off-session payment confirmation notifications are delivered, verify that [Stripe webhooks are configured](#handling-stripe-webhooks) for your application and the `invoice.payment_action_required` webhook is enabled in your Stripe dashboard. In addition, your `Billable` model should also use Laravel's `Illuminate\Notifications\Notifiable` trait.

세션 외 결제 확인 알림이 전달되도록하려면 애플리케이션에 대한 [Stripe webhooks 구성](#handling-stripe-webhooks) 및 Stripe 대시 보드에서 `invoice.payment_action_required` webhook이 활성화되어 있는지 확인하십시오. 또한 `Billable` 모델은 Laravel의 `Illuminate\Notifications\Notifiable` trait도 사용해야합니다.

> {note} Notifications will be sent even when customers are manually making a payment that requires additional confirmation. Unfortunately, there is no way for Stripe to know that the payment was done manually or "off-session". But, a customer will simply see a "Payment Successful" message if they visit the payment page after already confirming their payment. The customer will not be allowed to accidentally confirm the same payment twice and incur an accidental second charge.

> {note} 고객이 추가 확인이 필요한 수동 결제를 하는 경우에도 알림이 전송됩니다. 불행히도 Stripe이 지불이 수동 또는 "세션 외"로 완료되었음을 알 수있는 방법은 없습니다. 그러나 고객은 이미 결제를 확인한 후 결제 페이지를 방문하면 "결제 성공"메시지를 보게됩니다. 고객은 실수로 동일한 결제를 두 번 확인하고 실수로 두 번째 청구를 할 수 없습니다.

<a name="stripe-sdk"></a>
## Stripe SDK
## Stripe SDK

Many of Cashier's objects are wrappers around Stripe SDK objects. If you would like to interact with the Stripe objects directly, you may conveniently retrieve them using the `asStripe` method:

Cashier의 많은 객체는 Stripe SDK 객체를 감싸는 래퍼입니다. Stripe 객체와 직접 상호 작용하려면 `asStripe` 메소드를 사용하여 편리하게 조회 할 수 있습니다.

    $stripeSubscription = $subscription->asStripeSubscription();

    $stripeSubscription->application_fee_percent = 5;

    $stripeSubscription->save();

You may also use the `updateStripeSubscription` method to update the Stripe subscription directly:

`updateStripeSubscription` 메서드를 사용하여 Stripe 구독을 직접 업데이트 할 수도 있습니다.

    $subscription->updateStripeSubscription(['application_fee_percent' => 5]);

<a name="testing"></a>
## Testing
## 테스팅

When testing an application that uses Cashier, you may mock the actual HTTP requests to the Stripe API; however, this requires you to partially re-implement Cashier's own behavior. Therefore, we recommend allowing your tests to hit the actual Stripe API. While this is slower, it provides more confidence that your application is working as expected and any slow tests may be placed within their own PHPUnit testing group.

캐셔를 사용하는 애플리케이션을 테스트 할 때 실제 HTTP 요청을 Stripe API로 모킹 할 수 있습니다. 그러나 이를 위해서는 캐셔 자신의 행동을 부분적으로 다시 구현해야합니다. 따라서 테스트가 실제 Stripe API에 도달하도록 허용하는 것이 좋습니다. 속도는 느리지만 애플리케이션이 예상대로 작동하고 있으며 느린 테스트는 자체 PHPUnit 테스트 그룹 내에 배치 될 수 있습니다.

When testing, remember that that Cashier itself already has a great test suite, so you should only focus on testing the subscription and payment flow of your own application and not every underlying Cashier behavior.

테스트시 캐셔 자체에는 이미 훌륭한 테스트 suite가 있으므로 모든 기본 캐셔 동작이 아니라 자체 애플리케이션의 서브스크립션 및 지불 플로우 테스트에만 중점을 두어야합니다.

To get started, add the **testing** version of your Stripe secret to your `phpunit.xml` file:

시작하려면, **testing** 버전의 Stripe secret을 `phpunit.xml` 파일에 추가하십시오.

    <env name="STRIPE_SECRET" value="sk_test_<your-key>"/>

Now, whenever you interact with Cashier while testing, it will send actual API requests to your Stripe testing environment. For convenience, you should pre-fill your Stripe testing account with subscriptions / plans that you may then use during testing.

이제 테스트하는 동안 캐셔와 상호 작용할 때마다 실제 API 요청이 Stripe 테스트 환경으로 전송됩니다. 편의상 스트라이프 테스트 계정에 구독/플랜을 미리 채워서 테스트 중에 사용할 수 있도록해야합니다.

> {tip} In order to test a variety of billing scenarios, such as credit card denials and failures, you may use the vast range of [testing card numbers and tokens](https://stripe.com/docs/testing) provided by Stripe.

> {tip} 신용 카드 거부 및 실패와 같은 다양한 결제 시나리오를 테스트하기 위해 Stripe가 제공하는 광범위한 [테스트 카드 번호 및 토큰](https://stripe.com/docs/testing)을 사용할 수 있습니다.
