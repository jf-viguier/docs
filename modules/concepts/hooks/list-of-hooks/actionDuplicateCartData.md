---
Title: actionDuplicateCartData
hidden: true
hookTitle: 'Cart duplication'
files:
    -
        url: 'https://github.com/PrestaShop/PrestaShop/blob/9.1.x/classes/Cart.php'
        file: classes/Cart.php
locations:
    - 'front office'
type: action
hookAliases: 
array_return: false
check_exceptions: false
chain: false
origin: core
description: 'This hook is triggered after all the cart related data has been duplicated'

---

{{% hookDescriptor %}}

## Call of the Hook in the origin file

```php
Hook::exec('actionDuplicateCartData', ['oldCardId' => $this->id, 'newCartId' => $cart->id]);
```
