---
Title: actionFacetedSearchSetSupportedControllers
hidden: true
hookTitle: ''
files:
    -
        url: 'https://github.com/PrestaShop/PrestaShop/blob/9.1.x/modules/ps_facetedsearch/ps_facetedsearch.php'
        file: modules/ps_facetedsearch/ps_facetedsearch.php
locations:
    - 'front office'
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
Hook::exec(
            'actionFacetedSearchSetSupportedControllers',
            [
                'supportedControllers' => &$supportedControllers,
            ]
        );
```
