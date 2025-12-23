---
Title: actionAdminDuplicateDiscountBefore
hidden: true
hookTitle: ''
files:
    -
        url: 'https://github.com/PrestaShop/PrestaShop/blob/9.1.x/src/Adapter/Discount/Update/DiscountDuplicator.php'
        file: src/Adapter/Discount/Update/DiscountDuplicator.php
locations:
    - 'back office'
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
$this->hookDispatcher->dispatchWithParameters(
            'actionAdminDuplicateDiscountBefore',
            ['id_discount' => $oldDiscountId]
        );
```
