## 語言結構

### PHP 標籤

永遠使用長標籤： `<?php ?>` 來包裹 PHP 程式碼，不使用短標籤 `<? ?>`。這可以確保您的程式碼可執行在大多數未經設定的主機環境。

如果檔案中只包含PHP程式碼，則不應該包含結尾標籤 `?>`。這個標籤在 PHP 中不是必要的。這樣子可以避免在系統輸出前，不小心讓任何空白預先送出 header ，這會造成 Joomla 的 Session 功能錯誤 (參見 PHP 手冊的說明 [Instruction separation](http://php.net/basic-syntax.instruction-separation))。

檔案結尾必須永遠由一個空行結束。

### 引入程式

當您想要無條件引入一個檔案時，應使用 `require_once`。當您要有條件的引入一個檔案（例如 factory 類別與其方法），則用 `include_once`。這兩者都可以確保檔案不被二次載入。兩種用法共享檔案清單，所以混用不會有任何問題。被 `require_once` 引入的檔案不會再被 `include_once` 引入一次。

> **注意**
>
> `include_once` 與 `require_once` 是 PHP 關鍵字，不是函式，寫法應該如下:
>
>
> `require_once JPATH_COMPONENT . ’/helpers/helper.php’;`

您不應該把檔案路徑用括號包起來。

### 兼容 E_STRICT 的 PHP 程式

我們必須秉持並實踐 PHP 5.3 以上所支持的物件導向設計模式。Joomla! 一直致力於讓原始碼符合 E_STRICT 標準。

## 全域變數

盡量減少使用全域變數或超全域變數，用物件導向或工廠(Factory)模式替代之。

## 控制結構

所有的控制結構必須在關鍵字與起始括號之間放置一個空白字元，而開頭、結尾括號與邏輯判斷式之間無空格。這是為了區隔控制結構與函式以方便辨認，而括號內必須要包含邏輯。

所有的控制結構關鍵字，如： `if`, `else`, `do`, `for`, `foreach`, `try`, `catch`, `switch` 與 `while` 皆採 [Allman](http://en.wikipedia.org/wiki/Indent_style#Allman_style) 風格，關鍵字本身必須在一個新行，而語言區塊（大括號）的開頭與結束也接需再一個新行中。

### _if-else_ 範例

```php
if ($test)
{
	echo 'True';
}

// 註解可寫在此
// 要注意的是 "elseif" 採用單一單字而非拆開成 "else if"

elseif ($test === false)
{
	echo 'Really false';
}
else
{
	echo 'A white lie';
}
```

如果控制結構的判斷式需要多行，則第二行開始需要一個 Tab 縮排，結尾括號需要再同一行上。

```php
if ($test1
	&& $test2)
{
	echo 'True';
}
```

### _do-while_ 範例


```php
do
{
	$i++;
}
while ($i < 10);
```

### _for_ 範例

```php
for ($i = 0; $i < $n; $i++)
{
	echo 'Increment = ' . $i;
}
```

### _foreach_ 範例

```php
foreach ($rows as $index => $row)
{
	echo 'Index = ' . $id . ', Value = ' . $row;
}
```

### _while_ 範例

```php
while (!$done)
{
	$done = true;
}
```

### _switch_ 範例

當使用 `switch` 時， `case` 關鍵字必須縮排一次，而 `break` 關鍵字必須獨立在新的一行，且相較於 `case` 再縮排一次。

```php
switch ($value)
{
	case 'a':
		echo 'A';
		break;

	default:
		echo 'I give up';
		break;
}
```

## 參照

當使用變數參照時，參照符號應該距離等號一個空白字元，且跟後面的變數緊鄰，沒有空白。

範例:

```php
$ref1 = &$this->sql;
```

> **注意**
>
> 在 PHP 5 ，物件不再需要參照符號，所有物件皆是參照。

## 陣列

陣列元素的指派可稍微排版，當多行時，可用 Tab 縮排。每行跟隨一個都逗號結尾，最後一行可包含逗號，這是 PHP 允許的寫法，對於程式碼 diff 比對時也有所幫助。

範例:

```php
$options = array(
	'foo'	=> 'foo',
	'spam'	=> 'spam',
);
```

## 註解

行內註解可用 C 語言風格的 `/* ... */` 或 C++ 的單行註解 `// ...`。 C 風格的註解通常被用在文件開頭、類別、函式等等的文件標頭。而 C++ 風格單行註解則常用在程式解釋與提醒。註解對於幫助其他開發人員理解程式碼的目的有非常大的幫助，甚至包含您自己也能受惠。當程式碼開始進行複雜操作時，永遠記得要加上註解。至於 Perl 與 Shell 的井號 (`#`)註解則不建議使用，目前在 PHP 中也不再允許此類註解。

您可以在某些情況下藉由註解掉整段程式碼或類別函式以用於調整程式功能，但應該在最終提交到核心前去除這些註解或不再需要的程式區塊。

例如，不要在提交的程式碼中包含以下片段:

```php
// 這段程式碼未來需要修復
//$code = broken($fixme);
```

### 文件區塊註解

Documentation headers for PHP and Javascript code in files, classes, class properties, methods and functions, called the docblocks, follow a convention similar to JavaDoc or phpDOC.

These "DocBlocks" borrow from the PEAR standard but have some variations specific for the Joomla Framework.

Whereas normal code indenting uses real tabs, all whitespace in a Docblock uses real spaces. This provides better readability in source code browsers. The minimum whitespace between any text elements, such as tags, variable types, variable names and tag descriptions, is two real spaces. Variable types and tag descriptions should be aligned according to the longest Docblock tag and type-plus-variable respectively.

If the `@package` tag is used, it will be "Joomla.Platform".

If the `@subpackage` tag is used, it is the name of the top level folder under the /joomla/ folder. For example: Application, Database, Html, and so on.

Code contributed to the Joomla project that will become the copyright of the project is not allowed to include @author tags. You should update the contribution log in CREDITS.php. Joomla's philosophy is that the code is written "all together" and there is no notion of any one person "owning" any section of code. The `@author` tags are permitted in third-party libraries that are included in the core libraries.

Files included from third party sources must leave DocBlocks intact. Layout files use the same DocBlocks as other PHP files.

### 檔案文件標頭註解

文件標頭註解有以下內容且按照順序

-   簡易說明 (選填, 除非超過兩個以上類別或方法) 結束空一行.
-   完整說明 (選填) 結束空一行.
-   `@category` (選填)
-   `@package` (選填, 如果只有這份程式碼可不填)
-   `@subpackage` (選填)
-   `@author` (選填, 但只允許在非的Joomla源文件，例如，包括第三方庫)
-   `@copyright` (必填)
-   `@license` (必填且必須是兼容的Joomla許可證)
-   `@deprecated` (選填)
-   `@link` (選填)
-   `@see` (選填)
-   `@since` (通常是選填，但時所需的文件只包含程序代碼)

```
/**
 * @package     Joomla.Platform
 * @subpackage  Database
 * @copyright   Copyright 2005 - 2010 Open Source Matters. All rights re-served.
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */
```

## 方法呼叫

呼叫方法時, 方法名稱跟參數括號中不能有空白, 而第一個帶入參數也不得有空白; 每個參數逗點後一個空白隔開 (如果有的話), 最後一個參數跟括號沒有空格. 參數帶值等號前後需有空白. 可以多行對齊.

```php
// 單方法呼叫
$foo = bar($var1, $var2);

// 多方法呼叫
$short  = bar('short');
$medium = bar('medium');
$long   = bar('long');
```

## 宣告方法

宣告方法需換新行, 且開括號與結束括號接需換行. 一個空行需指定返回值.

宣告方法需含有註解, 且按照註解規則.

-   簡易說明 (必填, 後空行)
-   完整說明 (選填, 後空行)
-   `@param` (必填, 在 `@param` 空格 參數型態 空格 參數名稱 空格 功能)
-   `@return` (必填, 前後需空行)
-   標籤需按照字母排序, 然而 `@since` 必填.


```php
/**
 * A utility class.
 *
 * @package     Joomla.Platform
 * @subpackage  XBase
 *
 * @param       string  $path  The library path in dot notation.
 *
 * @return      void
 *
 * @since       1.6
 */
function jimport($path)
{
	// Body of method.
}
```

如果多行參數所有段行第一個參數需縮排, 結束刮續需跟最後個參數同一行


```php
function fooBar($param1, $param2,
	$param3, $param4)
{
	// Body of method.
}
```

## Class Definitions

Class definitions start on a new line and the opening and closing braces are also placed on new lines. Class methods must follow the guidelines for Function Definitions. Properties and methods must follow OOP standards and be declared appropriately (using public, protected, private and static as applicable).

Class definitions, properties and methods must each be provided with a DocBlock in accordance with the following sections.

### Class DocBlock Headers

The class Docblock consists of the following required and optional elements in the fol-lowing order.

-   Short description (required, unless the file contains more than two classes or functions), followed by a blank line).
-   Long description (optional, followed by a blank line).
-   `@category` (optional and rarely used)
-   `@package` (required)
-   `@subpackage` (optional)
-   `@author` (optional but only permitted in non-Joomla source files, for example, included third-party libraries like Geshi)
-   `@copyright` (optional unless different from the file Docblock)
-   `@license` (optional unless different from the file Docblock)
-   `@deprecated` (optional)
-   `@link` (optional)
-   `@see` (optional)
-   `@since` (required, being the version of the software the class was introduced)

### Class Property DocBlocks

The class property Docblock consists of the following required and optional elements in the following order.

-   Short description (required, followed by a blank line)
-   `@var` (required, followed by the property type)
-   `@deprecated` (optional)
-   `@since` (required)

### Class Method DocBlocks

The DocBlock for class methods follows the same convention as for PHP functions (see above).

```php
/**
 * A utility class.
 *
 * @package     Joomla.Platform
 * @subpackage  XBase
 * @since       1.6
 */
class JClass extends JObject
{
	/**
	 * Human readable name
	 *
	 * @var    string
	 * @since  1.6
	 */
	public $name;

	/**
	 * Method to get the name of the class.
	 *
	 * @param   string  $case  Optionally return in upper/lower case.
	 *
	 * @return  boolean  True if successfully loaded, false otherwise.
	 *
	 * @since   1.6
	 */
	public function getName($case = null)
	{
		// Body of method.

		return $this->name;
	}
}
```

## Naming Conventions

### Classes

Classes should be given descriptive names. Avoid using abbreviations where possible. Class names should always begin with an uppercase letter and be written in CamelCase even if using traditionally uppercase acronyms (such as XML, HTML). One exception is for Joomla Framework classes which must begin with an uppercase 'J' with the next letter also being uppercase.

For example:

-   JHtmlHelper
-   JXmlParser
-   JModel

### Functions and Methods

Functions and methods should be named using the "studly caps" style (also referred to as "bumpy case" or "camel caps"). The initial letter of the name is lowercase, and each letter that starts a new "word" is capitalized. Function in the Joomla framework must begin with a lowercase 'j'.

For example:

-   connect();
-   getData();
-   buildSomeWidget();
-   jImport();
-   jDoSomething();

Private class members (meaning class members that are intended to be used only from within the same class in which they are declared) are preceded by a single underscore. Properties are to be written in underscore format (that is, logical words separated by underscores) and should be all lowercase.

For example:

```php
class JFooHelper
{
	protected $field_name = null;

	private $_status = null;

	protected function sort()
	{
	}
}
```

### Constants

Constants should always be all-uppercase, with underscores to separate words. Always prefix constants with `J` and use common sense when completing the name to keep it from getting unnecessarily long. You *should* use the name of the class/package they are used in. For example, the constants used by the `Joomla\Application\Web\Router` class could begin with `JROUTE_`.

### Global Variables

Do not use global variables. Use static class properties or constants instead of globals.

### Regular Variables and Class Properties

Regular variables, follow the same conventions as function.

Class variables should be set to null or some other appropriate default value.

## Exception Handling

Exceptions should be used for error handling.

The following sections outline how to semantically use [SPL exceptions](http://php.net/manual/en/spl.exceptions.php).

### Logic Exceptions

The LogicException is thrown when there is an explicit problem with the way the API is being used. For example, if a dependency has failed (you try to operate on an object that has not been loaded yet).

The following child classes can also be used in appropriate situations:

#### BadFunctionCallException

This exception can be thrown if a callback refers to an undefined function or if some arguments are missing. For example if `is_callable()`, or similar, fails on a function.

#### BadMethodCallException

This exception can be thrown if a callback refers to an undefined method or if some arguments are missing. For example `is_callable()`, or similar, fails on a class method.  Another example might be if arguments passed to a magic call method are missing.

#### InvalidArgumentException

This exception can be thrown if there is invalid input.

#### DomainException

This exception is similar to the InvalidArgumentException but can be thrown if a value does not adhere to a defined valid data domain. For example trying to load a database driver of type "mongodb" but that driver is not available in the API.

#### LengthException

This exception can be thrown is a length check on an argument fails. For example a file signature was not a specific number of characters.

#### OutOfRangeException

This exception has few practical applications but can be thrown when an illegal index was requested.

### Runtime Exceptions

The RuntimeException is thrown when some sort of external entity or environment causes a problem that is beyond your control providing the input is valid. This exception is the default case for when the cause of an error can't explicitly be determined. For example you tried to connect to a database but the database was not available (server down, etc).  Another example might be if an SQL query failed.

#### UnexpectedValueException

This type of exception should be used when an unexpected result is encountered. For example a function call returned a string when a boolean was expected.

#### OutOfBoundsException

This exception has few practical applications but may be thrown if a value is not a valid key.

#### OverflowException

This exception has few practical applications but may be thrown when you add an element into a full container.

#### RangeException

This exception has few practical applications but may be thrown to indicate range errors during program execution. Normally this means there was an arithmetic error other than under/overflow. This is the runtime version of DomainException.

#### UnderflowException

This exception has few practical applications but may thrown when you try to remove an element of an empty container.

### Documenting exceptions

Each function or method must annotate the type of exception that it throws using an @throws tag and any downstream exceptions types that are thrown. Each type of exception need only be annotated once. No description is necessary.

## SQL Queries

SQL keywords are to be written in uppercase, while all other identifiers (with the exception of quoted text obviously) is to be in lowercase.

All table names should use the `#__` prefix rather than `jos_` to access Joomla content and allow for the user defined database prefix to be applied. Queries should also use the JDatabaseQuery API.

```php
// Get the database connector.
$db = JFactory::getDBO();

// Get the query from the database connector.
$query = $db->getQuery(true);

// Build the query programatically (using chaining if desired).
$query->select('u.*')
	// Use the qn alias for the quoteName method to quote table names.
	->from($db->qn('#__users').' AS u'));

// Tell the database connector what query to run.
$db->setQuery($query);

// Invoke the query or data retrieval helper.
$users = $db->loadObjectList();
```
