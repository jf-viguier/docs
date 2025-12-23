---
Title: actionObjectDuplicateAfter
hidden: true
hookTitle: 'After duplicating an object'
files:
    -
        url: 'https://github.com/PrestaShop/PrestaShop/blob/9.1.x/classes/ObjectModel.php'
        file: classes/ObjectModel.php
locations:
    - 'back office'
    - 'front office'
type: action
hookAliases: 
array_return: false
check_exceptions: false
chain: false
origin: core
description: 'This hook is called after duplicating an object by the core.'

---

{{% hookDescriptor %}}

## Call of the Hook in the origin file

```php
Hook::exec('actionObjectDuplicateAfter', ['oldObject' => $this, 'newObject' => $object_duplicated]);
```
