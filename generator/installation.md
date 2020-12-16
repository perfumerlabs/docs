---
layout: default
title: Installation
nav_exclude: true
---

Add to composer.json

```json
{
  "require-dev": {
    "perfumerlabs/perfumer-framework-pack": "*"
  }
}
```

Create generate.php file with content

```php
<?php

require '/path/to/vendor/autoload.php';

$generator = new \Perfumerlabs\Perfumer\Generator(__DIR__, [
    'generated_annotation_path' => 'generated/annotation',
    'generated_src_path' => 'generated/src',
    'generated_tests_path' => 'generated/tests',
    'src_path' => 'src',
    'tests_path' => 'tests',
    'contract_prefix' => 'Project\\Contract',
    'context_prefix' => 'Project',
    'class_prefix' => 'Project'
]);

$generator->addContextDirectory('path/to/context/directory');
$generator->addContractDirectory('path/to/contract/directory');
$generator->generateAll();
```