# Add new payment provider

[TOC]

This guide provides step-by-step instructions on creating a basic payment module skeleton for Mage-OS. It is intended for developers familiar with Mage-OS module development, PHP, and payment processing flow.

## Step 1: Create payment method module

`registration.php`

This file registers the module in Magento.

```php
<?php
use Magento\Framework\Component\ComponentRegistrar;

ComponentRegistrar::register(
    ComponentRegistrar::MODULE,
    'Vendor_PaymentModule',
    __DIR__
);

```

`etc/module.xml`

```xml
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:Module/etc/module.xsd">
    <module name="Vendor_Module">
        <sequence>
            <module name="Magento_Sales"/>
            <module name="Magento_Payment"/>
            <module name="Magento_Checkout"/>
        </sequence>
    </module>
</config>
```

`composer.json`

```json
{
    "name": "vendor/module-name",
    "version": "1.0.0",
    "description": "N/A",
    "type": "magento2-module",
    "require": {
        "magento/framework": "*",
        "magento/module-payment": "100.1.*",
        "magento/module-checkout": "100.1.*",
        "magento/module-sales": "100.1.*"
    },
    "license": [
        "Proprietary"
    ],
    "autoload": {
        "files": [
            "registration.php"
        ],
        "psr-4": {
            "Vendor\\Module\\": ""
        }
    }
}
```

## Step 2: Payment method configuration

`etc/config.xml`

In this file, you configure the payment method and its settings, such as title, active status, and method-specific options.

```xml
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:module:Magento_Store:etc/config.xsd">
    <default>
        <payment>
            <payment_method_name>
                <title>Payment method title</title>
                <model>BraintreeFacade</model>
                <active>0</active>
                <is_gateway>1</is_gateway>
                <can_use_checkout>1</can_use_checkout>
                <can_authorize>1</can_authorize>
                <can_capture>1</can_capture>
            </braintree>
        </payment>
    </default>
</config>
```

- title (**required**): Name displayed on the checkout page.
- model (**required**): Points to the model class handling payment logic.
- active (**required**): Defines if the payment method is enabled.
- is_gateway (**optional**): Is an integration with payment gateway provider.
- can_use_checkout (**optional**): Payment method is available in checkout
- can_authorize (**optional**): Payment method supports authorization operation
- can_capture (**optional**): Payment method supports capture operation
