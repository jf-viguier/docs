---
Title: actionOrderHasBeenDelivered
hidden: true
hookTitle: 'Called when checking if an order has been delivered'
files:
    -
        url: 'https://github.com/PrestaShop/PrestaShop/blob/9.1.x/classes/order/Order.php'
        file: classes/order/Order.php
locations:
    - 'front office'
type: action
hookAliases: 
array_return: false
check_exceptions: true
chain: true
origin: core
description: 'Allows modules to override or react to the hasBeenDelivered() method of the Order class.'

---

{{% hookDescriptor %}}

## Call of the Hook in the origin file

```php
Hook::exec(
            'actionOrderHasBeenDelivered',
            ['order' => $this],
            null,
            false,
            true,
            false,
            null,
            true
        );
```
