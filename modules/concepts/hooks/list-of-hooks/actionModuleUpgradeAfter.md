---
Title: actionModuleUpgradeAfter
hidden: true
hookTitle: ''
files:
    -
        url: 'https://github.com/PrestaShop/PrestaShop/blob/9.1.x/classes/module/Module.php'
        file: classes/module/Module.php
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
Hook::exec('actionModuleUpgradeAfter', ['module_name' => $name, 'old_version' => static::$modules_cache[$name]['upgrade']['upgraded_from'], 'new_version' => $version]);
```
