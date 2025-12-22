---
Title: actionOverrideQuantityAvailableByProduct
hidden: true
hookTitle: 'Override available quantity by product'
files:
    -
        url: 'https://github.com/PrestaShop/PrestaShop/blob/9.1.x/classes/stock/StockAvailable.php'
        file: classes/stock/StockAvailable.php
locations:
    - 'front office'
type: action
hookAliases: 
array_return: false
check_exceptions: true
chain: true
origin: core
description: 'Allows modules to override the available quantity returned by StockAvailable::getQuantityAvailableByProduct().'

---

{{% hookDescriptor %}}

## Call of the Hook in the origin file

```php
Hook::exec(
            'actionOverrideQuantityAvailableByProduct',
            [
                'id_product' => $id_product,
                'id_product_attribute' => $id_product_attribute,
                'id_shop' => $id_shop,
            ],
            null,
            false,
            true,
            false,
            null,
            true
        );
```
