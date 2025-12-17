---
Title: overrideImageLink
hidden: true
hookTitle: 'Override product image link'
files:
    -
        url: 'https://github.com/PrestaShop/PrestaShop/blob/9.1.x/classes/Link.php'
        file: classes/Link.php
locations:
    - 'front office'
type: action
hookAliases: 
array_return: true
check_exceptions: false
chain: false
origin: core
description: 'Allows to fully override the image URL returned by the getImageLink() method.'

---

{{% hookDescriptor %}}

## Call of the Hook in the origin file

```php
Hook::exec(
            'overrideImageLink',
            [
                'name' => $name,
                'ids' => $idImage,
                'type' => $type,
                'extension' => $extension,
            ],
            null,
            true
        );
```
