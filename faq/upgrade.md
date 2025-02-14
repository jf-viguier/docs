---
title: Upgrade FAQ
showOnHomepage: true
---

# Upgrade FAQ

- [After upgrading, I lost access to some Back-Office pages](#restore-access-to-back-office-page)
- [Upgrade fails with Error message JqXHR](#upgrade-fails-with-error-message-jqxhr)


## Restore access to Back Office Page

**Q:** After upgrading my PrestaShop to a new version, I lost access to some Back-Office pages. How can I fix it?

**A:** It is likely that some SQL configuration is not correct.

First, identify what is the `slug` of the Back Office pages. You can find them into the SQL table `ps_authorization_role`. This will tell you the SQL identifier for these pages.

Second, identify the _Role_ of the User you use to browse the Back Office.

Third, check whether the table `ps_access` grants access to the Back Office pages, using the identifier of the role and the identifiers of the Back Office pages. There must be a record for the role and the page. If there is no such record, create it to grant access.

## Upgrade fails with Error message JqXHR

If during upgrade process, it fails with the error message:

```
[Ajax/Server operation [...]] textStatus: "Error" errorThrown: "" JqXHR: "",
```

This error message indicates something went wrong on server side.

In order to find out what exactly went wrong, you need to check the upgrade process logs that are located into the folder `{Prestashop_Folder}/{Admin_folder}/autoupgrade/tmp/`

## The version of PrestaShop does not match the one stored in database

**Q:** Upgrade cannot start because the error "The version of PrestaShop stored in database does not match the running code. Your database structure may not be up-to-date and/or the value of PS_VERSION_DB needs to be updated in the configuration table." is displayed.

**A:** In PrestaShop, `PS_VERSION_DB` is a constant that holds the current version number of your PrestaShop database schema. The main purpose of `PS_VERSION_DB` is to keep track of the database schema's version history. When you upgrade a shop, the database schema is modified to match the structure of the new version (e.g. add or remove tables, columns, or relationships). 

Before upgrading PrestaShop, the upgrade module relies on this constant to ensure that the current version matches the database schema. If the values don't match, it would be a sign of potential issues with the database structure or data which could lead to unforeseen consequences during the upgrade process.
