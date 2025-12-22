---
Title: actionProductGetAttributesGroupsAfter
hidden: true
hookTitle: 'Triggers after getting product attributes groups'
files:
    -
        url: 'https://github.com/PrestaShop/PrestaShop/blob/9.1.x/classes/Product.php'
        file: classes/Product.php
locations:
    - 'front office'
type: action
hookAliases: 
array_return: false
check_exceptions: false
chain: false
origin: core
description: 'Allows to modify product attributes groups after they are retrieved from the database.'

---

{{% hookDescriptor %}}

## Call of the Hook in the origin file

```php
Hook::exec('actionProductGetAttributesGroupsAfter', [
            'product' => $this,
            'id_lang' => $id_lang,
            'id_product_attribute' => $id_product_attribute,
            'attributes_groups' => &$result,
        ]);
```
