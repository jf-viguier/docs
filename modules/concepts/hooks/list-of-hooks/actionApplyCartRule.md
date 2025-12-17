---
Title: actionApplyCartRule
hidden: true
hookTitle: ''
files:
    -
        url: 'https://github.com/PrestaShop/PrestaShop/blob/9.1.x/src/Core/Cart/CartRuleCalculator.php'
        file: src/Core/Cart/CartRuleCalculator.php
locations:
    - 'front office'
type: action
hookAliases: 
array_return: false
check_exceptions: false
chain: false
origin: core
description: ''

---

{{% hookDescriptor %}}

## Call of the Hook in the origin file

```php
Hook::exec(
            'actionApplyCartRule',
            [
                'cart_rule_calculator' => $this,
                'cart_rule_data' => $cartRuleData,
                'cart_rule' => $cartRule,
                'cart' => $cart,
                'with_free_shipping' => $withFreeShipping,
                'is_applied_by_modules' => &$isAppliedByModules,
            ]
        );
```
