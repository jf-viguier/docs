---
Title: actionModifyHtmlPurifierConfig
hidden: true
hookTitle: 'Called when configuring HTMLPurifier'
files:
    -
        url: 'https://github.com/PrestaShop/PrestaShop/blob/9.1.x/classes/Tools.php'
        file: classes/Tools.php
locations:
    - 'front office'
type: action
hookAliases: 
array_return: false
check_exceptions: false
chain: false
origin: core
description: 'Allows modules to modify the HTMLPurifier definition by adding custom allowed HTML elements or attributes during Tools::purifyHTML().'

---

{{% hookDescriptor %}}

## Call of the Hook in the origin file

```php
Hook::exec('actionModifyHtmlPurifierConfig', [
                        'config' => &$config,
                    ]);
```
