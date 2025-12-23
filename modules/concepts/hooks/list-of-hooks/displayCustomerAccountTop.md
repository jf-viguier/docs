---
Title: displayCustomerAccountTop
hidden: true
hookTitle: 'Customer account displayed in Front Office (Top part)'
files:
    -
        url: 'https://github.com/PrestaShop/PrestaShop/blob/9.1.x/themes/hummingbird/templates/customer/my-account.tpl'
        file: themes/hummingbird/templates/customer/my-account.tpl
locations:
    - 'front office'
type: display
hookAliases: 
array_return: false
check_exceptions: false
chain: false
origin: core
description: 'This hook displays new elements on the customer account page on Top'

---

{{% hookDescriptor %}}

## Call of the Hook in the origin file

```php
{hook h='displayCustomerAccountTop'};
```
