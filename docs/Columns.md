# Columns
The column definition is a central part of ZfcDatagrid.

There are currently 3 types of columns: `Action`, `ExternalData` and `Select`
Most time you will use `Select`, which just says that this column will use data from your defined datasource.
`ExternalData` column is used, if you want to enhance your dataset with external data like the image from gravatar.
`Action` is like the name says for row actions.

A minimal column looks like
```php
use ZfcDatagrid\Column;

$col = new Column\Select('displayName');
$col->setLabel('Displayname');
$grid->addColumn($col);
```

## Options
All 3 column types share together an abstract class, there following options are possible:
```php
use ZfcDatagrid\Column;
use ZfcDatagrid\Column\Formatter;
use ZfcDatagrid\Column\Type;
use ZfcDatagrid\Column\Style;
use ZfcDatagrid\Filter;

$col = new Column\Select('myColumn');

//The displayed column label
$col->setLabel('Label');

//a uniqueId is auto generated, but you can override this way
$col->setUniqueId('myUniqueId');

//the width is in percent, because there are different output modes. If the sum of all percentage is not 100 it's adjusted automatically by the grid!
$col->setWidth(20);

 //hide or show this column
$col->setHidden($bool);

//set this as a `primaryKey` column (all identity columns will be used together as the ID for actions and rowIds)
$col->setIdentity($bool); 

//set the columnType. Default is string (other types see later)
//types also changes things: change filtering, formatting, styling ...
$col->setType(new Type\Image());

//format the value with this formatter
//many things are already good when using the right Type!
$col->setFormatter(new Formatter\FileSize());

//you can set different styles for columns
//you can also combine multiple styles
$col->addStyle(new Style\Bold());

//style can also be applied on different values only!
$style = new Style\Color(Style\Color::$RED);
$style->setByValue($col, 20, Filter::EQUAL); //or NOT_EQUAL
$col->addStyle($style);

//you can add multiple class names
//by default bootstrapTable will add the class name 'btn' when type is of 'Action'
$col->addClass('btn-primary'); // generates "btn btn-primary"

//you can override this default behaviour by (re-)setting the complete class attribute
$col->setAttribute('class', 'non-twb-class'); // overrides all existing class names

//disable or enable user sorting on this column
//default: on, e.g. on Type\Image default disabled)
$col->setUserSortDisabled($bool);

//Sort by this column, as long the user does not override sorting
//you can define multiple sort columns! (just increase the priority)
$col->setSortDefault(1, 'ASC');

//Disable or enable column filtering
//is also automatically disabled e.g. on Type\Image
$col->setUserFilterDisabled($bool);

//filter the data by this value, as long no user filtering is taken
$col->setFilterDefaultValue('myFilterValue');

//adjust the filter operator (default is LIKE, dependend on the type)
$col->setFilterDefaultOperation(Filter::LIKE_RIGHT);

//replace the retrieved data values with those
$replaces = array('m' => 'male', 'f' => 'female');
$col->setReplaceValues($replaces, $boolNotReplacedGetEmpty);

//enable translation for this column data
$col->setTranslationEnabled($bool);

//set filter options (if u use replaceValues, this is already done automatically)
$options = array('m' => 'male', 'f' => 'female');
$col->setFilterSelectOptions($options);

//only jqGrid currently (custom display)
$col->setRendererParameter('formatter', '
    function (value, options, rowObject) {
    	//do something custom
    	return value;
	}
', 'jqgrid');

//disable the rowClick action on this column (if a row click action is available)
$col->setRowClickDisabled($bool);

$grid->addColumn($col);
```






Advanced
```php
use ZfcDatagrid\Column;

$col = new Column\Select('displayName', 'tableNameOrAlias');
$col->setLabel('Displayname');
$col->setWidth(25);
$col->setSortDefault(1, 'ASC');
$col->addStyle(new Style\Bold());
$grid->addColumn($col);
```
