---
title: Create a new endpoint using CQRS
menuTitle: New CQRS endpoint
showOnHomepage: true
weight: 100
---

# Create a new API endpoint using CQRS

This guide explains how to create a pull request for a new endpoint. This documentation is based on the [pull request](https://github.com/PrestaShop/ps_apiresources/pull/55) for the attribute group endpoints in PrestaShop's `ps_apiresources` module.

## 📋 Prerequisites

- Basic knowledge of PHP and OOP
- Understanding of CQRS (Command Query Responsibility Segregation) pattern
- PrestaShop development environment configured
- Git installed and configured
- PHPUnit knowledge for testing

## 🎯 Objective

Create REST API endpoints to manage attribute groups (AttributeGroup) with complete CRUD operations and comprehensive PHPUnit test coverage.

## 🏗️ Project Structure

```
ps_apiresources/
├── src/
│   └── ApiPlatform/
│       └── Resources/
│           └── AttributeGroup/
│               ├── AttributeGroup.php          # Resource for single operations
│               ├── AttributeGroupList.php      # Resource for listing
│               └── AttributeGroupForEditing.php # Resource for editing
├── tests/
│   └── Integration/
│       └── ApiPlatform/
│           └── Resources/
│               └── AttributeGroupEndpointTest.php
```

## 📝 Implementation Steps

### 1. Create the Main Resource (AttributeGroup.php)

Create the file `src/ApiPlatform/Resources/AttributeGroup/AttributeGroup.php`:

```php
<?php
declare(strict_types=1);

namespace PrestaShop\Module\APIResources\ApiPlatform\Resources\AttributeGroup;

use ApiPlatform\Metadata\ApiProperty;
use ApiPlatform\Metadata\ApiResource;
use PrestaShop\PrestaShop\Core\Domain\AttributeGroup\Command\AddAttributeGroupCommand;
use PrestaShop\PrestaShop\Core\Domain\AttributeGroup\Command\DeleteAttributeGroupCommand;
use PrestaShop\PrestaShop\Core\Domain\AttributeGroup\Command\EditAttributeGroupCommand;
use PrestaShop\PrestaShop\Core\Domain\AttributeGroup\Exception\AttributeGroupNotFoundException;
use PrestaShop\PrestaShop\Core\Domain\AttributeGroup\Query\GetAttributeGroupForEditing;
use PrestaShopBundle\ApiPlatform\Metadata\CQRSCreate;
use PrestaShopBundle\ApiPlatform\Metadata\CQRSDelete;
use PrestaShopBundle\ApiPlatform\Metadata\CQRSGet;
use PrestaShopBundle\ApiPlatform\Metadata\CQRSPartialUpdate;
use PrestaShopBundle\ApiPlatform\Metadata\CQRSUpdate;

#[ApiResource(
    operations: [
        // GET /attribute-group/{attributeGroupId}
        new CQRSGet(
            uriTemplate: '/attribute-group/{attributeGroupId}',
            requirements: ['attributeGroupId' => '\d+'],
            CQRSQuery: GetAttributeGroupForEditing::class,
            scopes: ['attribute_group_read']
        ),
        // POST /attribute-group
        new CQRSCreate(
            uriTemplate: '/attribute-group',
            CQRSCommand: AddAttributeGroupCommand::class,
            CQRSQuery: GetAttributeGroupForEditing::class,
            scopes: ['attribute_group_write']
        ),
        // PATCH /attribute-group/{attributeGroupId}
        new CQRSPartialUpdate(
            uriTemplate: '/attribute-group/{attributeGroupId}',
            requirements: ['attributeGroupId' => '\d+'],
            read: false,
            CQRSCommand: EditAttributeGroupCommand::class,
            CQRSQuery: GetAttributeGroupForEditing::class,
            scopes: ['attribute_group_write']
        ),
        // PUT /attribute-group/{attributeGroupId}
        new CQRSUpdate(
            uriTemplate: '/attribute-group/{attributeGroupId}',
            requirements: ['attributeGroupId' => '\d+'],
            read: false,
            CQRSCommand: EditAttributeGroupCommand::class,
            CQRSQuery: GetAttributeGroupForEditing::class,
            scopes: ['attribute_group_write']
        ),
        // DELETE /attribute-group/{attributeGroupId}
        new CQRSDelete(
            uriTemplate: '/attribute-group/{attributeGroupId}',
            requirements: ['attributeGroupId' => '\d+'],
            output: false,
            CQRSCommand: DeleteAttributeGroupCommand::class,
            scopes: ['attribute_group_write']
        ),
    ],
    exceptionToStatus: [AttributeGroupNotFoundException::class => 404],
)]
class AttributeGroup
{
    #[ApiProperty(identifier: true)]
    public int $attributeGroupId;

    public array $localizedNames;

    public array $localizedPublicNames;

    public bool $isColorGroup;

    public string $groupType;

    public int $position;
}
```

### 2. Create the List Resource (AttributeGroupList.php)

Create the file `src/ApiPlatform/Resources/AttributeGroup/AttributeGroupList.php`:

```php
<?php
declare(strict_types=1);

namespace PrestaShop\Module\APIResources\ApiPlatform\Resources\AttributeGroup;

use ApiPlatform\Metadata\ApiProperty;
use ApiPlatform\Metadata\ApiResource;
use PrestaShop\PrestaShop\Core\Search\Filters\AttributeGroupFilters;
use PrestaShopBundle\ApiPlatform\Metadata\PaginatedList;

#[ApiResource(
    operations: [
        new PaginatedList(
            uriTemplate: '/attribute-groups',
            scopes: ['attribute_group_read'],
            ApiResourceMapping: [
                '[id_attribute_group]' => '[attributeGroupId]',
                '[name]' => '[localizedNames]',
                '[public_name]' => '[localizedPublicNames]',
                '[is_color_group]' => '[isColorGroup]',
                '[group_type]' => '[groupType]',
                '[position]' => '[position]',
            ],
            gridDataFactory: 'prestashop.core.grid.data_factory.attribute_group',
            filtersClass: AttributeGroupFilters::class,
            filtersMapping: [
                '[attributeGroupId]' => '[id_attribute_group]',
                '[name]' => '[name]',
                '[isColorGroup]' => '[is_color_group]',
                '[groupType]' => '[group_type]',
            ],
        ),
    ],
    normalizationContext: ['skip_null_values' => false],
)]
class AttributeGroupList
{
    #[ApiProperty(identifier: true)]
    public int $attributeGroupId;

    public array $localizedNames;

    public array $localizedPublicNames;

    public bool $isColorGroup;

    public string $groupType;

    public int $position;
}
```

### 3. Define Required Scopes

Add scopes in the appropriate configuration file:

```yaml
# In scopes configuration file
attribute_group_read:
  description: 'Read attribute groups'
  
attribute_group_write:
  description: 'Write attribute groups'
```

## 🔗 API Endpoints

### GET - Retrieve Single Attribute Group

#### Endpoint Configuration
```php
new CQRSGet(
    uriTemplate: '/attribute-group/{attributeGroupId}',
    requirements: ['attributeGroupId' => '\d+'],
    CQRSQuery: GetAttributeGroupForEditing::class,
    scopes: ['attribute_group_read']
),
```

#### Usage
- **URL**: `GET /attribute-group/{attributeGroupId}`
- **Purpose**: Retrieve a specific attribute group by its ID
- **Authentication**: Requires `attribute_group_read` scope
- **Parameters**:
    - `attributeGroupId` (integer, required): The ID of the attribute group to retrieve

#### Request Example
```http
GET /api/attribute-group/1
Authorization: Bearer your_access_token
Content-Type: application/json
```

#### Response Example
```json
{
    "attributeGroupId": 1,
    "localizedNames": {
        "en": "Color",
        "fr": "Couleur"
    },
    "localizedPublicNames": {
        "en": "Color",
        "fr": "Couleur"
    },
    "isColorGroup": true,
    "groupType": "color",
    "position": 1
}
```

#### Error Responses
- **404 Not Found**: When the attribute group doesn't exist
- **401 Unauthorized**: When authentication is missing or invalid
- **403 Forbidden**: When the user lacks the required scope

### GET - List Attribute Groups

#### Endpoint Configuration
```php
new PaginatedList(
    uriTemplate: '/attribute-groups',
    scopes: ['attribute_group_read'],
    ApiResourceMapping: [
        '[id_attribute_group]' => '[attributeGroupId]',
        '[name]' => '[localizedNames]',
        '[public_name]' => '[localizedPublicNames]',
        '[is_color_group]' => '[isColorGroup]',
        '[group_type]' => '[groupType]',
        '[position]' => '[position]',
    ],
    gridDataFactory: 'prestashop.core.grid.data_factory.attribute_group',
    filtersClass: AttributeGroupFilters::class,
    filtersMapping: [
        '[attributeGroupId]' => '[id_attribute_group]',
        '[name]' => '[name]',
        '[isColorGroup]' => '[is_color_group]',
        '[groupType]' => '[group_type]',
    ],
),
```

#### Usage
- **URL**: `GET /attribute-groups`
- **Purpose**: Retrieve a paginated list of attribute groups
- **Authentication**: Requires `attribute_group_read` scope
- **Parameters**:
    - `page` (integer, optional): Page number for pagination
    - `itemsPerPage` (integer, optional): Number of items per page
    - `attributeGroupId` (integer, optional): Filter by attribute group ID
    - `name` (string, optional): Filter by name
    - `isColorGroup` (boolean, optional): Filter by color group status
    - `groupType` (string, optional): Filter by group type

#### Request Examples
```http
# Basic list
GET /api/attribute-groups
Authorization: Bearer your_access_token

# With pagination
GET /api/attribute-groups?page=1&itemsPerPage=10
Authorization: Bearer your_access_token

# With filters
GET /api/attribute-groups?isColorGroup=true&groupType=color
Authorization: Bearer your_access_token
```

#### Response Example
```json
{
    "hydra:member": [
        {
            "attributeGroupId": 1,
            "localizedNames": {
                "en": "Color",
                "fr": "Couleur"
            },
            "localizedPublicNames": {
                "en": "Color",
                "fr": "Couleur"
            },
            "isColorGroup": true,
            "groupType": "color",
            "position": 1
        },
        {
            "attributeGroupId": 2,
            "localizedNames": {
                "en": "Size",
                "fr": "Taille"
            },
            "localizedPublicNames": {
                "en": "Size",
                "fr": "Taille"
            },
            "isColorGroup": false,
            "groupType": "select",
            "position": 2
        }
    ],
    "hydra:totalItems": 2,
    "hydra:view": {
        "@id": "/api/attribute-groups?page=1",
        "@type": "hydra:PartialCollectionView",
        "hydra:first": "/api/attribute-groups?page=1",
        "hydra:last": "/api/attribute-groups?page=1"
    }
}
```

### POST - Create Attribute Group

#### Endpoint Configuration
```php
new CQRSCreate(
    uriTemplate: '/attribute-group',
    CQRSCommand: AddAttributeGroupCommand::class,
    CQRSQuery: GetAttributeGroupForEditing::class,
    scopes: ['attribute_group_write']
),
```

#### Usage
- **URL**: `POST /attribute-group`
- **Purpose**: Create a new attribute group
- **Authentication**: Requires `attribute_group_write` scope
- **Content-Type**: `application/json`

#### Request Example
```http
POST /api/attribute-group
Authorization: Bearer your_access_token
Content-Type: application/json

{
    "localizedNames": {
        "en": "Material",
        "fr": "Matériau"
    },
    "localizedPublicNames": {
        "en": "Material",
        "fr": "Matériau"
    },
    "isColorGroup": false,
    "groupType": "select",
    "position": 3
}
```

#### Response Example
```json
{
    "attributeGroupId": 3,
    "localizedNames": {
        "en": "Material",
        "fr": "Matériau"
    },
    "localizedPublicNames": {
        "en": "Material",
        "fr": "Matériau"
    },
    "isColorGroup": false,
    "groupType": "select",
    "position": 3
}
```

#### Validation Rules
- **localizedNames**: Required, must contain at least one language
- **localizedPublicNames**: Required, must contain at least one language
- **isColorGroup**: Required, must be boolean
- **groupType**: Required, must be valid group type
- **position**: Optional, must be positive integer

#### Error Responses
- **400 Bad Request**: When validation fails
- **401 Unauthorized**: When authentication is missing or invalid
- **403 Forbidden**: When the user lacks the required scope

### PUT - Full Update Attribute Group

#### Endpoint Configuration
```php
new CQRSUpdate(
    uriTemplate: '/attribute-group/{attributeGroupId}',
    requirements: ['attributeGroupId' => '\d+'],
    read: false,
    CQRSCommand: EditAttributeGroupCommand::class,
    CQRSQuery: GetAttributeGroupForEditing::class,
    scopes: ['attribute_group_write']
),
```

#### Usage
- **URL**: `PUT /attribute-group/{attributeGroupId}`
- **Purpose**: Completely replace an existing attribute group
- **Authentication**: Requires `attribute_group_write` scope
- **Content-Type**: `application/json`
- **Parameters**:
    - `attributeGroupId` (integer, required): The ID of the attribute group to update

#### Request Example
```http
PUT /api/attribute-group/1
Authorization: Bearer your_access_token
Content-Type: application/json

{
    "localizedNames": {
        "en": "Updated Color",
        "fr": "Couleur Mise à Jour",
        "es": "Color Actualizado"
    },
    "localizedPublicNames": {
        "en": "Updated Color",
        "fr": "Couleur Mise à Jour",
        "es": "Color Actualizado"
    },
    "isColorGroup": true,
    "groupType": "color",
    "position": 1
}
```

#### Response Example
```json
{
    "attributeGroupId": 1,
    "localizedNames": {
        "en": "Updated Color",
        "fr": "Couleur Mise à Jour",
        "es": "Color Actualizado"
    },
    "localizedPublicNames": {
        "en": "Updated Color",
        "fr": "Couleur Mise à Jour",
        "es": "Color Actualizado"
    },
    "isColorGroup": true,
    "groupType": "color",
    "position": 1
}
```

#### Important Notes
- PUT replaces the entire resource, all fields must be provided
- Missing fields will be set to their default values
- Use PATCH for partial updates instead

### PATCH - Partial Update Attribute Group

#### Endpoint Configuration
```php
new CQRSPartialUpdate(
    uriTemplate: '/attribute-group/{attributeGroupId}',
    requirements: ['attributeGroupId' => '\d+'],
    read: false,
    CQRSCommand: EditAttributeGroupCommand::class,
    CQRSQuery: GetAttributeGroupForEditing::class,
    scopes: ['attribute_group_write']
),
```

#### Usage
- **URL**: `PATCH /attribute-group/{attributeGroupId}`
- **Purpose**: Partially update an existing attribute group
- **Authentication**: Requires `attribute_group_write` scope
- **Content-Type**: `application/json`
- **Parameters**:
    - `attributeGroupId` (integer, required): The ID of the attribute group to update

#### Request Example
```http
PATCH /api/attribute-group/1
Authorization: Bearer your_access_token
Content-Type: application/json

{
    "localizedNames": {
        "en": "Partially Updated Color"
    },
    "isColorGroup": false
}
```

#### Response Example
```json
{
    "attributeGroupId": 1,
    "localizedNames": {
        "en": "Partially Updated Color",
        "fr": "Couleur"
    },
    "localizedPublicNames": {
        "en": "Color",
        "fr": "Couleur"
    },
    "isColorGroup": false,
    "groupType": "color",
    "position": 1
}
```

#### Important Notes
- PATCH only updates the provided fields
- Other fields remain unchanged
- More efficient than PUT for small updates
- Supports partial localized arrays (only updates provided languages)

#### Error Responses
- **404 Not Found**: When the attribute group doesn't exist
- **400 Bad Request**: When validation fails
- **401 Unauthorized**: When authentication is missing or invalid
- **403 Forbidden**: When the user lacks the required scope

### DELETE - Remove Attribute Group

#### Endpoint Configuration
```php
new CQRSDelete(
    uriTemplate: '/attribute-group/{attributeGroupId}',
    requirements: ['attributeGroupId' => '\d+'],
    output: false,
    CQRSCommand: DeleteAttributeGroupCommand::class,
    scopes: ['attribute_group_write']
),
```

#### Usage
- **URL**: `DELETE /attribute-group/{attributeGroupId}`
- **Purpose**: Delete an existing attribute group
- **Authentication**: Requires `attribute_group_write` scope
- **Parameters**:
    - `attributeGroupId` (integer, required): The ID of the attribute group to delete

#### Request Example
```http
DELETE /api/attribute-group/1
Authorization: Bearer your_access_token
```

#### Response
- **Status Code**: `204 No Content`
- **Body**: Empty (no content returned)

#### Verification Example
```http
# Verify deletion by trying to retrieve the deleted resource
GET /api/attribute-group/1
Authorization: Bearer your_access_token

# Should return 404 Not Found
```

#### Important Notes
- Deletion is permanent and cannot be undone
- May fail if the attribute group is referenced by other entities
- Always verify deletion in a test environment first
- Consider soft delete patterns for production systems

#### Error Responses
- **404 Not Found**: When the attribute group doesn't exist
- **409 Conflict**: When the attribute group cannot be deleted due to dependencies
- **401 Unauthorized**: When authentication is missing or invalid
- **403 Forbidden**: When the user lacks the required scope

## 🧪 PHPUnit Testing Strategy

### Integration Tests

Integration tests should test the actual API endpoints and their behavior with the PrestaShop system.

#### CRUD Integration Test

Create `tests/Integration/ApiPlatform/Resources/AttributeGroupEndpointTest.php`:

```php
<?php
declare(strict_types=1);

namespace PsApiResourcesTest\Integration\ApiPlatform;

use Symfony\Component\HttpFoundation\Response;
use Tests\Resources\Resetter\LanguageResetter;

class AttributeGroupEndpointTest extends ApiTestCase
{
    public static function setUpBeforeClass(): void
    {
        parent::setUpBeforeClass();
        // Add the fr-FR language to test multi lang values accurately
        LanguageResetter::resetLanguages();
        self::addLanguageByLocale('fr-FR');
        // Pre-create the API Client with the needed scopes, this way we reduce the number of created API Clients
        self::createApiClient(['attribute_group_write', 'attribute_group_read']);
    }

    public static function tearDownAfterClass(): void
    {
        parent::tearDownAfterClass();
        // Reset DB as it was before this test
        LanguageResetter::resetLanguages();
    }

    public function getProtectedEndpoints(): iterable
    {
        yield 'get endpoint' => [
            'GET',
            '/attributes/group/1',
        ];

        yield 'create endpoint' => [
            'POST',
            '/attributes/group',
        ];

        yield 'patch endpoint' => [
            'PATCH',
            '/attributes/group/1',
        ];

        yield 'list endpoint' => [
            'GET',
            '/attributes/groups',
        ];
    }

    public function testAddAttributeGroup(): int
    {
        $itemsCount = $this->countItems('/attributes/groups', ['attribute_group_read']);

        $postData = [
            'names' => [
                'en-US' => 'name en',
                'fr-FR' => 'name fr',
            ],
            'publicNames' => [
                'en-US' => 'name en',
                'fr-FR' => 'name fr',
            ],
            'type' => 'select',
            'shopIds' => [1],
        ];

        // Create an attribute group, the POST endpoint returns the created item as JSON
        $attributeGroup = $this->createItem('/attributes/group', $postData, ['attribute_group_write']);
        $this->assertArrayHasKey('attributeGroupId', $attributeGroup);
        $attributeGroupId = $attributeGroup['attributeGroupId'];

        // We assert the returned data matches what was posted (plus the ID)
        $this->assertEquals(
            ['attributeGroupId' => $attributeGroupId] + $postData,
            $attributeGroup
        );

        $newItemsCount = $this->countItems('/attributes/groups', ['attribute_group_read']);
        $this->assertEquals($itemsCount + 1, $newItemsCount);

        return $attributeGroupId;
    }

    /**
     * @depends testAddAttributeGroup
     *
     * @param int $attributeGroupId
     *
     * @return int
     */
    public function testGetAttributeGroup(int $attributeGroupId): int
    {
        $attributeGroup = $this->getItem('/attributes/group/' . $attributeGroupId, ['attribute_group_read']);
        $this->assertEquals([
            'attributeGroupId' => $attributeGroupId,
            'names' => [
                'en-US' => 'name en',
                'fr-FR' => 'name fr',
            ],
            'publicNames' => [
                'en-US' => 'name en',
                'fr-FR' => 'name fr',
            ],
            'type' => 'select',
            'shopIds' => [1],
        ], $attributeGroup);

        return $attributeGroupId;
    }

    /**
     * @depends testGetAttributeGroup
     *
     * @param int $attributeGroupId
     *
     * @return int
     */
    public function testPartialUpdateAttributeGroup(int $attributeGroupId): int
    {
        $patchData = [
            'names' => [
                'en-US' => 'updated name en',
                'fr-FR' => 'updated name fr',
            ],
            'publicNames' => [
                'en-US' => 'updated name en',
                'fr-FR' => 'update name fr',
            ],
            'type' => 'radio',
            'shopIds' => [1],
        ];

        $updatedAttributeGroup = $this->partialUpdateItem('/attributes/group/' . $attributeGroupId, $patchData, ['attribute_group_write']);
        $this->assertEquals(['attributeGroupId' => $attributeGroupId] + $patchData, $updatedAttributeGroup);

        // We check that when we GET the item it is updated as expected
        $attributeGroup = $this->getItem('/attributes/group/' . $attributeGroupId, ['attribute_group_read']);
        $this->assertEquals(['attributeGroupId' => $attributeGroupId] + $patchData, $attributeGroup);

        return $attributeGroupId;
    }

    /**
     * @depends testPartialUpdateAttributeGroup
     *
     * @param int $attributeGroupId
     *
     * @return int
     */
    public function testListAttributeGroups(int $attributeGroupId): int
    {
        $paginatedAttributeGroups = $this->listItems('/attributes/groups', ['attribute_group_read']);
        $this->assertGreaterThanOrEqual(1, $paginatedAttributeGroups['totalItems']);

        // First we search for the test item in the list
        $testAttributeGroup = null;
        foreach ($paginatedAttributeGroups['items'] as $attributeGroup) {
            if ($attributeGroup['attributeGroupId'] === $attributeGroupId) {
                $testAttributeGroup = $attributeGroup;
                break;
            }
        }
        $this->assertNotNull($testAttributeGroup);
        // Position should be at least 3 since there are three groups in the default fixtures data
        $this->assertGreaterThanOrEqual(3, $testAttributeGroup['position']);
        $position = $testAttributeGroup['position'];
        $this->assertEquals([
            'attributeGroupId' => $attributeGroupId,
            'name' => 'updated name en',
            'values' => 0,
            'position' => $position,
        ], $testAttributeGroup);

        return $attributeGroupId;
    }

    /**
     * @depends testListAttributeGroups
     *
     * @param int $attributeGroupId
     */
    public function testRemoveAttributeGroup(int $attributeGroupId): void
    {
        // Delete the item
        $return = $this->deleteItem('/attributes/group/' . $attributeGroupId, ['attribute_group_write']);
        // This endpoint return empty response and 204 HTTP code
        $this->assertNull($return);

        // Getting the item should result in a 404 now
        $this->getItem('/attributes/group/' . $attributeGroupId, ['attribute_group_read'], Response::HTTP_NOT_FOUND);
    }
}
```

### Test Configuration and Setup

Run tests with:

```bash
# Create the test database 
composer create-test-db

# Setup your tests in local
composer setup-local-tests

# Run the tests on the module
composer run-module-tests
```

## 🔍 Key Points to Remember

### Naming Conventions
- **CamelCase** for API Resource properties
- **snake_case** for database mapping
- **PascalCase** for class names

### Data Mapping
- `ApiResourceMapping`: transforms DB data to API format
- `filtersMapping`: transforms API filters to grid format
- `CQRSQueryMapping`: for CQRS queries
- `CQRSCommandMapping`: for CQRS commands

### Security Scopes
- `_read`: for read operations (GET)
- `_write`: for write operations (POST, PUT, PATCH, DELETE)

### URI Templates
- Use consistent and descriptive names
- Follow REST conventions:
    - `GET /resource/{id}`: retrieve single item
    - `GET /resources`: list items
    - `POST /resource`: create item
    - `PUT/PATCH /resource/{id}`: update item
    - `DELETE /resource/{id}`: delete item

### Testing Best Practices
- **Unit tests**: Test resource classes in isolation
- **Integration tests**: Test actual API endpoints
- **Data providers**: Use for testing multiple scenarios
- **Mocking**: Mock external dependencies in unit tests
- **Test doubles**: Use for CQRS commands and queries
- **Coverage**: Aim for high test coverage on critical paths

## 🚀 PR Creation Process

### 1. Environment Setup
```bash
# Clone repository
git clone https://github.com/PrestaShop/ps_apiresources.git
cd ps_apiresources

# Create new feature branch
git checkout -b feature/attribute-group-endpoints
```

### 2. Development
- Create resource files
- Add comprehensive tests (unit + integration)
- Update documentation if necessary

### 3. Local Testing
```bash
# Run all tests
composer test

# Check code style
composer cs-fix

# Run static analysis
composer phpstan

# Generate coverage report
composer test:coverage
```

## 📚 Useful Resources

- [PrestaShop API Resources Documentation](https://devdocs.prestashop-project.org/9/admin-api/resource_server/api-resources/)
- [API Platform Documentation](https://api-platform.com/docs/)
- [PHPUnit Documentation](https://phpunit.de/documentation.html)
- [CQRS Pattern](https://martinfowler.com/bliki/CQRS.html)
- [PrestaShop Contributing Guidelines](https://devdocs.prestashop-project.org/9/contribute/)
- [Symfony Testing Best Practices](https://symfony.com/doc/current/testing.html)

## 🎉 Conclusion

Following this guide will help you create a comprehensive PR for adding API endpoints to PrestaShop with proper test coverage. Remember to:

1. Follow PrestaShop coding standards
2. Write comprehensive tests (unit + integration)
3. Achieve good test coverage
4. Document your changes properly
5. Follow the team's review process

Good luck with your contribution! 🚀