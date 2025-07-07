---
Title: displayAdminProductsExtra
hidden: true
hookTitle: 'Display new elements in back office product page, Extra tab'
files:
    -
        url: 'https://github.com/PrestaShop/PrestaShop/blob/9.0.x/src/PrestaShopBundle/Form/Admin/Sell/Product/ExtraModulesType.php'
        file: src/PrestaShopBundle/Form/Admin/Sell/Product/ExtraModulesType.php
locations:
    - 'back office'
type: display
hookAliases: 
hasExample: true
array_return: false
check_exceptions: false
chain: false
origin: core
description: 'This form type is used to display modules in an extra tab that regroup the modules implementing the displayAdminProductsExtra hook. This is not the recommended way to integrate in the product page anymore but we keep it for backward compatibility.'

---

{{% hookDescriptor %}}

## Example implementation

This hook has been implemented as an example in our [modules examples repository - demoproductform](https://github.com/PrestaShop/example-modules/tree/master/demoproductform).
