---
Title: actionPaymentModuleProductVarTplAfter
hidden: true
hookTitle: 'Triggers after product data is prepared for e-mail template'
files:
    -
        url: 'https://github.com/PrestaShop/PrestaShop/blob/9.1.x/classes/PaymentModule.php'
        file: classes/PaymentModule.php
locations:
    - 'front office'
type: action
hookAliases: 
array_return: false
check_exceptions: false
chain: false
origin: core
description: 'Allows to modify product data in e-mail template.'

---

{{% hookDescriptor %}}

## Call of the Hook in the origin file

```php
Hook::exec('actionPaymentModuleProductVarTplAfter', [
                    'product_var_tpl' => &$product_var_tpl,
                    'product' => $product,
                    'order' => $order,
                    'context' => $this->context,
                ]);
```
