---
Title: actionOverrideProductQuantity
hidden: true
hookTitle: 'Override product quantity calculation'
files:
    -
        url: 'https://github.com/PrestaShop/PrestaShop/blob/9.1.x/classes/Product.php'
        file: classes/Product.php
locations:
    - 'front office'
type: action
hookAliases: 
array_return: false
check_exceptions: true
chain: true
origin: core
description: 'Allows modules to override the final product quantity returned by Product::getQuantity(), including cart-aware calculations.'

---

{{% hookDescriptor %}}

## Call of the Hook in the origin file

```php
Hook::exec(
            'actionOverrideProductQuantity',
            [
                'id_product' => $idProduct,
                'id_product_attribute' => $idProductAttribute,
                'cart' => $cart,
                'cacheIsPack' => $cacheIsPack,
                'idCustomization' => $idCustomization,
                'isCartProvided' => $cart !== null, // true if $cart is passed to reduce the quantity by the amount in cart
            ],
            null,
            false,
            true,
            false,
            null,
            true
        );
```
