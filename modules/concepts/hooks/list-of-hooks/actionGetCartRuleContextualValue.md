---
Title: actionGetCartRuleContextualValue
hidden: true
hookTitle: ''
files:
    -
        url: 'https://github.com/PrestaShop/PrestaShop/blob/9.1.x/classes/CartRule.php'
        file: classes/CartRule.php
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
            'actionGetCartRuleContextualValue',
            [
                'cart_rule' => $this,
                'use_tax' => $use_tax,
                'context' => $context,
                'filter' => $filter,
                'package' => $package,
                'use_cache' => $use_cache,
                'contextualValueFromModules' => &$contextualValueFromModules,
            ]
        );
```
