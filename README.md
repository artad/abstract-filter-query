# Abstract filter query

The library allows you to abstract from various query builders and use a common interface for everything.
To do this, you just need to write your own hotkey, which will be inherited from *AFQ\Converter\AbstractConverter*.

Here is example of using it

```php
<?php

use AFQ\Block\AndFilterBlock;
use AFQ\Comparison\Equal;
use AFQ\Comparison\In;
use AFQ\Converter\FilterBlockToSqlWhereConverter;
use AFQ\Converter\FilterBlockToYoutrackConverter;
use AFQ\FilterQuery;
use AFQ\Sorting\Sorting;

require_once '../vendor/autoload.php';

$queryFilter = new FilterQuery();
$queryFilter->setFilterBlock(new AndFilterBlock([
    new Equal('someKey', 'someValue'),
    new In('someKey2', ['value1', 'value2', 'value3'])
]));
$queryFilter->setSorting((new Sorting([
    ['someKey', Sorting::DESC],
    ['someKey2', Sorting::ASC]
])));

$sqlConverter = new FilterBlockToSqlWhereConverter();
$ytConverter = new FilterBlockToYoutrackConverter();

echo "Sql will be:\n";
echo $sqlConverter->convertFilterQuery($queryFilter);
echo "\n\n";
echo "Youtrack will be:\n";
echo $ytConverter->convertFilterQuery($queryFilter);
```