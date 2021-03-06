# Rational Meta Box Class

## Description

PHP Class for building WordPress meta boxes using multidimensional, associative arrays.

- Uses [WordPress' native functions](https://codex.wordpress.org/Function_Reference/add_meta_box)
- Supports all, basic HTML and HTML5 inputs and attributes
- Reduces a lot of redundant callback programming

## Installation

```php
function rational_meta_boxes() {
	require_once( 'inc/class.rational-meta-box.php' );
	$rational_meta_box = new RationalMetaBoxes();
	$rational_meta_box->generate_boxes();
}
add_action( 'admin_init', 'rational_meta_boxes' );
```
1. Create a function to hook into WordPress' `admin_init` hook.
2. `require_once` the Rational Meta Box class.
3. Instantiate the class.
4. Trigger the generation of the boxes with `generate_boxes()` method.

## Customizing
```php
$my_boxes = array(
	'my-box-one'	=> array(
		'title'			=> 'My Box One',
		'screen'		=> array( 'post', 'page' ),
		'description'	=> 'My custom meta box description',
		'fields'		=> array(
			'my-text-field'	=> array(
				'type'			=> 'text',
				'label'			=> 'My Text Field',
				'description'	=> 'Instructions or an explanation of the field',
			),
		),
	),
);
$rational_meta_box = new RationalMetaBoxes( $my_boxes );
```

1. Create an array with the top level being the attributes of your meta box and fields within that.
2. Pass the array to the function that initializes the Rational Meta Box class.
3. Check out your meta box on the given post type(s)' edit page.

![Screenshot of rendered 'my_boxes' array](http://i.imgur.com/IevTxdS.jpg)

## Parameters

### Meta Box Options

See [WordPress' `add_meta_box` function](https://codex.wordpress.org/Function_Reference/add_meta_box#Parameters) for more information.

- **$id**: (required, string, preferably hyphenated) Provided as the 'key' for the array of attributes.
- **$title**: (required, string) Defines the "title" of the meta box.
- **$screen**: (optional, string or array) Where the meta box will appear. Can be string or an array of strings.  
(`'post', 'page','dashboard','link','attachment','custom_post_type'`)  
Default: `'post'`
- **$description**: (optional, string) A description for the meta box.
- **$context**: (optional, string) Position on the editor window.  
(`'normal', 'advanced', or 'side'`)  
Default: `'advanced'`
- **$priority**: (optional, string) The priority to give the meta box.  
(`'high', 'core', 'default' or 'low'`)  
Default: `'default'`
- **$fields**: (optional, array) An array of multidimensional, associative arrays containing field data. See [Field Options](#field-options) below.

Example:
```php
$my_boxes = array(
	'meta-box-id'   => array(
		'title'         => 'Meta Box One',
		'screen'        => array( 'post', 'page' ),
		'description'   => 'Description of meta box',
		'context'       => 'side',
		'priority'      => 'core',
		'fields'        => array(
			'text-field-id' => array(
				'type'          => 'text',
				'label'         => 'Field Label',
				'description'   => 'Instructions or an explanation of the field',
			),
		),
	),
);
```
<a name="field-options" id="field-options"></a>
### Field Options
- **$id**: (required, string, preferably hyphenated) Provided as the 'key' for the array of field attributes.
- **$type**: (optional, string) The type of input to generate.  
(`'file', 'checkbox', 'color', 'date', 'month', 'week', 'datetime', 'datetime-local', 'email', 'number', 'password', 'radio', 'range', 'search', 'select', 'tel', 'text', 'time', 'textarea', 'url'`)  
Default: `'text'`
- **$label**: (required, string) Identifying text for the left column. Serves as a label for some input types.
- **$description**: (optional, string) Serves as 'help text' for most elements. Also serves as a label for checkboxes.
- **$value**: (optional, string or integer) The default value of the field. (_Note: In the case of checkboxes this server as the value of the checkbox written to the database when checked. If none set, it uses '1'_)
- **$options**: (radio and select types only, required, array) Used to define the options available in radio and select inputs. See [the example](#example-field-options) for more detail.
- **$other**: (optional, array) More input specific attributes defined below and supplied in an associative array.
    - **$autocomplete**: (optional, string) Defines whether or not the browser can "autocomplete" this field.  
    (`'on', 'off'`)  
    Default: `'on'`
    - **$autofocus**: (optional, boolean) If set to true, the last field in the HTML of the page, with this parameter, will be the first one focused on page load.
    - **$char_count**: ("text" type fields only, optional, integer) If provided, adds a character count box to the bottom, right of the input. _Note: Doesn't limit the number of characters. Use **$maxlength** to control that aspect._
    - **$class**: (optional, string) Defines the class(es) applied to the field.  
    Default: `'regular-text'` for most fields, `'large_text'` for textareas.
    - **$cols**: (textarea only, optional, integer) Defines the number of columns of the given textarea. (_Note: **$class** should probably be set to a blank or `null` value to use **$cols** properly._)
    - **$disabled**: (optional, boolean) If set to true the field will be disabled on load.
    - **$max**: (date and number types only, optional, integer) Defines the upper possible boundary for the value of the field.
    - **$maxlength**: ("text" type fields only, optional, integer) Sets the upper limit of characters for the value of the field.
    - **min**: ("text" type fields only, optional, integer) Defines the lower possible boundary for the value of the field.
    - **$pattern**: ("text" type fields only, optional, string/regexp) Regular expression to validate the value against before submitting.
    - **$placeholder**: ("text" type fields only, optional, string) HTML5 placeholder value.
    - **$readonly**: (optional, boolean) If set to true the field will be only allow for reading, not editing.
    - **$required**: (optional, boolean) If set to true the field will be required.
    - **$rows**: (textarea only, optional, integer) Defines the number of row of the given textarea.
    - **$step**: (range and number only, optional, integer) Defines the amount of each "step" in between the **$min** and **$max** values for a range or number field.



## Examples
<a name="example-field-options" id="example-field-options"></a>
### Radio and Select Options Attribute
**`'select'` type**
```php
'options'   => array(
	'Choose One&hellip;',
	'Option One',
	'option-two'    => 'Option Two',
),
```
![Screenshot of rendered select element with options](http://i.imgur.com/VKuhig4.jpg)

You can use either the value itself or a key/value pair where the 'key' parameter defines the option or radio input's value and the 'value' parameter defines the human readable text part.

**`'radio'` type**
```php
'options'   => array(
	'Option One',
	'option-two'    => 'Option Two',
),
```

![Screenshot of the rendered radio input options](http://i.imgur.com/dpCe52B.jpg)

If you choose not to use the key/value method the script will generate a machine readable version of the provided text for use in the value. For instance the string 'Option One' would be converted to `'option-one'`.

## License

The MIT License (MIT)

Copyright (c) 2015 Jeremy Hixon.

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.