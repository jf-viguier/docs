---
Title: actionCheckoutStepRenderTemplate
hidden: true
hookTitle: 'Modify the parameters of the checkout step template rendering'
files:
    -
        url: 'https://github.com/PrestaShop/PrestaShop/blob/9.1.x/classes/checkout/AbstractCheckoutStep.php'
        file: classes/checkout/AbstractCheckoutStep.php
locations:
    - 'front office'
type: action
hookAliases: 
array_return: false
check_exceptions: false
chain: false
origin: core
description: 'This hook is called when rendering every checkout step template'

---

{{% hookDescriptor %}}

## Call of the Hook in the origin file

```php
Hook::exec('actionCheckoutStepRenderTemplate', [
            'template' => &$template,
            'params' => &$params,
            'extraParams' => &$extraParams,
            'defaultParams' => &$defaultParams,
        ]);
```
