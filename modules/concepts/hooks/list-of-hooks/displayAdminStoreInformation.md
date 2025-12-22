---
Title: displayAdminStoreInformation
hidden: true
hookTitle: 'Display extra store information'
files:
    -
        url: 'https://github.com/PrestaShop/PrestaShop/blob/9.1.x/src/PrestaShopBundle/Resources/views/Admin/Configure/AdvancedParameters/system_information.html.twig'
        file: src/PrestaShopBundle/Resources/views/Admin/Configure/AdvancedParameters/system_information.html.twig
locations:
    - 'back office'
type: display
hookAliases: 
array_return: false
check_exceptions: false
chain: false
origin: core
description: 'This hook displays content in the Information page to add store information'

---

{{% hookDescriptor %}}

## Call of the Hook in the origin file

```php
{{ renderhook('displayAdminStoreInformation') }};
```
