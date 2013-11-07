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

### File DocBlock Headers

The file header DocBlock consists of the following required and optional elements in the following order:

-   Short description (optional unless the file contains more than two classes or functions), followed by a blank line).
-   Long description (optional, followed by a blank line).
-   `@category` (optional and rarely used)
-   `@package` (generally optional but required when files contain only procedural code)
-   `@subpackage` (optional)
-   `@author` (optional but only permitted in non-Joomla source files, for example, included third-party libraries like Geshi)
-   `@copyright` (required)
-   `@license` (required and must be compatible with the Joomla license)
-   `@deprecated` (optional)
-   `@link` (optional)
-   `@see` (optional)
-   `@since` (generally optional but required when files contain only procedural code)

```
/**
 * @package     Joomla.Platform
 * @subpackage  Database
 * @copyright   Copyright 2005 - 2010 Open Source Matters. All rights re-served.
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */
```

## Function Calls

Functions should be called with no spaces between the function name and the opening parenthesis, and no space between this and the first parameter; a space after the comma between each parameter (if they are present), and no space between the last parameter and the closing parenthesis. There should be space before and exactly one space after the equals sign. Tab alignment over multiple lines is permitted.

```php
// An isolated function call.
$foo = bar($var1, $var2);

// Multiple aligned function calls.
$short  = bar('short');
$medium = bar('medium');
$long   = bar('long');
```

## Function Definitions

Function definitions start on a new line and the opening and closing braces are also placed on new lines. An empty line should precede lines specifying the return value.

Function definitions must include a documentation comment in accordance with the Commenting section of this document.

-   Short description (required, followed by a blank line)
-   Long description (optional, followed by a blank line)
-   `@param` (required if there are method or function arguments, the last `@param` tag is followed by a blank line)
-   `@return` (required, followed by a blank line)
-   All other tags in alphabetical order, however @since is always required.


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

If a function definition goes over multiple lines, all lines must be indented with one tab and the closing brace must go on the same line as the last parameter.


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

## 例外處理

例外處理應當只用於錯誤處理上。

下面的章節將指出如何照語意使用 [SPL exceptions](http://php.net/manual/en/spl.exceptions.php)。

### 邏輯例外

在 API 的使用方式上發生的明確問題會丟出 LogicException 的例外。例如：如果有個依賴參數失效時（你試圖操作一個未被載入的物件）。

下方的子類別也可被用於適當的情境下：

#### BadFunctionCallException

如果一個 callback 參照到一個未定義的函式(function) 或是缺少了一些參數時可以丟出該例外。例如：如果 `is_callable()` 或其他類似的函式用在一個函式(function)上時得到失敗的結果。

#### BadMethodCallException

如果一個 callback 參照到一個未定義的方法(method) 或是缺少了一些參數時可以丟出該例外。例如：如果 `is_callable()` 或其他類似的函式用在一個方法(method)上時得到失敗的結果。 另一個例子可能是找不到某些使用魔術方法的參數。

#### InvalidArgumentException

當有不合規格的輸入發生時丟出該例外。

#### DomainException

該例外和 InvalidArgumentException 類似，但會在一數值未依附在一已定義的有效資料群的情形下丟出該例外。例如：試圖載入一個"mongodb"的資料庫引擎，但該引擎無法使用在 API 下。

#### LengthException

當作一參數的長度檢查失敗時丟出該例外。例如：一個檔案的雜湊值不是一個指定長度的字串。

#### OutOfRangeException

該例外較少被實際的應用，但可在存取一個非法的索引值時丟出該例外。

### Runtime 例外

在外部實體或環境造成你無法控制的問題所產生的錯誤輸入時丟出 RuntimeException 的例外。當一個錯誤的產生不能明確地被判斷出來時，預設丟出該例外。例如：你試圖連到資料庫但資料庫卻無法使用 (伺服器掛掉之類的)。另一個例子可能為 SQL 查詢語句的錯誤。

#### UnexpectedValueException

當一個非預期的結果發生時應當丟出該種類的例外。例如：當一個函式預期是要回傳布林值卻回傳字串時。

#### OutOfBoundsException

該例外較少實際的應用，但也許當某個值不為合法的索引值時丟出該例外。

#### OverflowException

該例外較少實際的應用，但也許當你加入一個元素到已經滿載的容器物件時丟出該例外。

#### RangeException

該例外較少實際的應用，但也許在程式執行期間丟出該例外以指出一個範圍的錯誤。通常這是指除了運算虧位/溢位之外的錯誤。該例外為 DomainException 的 runtime 版本。

#### UnderflowException

該例外較少實際的應用，但也許當你從一個空的容器物件中移除一個元素時丟出該例外。

### 例外處理的文件撰寫

每個函式(function)或方法(method)必須要註解使用到的例外，用 @throws 的標籤和丟出例外的物件名稱作註解。每種例外只須備被註解一次。註解的說明非必須。

## SQL 查詢語句

SQL 的保留字要全用大寫，其他的文字符號 (當然被引號包起來的文字為例外) 為小寫。

全部的 table 名稱必須要用 `#__` 的前綴字而不是用 `jos_` 去讀取 Joomla 的內容，並允許替換為使用者定義的資料庫前綴字。查詢語句也必須使用 JDatabaseQuery API。

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
