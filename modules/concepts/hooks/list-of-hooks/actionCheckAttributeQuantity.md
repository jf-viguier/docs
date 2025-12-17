---
Title: actionCheckAttributeQuantity
hidden: true
hookTitle: 'Check product attribute quantity availability'
files:
    -
        url: 'https://github.com/PrestaShop/PrestaShop/blob/9.1.x/classes/ProductAttribute.php'
        file: classes/ProductAttribute.php
locations:
    - 'front office'
type: action
hookAliases: 
array_return: false
check_exceptions: true
chain: true
origin: core
description: 'Allows modules to validate or override the stock availability check for a specific product combination.'

---

{{% hookDescriptor %}}

## Call of the Hook in the origin file

```php
Hook::exec(
            'actionCheckAttributeQuantity',
            [
                'id_product_attribute' => $idProductAttribute,
                'qty' => $qty,
                'shop' => $shop,
            ],
            null,
            false,
            true,
            false,
            null,
            true
        );
```
