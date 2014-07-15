﻿# စိစစ္ျခင္း

- [အေျခခံအသုံးျပဳပုံ](#basic-usage)
- [Working With Error Messages](#working-with-error-messages)
- [Error Messages & Views](#error-messages-and-views)
- [Available Validation Rules](#available-validation-rules)
- [Conditionally Adding Rules](#conditionally-adding-rules)
- [Custom Error Messages](#custom-error-messages)
- [Custom Validation Rules](#custom-validation-rules)

<a name="basic-usage"></a>
## အေျခခံအသုံးျပဳပုံ

Laravel အေနျဖင့္ data မ်ားကုိ စိစစ္ရာတြင္ ရုိးရွင္း အဆင္ေျပေသာ နည္းလမ္းမ်ားကုိ အသုံးျပဳထားသည္။ error message မ်ားကုိ `Validation` class မွ တဆင့္ ထုတ္ယူႏုိင္သည္။

#### အေျခခံအသုံးျပဳပုံ ဥပမာ

	$validator = Validator::make(
		array('name' => 'Dayle'),
		array('name' => 'required|min:5')
	);

Validation ျပဳလုပ္ရာတြင္  `make` method ဟုသည့္ method ကုိ အသုံးျပဳျပီး array တြင္းပါရွိမည့္ ပထမ argument မွာ data ျဖစ္ျပီး ဒုတိယ argument မွာ ထုိ data မ်ားကုိ စိစစ္မည့္ rule မ်ားကို ထည့္သြင္းရမည္။


#### Array ကုိ အသုံးျပဳ၍ Rule မ်ား သတ္မွတ္ျခင္း

တခုထက္ပုိေသာ rule မ်ားကို သတ္မွတ္လုိပါက "pipe" character ကုိျဖစ္ေစ array အတြင္း ျခား၍ျဖစ္ေစ ထည့္သြင္းႏုိင္သည္။

	$validator = Validator::make(
		array('name' => 'Dayle'),
		array('name' => array('required', 'min:5'))
	);

#### Fields မ်ားစြာကုိ စိစစ္ျခင္း

    $validator = Validator::make(
        array(
            'name' => 'Dayle',
            'password' => 'lamepassword',
            'email' => 'email@example.com'
        ),
        array(
            'name' => 'required',
            'password' => 'required|min:8',
            'email' => 'required|email|unique:users'
        )
    );

`Validator` instance ကုိ ျပဳလုပ္ျပီးပါက `fails` သုိ ့မဟုတ္ `passes` method ကုိ အသုံးျပဳ၍ အခ်က္အလက္မ်ား စိစစ္ႏုိင္သည္။


	if ($validator->fails())
	{
		// The given data did not pass validation
	}

စိစစ္ျခင္း မေအာင္ျမင္ပါက validator မွ error message ကုိ ရယူႏုိင္ေပသည္။

	$messages = $validator->messages();

္fail ျဖစ္သည့္ rule မ်ားကုိသာ ရယူလုိျပီး message မ်ား မပါဝင္ေစလုိပါက `failed` method ကုိ အသုံးျပဳႏုိင္သည္။

	$failed = $validator->failed();

#### Files မ်ားစိစစ္ျခင္း

`Validator` class အေနျဖင့္ `size` ႏွင့္ `mimes` အပါအဝင္ မ်ားေျမာင္လွေသာ validation method မ်ားကုိ အေထာက္အပံ့ေပးထားျပီး file မ်ား validate ျပဳလုပ္လုိပါက ထုိထဲ့သုိ ့ ထည့္သြင္းရန္သာ လုိေပမည္။


<a name="working-with-error-messages"></a>
## Error Messages မ်ားႏွင့္ လႈပ္ရွားျခင္း


After calling the  on a 
`Validator` instance မွ `messages` method ကုိ ေခၚျပီးပါက Error message မ်ားျဖင့္ အလုပ္လုပ္ရာတြင္ လြယ္ကူေစမည့္ method မ်ားစြာပါဝင္မည့္ `MessageBag` ပါဝင္မည္ ျဖစ္သည္။

#### Field တစ္ခုမွ ပထမဆုံး Error Message ကုိ ထုတ္ယူျခင္း

	echo $messages->first('email');

#### Field တစ္ခုမွ Error Message မ်ားထုတ္ယူျခင္း

	foreach ($messages->get('email') as $message)
	{
		//
	}

#### Field အားလုံးမွ Error Message မ်ားထုတ္ယူျခင္း

	foreach ($messages->all() as $message)
	{
		//
	}

#### Field တစ္ခုမွ message ရွိမရွိ စစ္ေဆးျခင္း

	if ($messages->has('email'))
	{
		//
	}

#### Error Message မ်ားအား Format ေျပာင္း၍ ထုတ္ယူျခင္း

	echo $messages->first('email', '<p>:message</p>');
	
> **မွတ္ခ်က္:**  ပုံမွန္အားျဖင့္ messages မ်ားကုိ Bootstrap ျဖင့္ အဆင္ေျပမည့္ ပုံစံမ်ားအေနျဖင့္ သတ္မွတ္ထားပါသည္။

#### Error Messages မ်ားအား Format တစ္ခု သတ္မွတ္၍ ထုတ္ယူျခင္း

	foreach ($messages->all('<li>:message</li>') as $message)
	{
		//
	}

<a name="error-messages-and-views"></a>
## Error Message မ်ားႏွင့္ View မ်ား

Validation ကုိ ေဆာင္ရြက္ျပီးသည္ႏွင့္ Error message မ်ားကုိ လြယ္လင့္တကူ ျပန္လည္ျပသႏုိင္ရန္ လုိအပ္ေပသည္။ ထုိလုိအပ္ခ်က္မ်ားကုိ Laravel မွ အဆင္ေျပလြယ္ကူစြာ ျဖည့္စြမ္းထားသည္။ ေအာက္ပါ route မ်ားကုိ ဥပမာ အေနျဖင့္ၾကည့္ပါ။

	Route::get('register', function()
	{
		return View::make('user.register');
	});

	Route::post('register', function()
	{
		$rules = array(...);

		$validator = Validator::make(Input::all(), $rules);

		if ($validator->fails())
		{
			return Redirect::to('register')->withErrors($validator);
		}
	});

	
Note that when စိစစ္ျခင္း မေအာင္ျမင္ပါက `Validator` instance ကုိ `withErrors` method ျဖင့္ Error မ်ားကို passing ေပးလုိက္ျပီး Redirect ျပဳလုပ္လုိက္သည္ ကုိ ေတြ ့ရေပမည္။ အဆုိပါ method ကုိ အသုံးျပဳျခင္းျဖင့္ error message မ်ားကို လြယ္လင့္တကူ ျဖတ္ကနဲ  ျပသရာတြင္ သုံးႏုိင္ရင္ next request ၏ Session ထဲတြင္ ထည့္သြင္းထားပါသည္။

သုိ ့ပင္ေသာ္ညား GET route နဲ ့ Error Message မ်ားကုိ အေသခ်ည္ေႏွာင္ထားရန္ မလုိသည္ကို သတိျပဳရမည္။ အဘယ္ေၾကာင့္ဆုိေသာ္ Laravel သည္ Session data မ်ားမွ Error မ်ားကုိ စစ္ေဆးျပီး view ဆီသုိ ့ အဆင္ေျပသည္ႏွင့္ တျပိဳင္နက္ ျပသႏုိင္ရန္ ျပင္ဆင္ထားသည္ကုိ သတိျပဳရမည္။ **ထုိေၾကာင့္ အေရးၾကီးသည့္ အခ်က္မွာ`$errors` ဟုသည္ variable မွာ သင့္ view ၏ request တုိင္းတြင္ ျပင္ဆင္ထားေသာေၾကာင့္ အျမဲတမ္း အဆင္သင့္ ျဖစ္ေနသည္ကုိ မွတ္ထားရန္လုိသည္။ `$errors` variable မွာ `MessageBag` ၏ instance ျဖစ္သည္။

ထုိေၾကာင့္ redirect ျပဳလုပ္ျပီးေနာက္ `$errors` variable ႏွင့္ သင့္ view မွာ အလုိအေလ်ာက္ ခ်ည္ေႏွာင္ျပီးသား ျဖစ္ေပသည္။

	<?php echo $errors->first('email'); ?>

### အမည္ေပးထားေသာ Error Bag မ်ား

သင့္အေနျဖင့္ Page တစ္ခုတည္းတြင္ မ်ားျပားလွေသာ form မ်ားသည္ရွိသည္ ဆုိပါစုိ ့။ ထုိအခါ သင့္အေနျဖင့္ Error မ်ား၏ `MessageBag` မ်ားကုိ ကြဲျပားျခားနား ေစရန္ အမည္နာမ ေပးလုိေပမည္။ ထုိအခါတြင္ သင့္အေနျဖင့္ `withErrors` ဟုသည့္ method ၏ ဒုတိယ argument အေနျဖင့္ မိမိေပးလုိသည့္ အမည္ကုိ ထည့္သြင္းႏုိင္သည္။

	return Redirect::to('register')->withErrors($validator, 'login');

ထုိေနာက္ သင့္အေနျဖင့္ `$errors` variable မွ `MessageBag` instance ကုိ ေအာက္ပါအတုိင္း ဆြဲထုတ္ႏုိင္သည္။

	<?php echo $errors->login->first('email'); ?>

<a name="available-validation-rules"></a>
## အသုံးျပဳႏုိင္သည့္ စိစစ္ျခင္း Rule မ်ား

ေအာက္တြင္ ေဖာ္ျပထားသည္မွာ အသုံးျပဳႏုိင္ေသာ စိစစ္ေရး rule မ်ားႏွင့္ ၄င္းတုိ ့၏ function မ်ားျဖစ္ၾကသည္။

- [Accepted](#rule-accepted)
- [Active URL](#rule-active-url)
- [After (Date)](#rule-after)
- [Alpha](#rule-alpha)
- [Alpha Dash](#rule-alpha-dash)
- [Alpha Numeric](#rule-alpha-num)
- [Array](#rule-array)
- [Before (Date)](#rule-before)
- [Between](#rule-between)
- [Confirmed](#rule-confirmed)
- [Date](#rule-date)
- [Date Format](#rule-date-format)
- [Different](#rule-different)
- [Digits](#rule-digits)
- [Digits Between](#rule-digits-between)
- [E-Mail](#rule-email)
- [Exists (Database)](#rule-exists)
- [Image (File)](#rule-image)
- [In](#rule-in)
- [Integer](#rule-integer)
- [IP Address](#rule-ip)
- [Max](#rule-max)
- [MIME Types](#rule-mimes)
- [Min](#rule-min)
- [Not In](#rule-not-in)
- [Numeric](#rule-numeric)
- [Regular Expression](#rule-regex)
- [Required](#rule-required)
- [Required If](#rule-required-if)
- [Required With](#rule-required-with)
- [Required With All](#rule-required-with-all)
- [Required Without](#rule-required-without)
- [Required Without All](#rule-required-without-all)
- [Same](#rule-same)
- [Size](#rule-size)
- [Unique (Database)](#rule-unique)
- [URL](#rule-url)

<a name="rule-accepted"></a>
#### accepted

အဆုိပါ field တြင္ စိစစ္သည္မွာ  _yes_, _on_, သုိ ့မဟုတ္  _1_  တုိ ့ျဖစ္သည္။ "Terms of Service" ကဲ့သုိ ့ တခုသာ ေရြးမေရြး စိစစ္ရာေနရာမ်ားတြင္ ၄င္းကုိ အသုံးျပဳႏုိင္သည္။


<a name="rule-active-url"></a>
#### active_url

အဆုိပါ field တြင္ စိစစ္သည္မွာ `checkdnsrr` ဟုသည္ PHP function ကုိ အသုံးျပဳ၍ အင္ထုထားသည့္ URL ဟုတ္မဟုတ္ကို စစ္ေဆးသြားမည္ ျဖစ္သည္။

<a name="rule-after"></a>
#### after:_date_

အဆုိပါ field တြင္ စိစစ္သည္မွာ သတ္မွတ္ထားေသာ date အတြင္းတြင္သာ ထည့္သြင္းေစရႏ္ ျဖစ္သည္။ date မ်ားကုိ  PHP ၏ `strtotime` function ကုိ အသုံးျပဳ၍ ေျပာင္းလဲကာ စိစစ္သြားမည္ ျဖစ္သည္။

<a name="rule-alpha"></a>
#### alpha
အဆုိပါ field တြင္ ပါဝင္ေသာ အခ်က္အလက္မ်ားသည္ အကၡရာ မ်ားသာ ျဖစ္ရမည္ ျဖစ္သည္။ ဥပမာ ကိန္းဂဏန္းမ်ားကို လက္ခံသြားမည္ မဟုတ္။

<a name="rule-alpha-dash"></a>
#### alpha_dash

အဆုိပါ field တြင္ ပါဝင္ေသာ အခ်က္အလက္မ်ားသည္ အကၡရာ ႏွင့္ ကိန္းဂဏန္းမ်ားသာ မက dash ႏွင့္ underscore ကုိပါ လက္ခံသြားမည္ ျဖစ္သည္။

<a name="rule-alpha-num"></a>
#### alpha_num

အဆုိပါ field တြင္ ပါဝင္ေသာ အခ်က္အလက္မ်ားသည္ အကၡရာ ႏွင့္ ကိန္းဂဏန္းမ်ားသာ လက္ခံသြားမည္။

<a name="rule-array"></a>
#### array

အဆုိပါ field တြင္ ပါဝင္ေသာ အခ်က္အလက္မ်ားသည္ array အမ်ိဳးအစားကိုသာ လက္ခံသြားမည္။

<a name="rule-before"></a>
#### before:_date_

အဆုိပါ field တြင္ပါဝင္ေသာ အခ်က္အလက္မ်ားကို date ျဖင့္ စိစစ္သတ္မွတ္ျခင္း ျဖစ္သည္။ dates မ်ားကုိ PHP မွ `strtotime` function ကုိ အသုံးျပဳ၍ passing ေပးသြားမည္ ျဖစ္သည္။

<a name="rule-between"></a>
#### between:_min_,_max_

အဆုိပါ field တြင္ထည့္သြင္းေသာ အခ်က္အလက္မ်ား ၏ အမ်ားဆုံးႏွင့္ အနည္းဆုံး တန္ဖုိးမ်ားကုိ သတ္မွတ္ျခင္း ျဖစ္ျပီး String ၊ numeric ႏွင့္ file မ်ားကို `size` rule ကုိ အသုံးျပဳသကဲ့သုိ ့ ဆင္တင္တင္ပင္ ျဖစ္သည္။

<a name="rule-confirmed"></a>
#### confirmed

အဆုိပါ field ၏ အခ်က္အလက္သည္ ရည္ညြန္း field ၏ အခ်က္အလက္ ဥပမာ `foo_confirmation`  ႏွင့္ တူညီရမည္ ျဖစ္သည္။ ဥပမာ ျပဳရေသာ္ `password` field သည္ `password_confirmation` field ႏွင့္ ထပ္တူညီရမည္ ျဖစ္သည္။

<a name="rule-date"></a>
#### date

တိက် မွန္ကန္ေသာ date ျဖစ္ေစရန္ စိစစ္ေပးျပီး `strtotime` ဟူေသာ PHP function ကုိ အသုံးျပဳထားသည္။

<a name="rule-date-format"></a>
#### date_format:_format_

အဆုိပါ field မွ format ႏွင့္ တူညီရမည္ ျဖစ္ျပီး `date_parse_from_format` ဟူသည္ PHP function ကုိ အသုံးျပဳထားသည္။

<a name="rule-different"></a>
#### different:_field_

အဆုိပါ field သည္ အျခား ရည္ညြန္း field ႏွင့္ လုံးဝ ကြဲျပားျခားရမည္ ျဖစ္သည္။
The given _field_ must be different than the field under validation.

<a name="rule-digits"></a>
#### digits:_value_

အဆုိပါ file တြင္ numeric value ျဖစ္ျပီး တိက်ေသခ်ာေသာ ဂဏန္း အလုံးအေရအတြက္ ကုိသာ ထည့္သြင္းရမည္ျဖစ္သည္။

<a name="rule-digits-between"></a>
#### digits_between:_min_,_max_

အဆုိ field တြင္ _min_ and _max_ အၾကား ထည့္သြင္းရေသာ ဂဏန္းအလုံးအေရအတြက္ကုိသာ ထည့္သြင္းခြင့္ရမည္ျဖစ္သည္။

<a name="rule-email"></a>
#### email

အဆုိပါ field တြင္ email address format အတုိင္း ထည့္သြင္းထားျခင္း ရွိမရွိ စစ္ေဆးသြားမည္ ျဖစ္သည္။

<a name="rule-exists"></a>
#### exists:_table_,_column_

The field under validation must exist on a given database table.

#### Basic Usage Of Exists Rule

	'state' => 'exists:states'

#### Specifying A Custom Column Name

	'state' => 'exists:states,abbreviation'

You may also specify more conditions that will be added as "where" clauses to the query:

	'email' => 'exists:staff,email,account_id,1'

Passing `NULL` as a "where" clause value will add a check for a `NULL` database value:

	'email' => 'exists:staff,email,deleted_at,NULL'

<a name="rule-image"></a>
#### image

The file under validation must be an image (jpeg, png, bmp, or gif)

<a name="rule-in"></a>
#### in:_foo_,_bar_,...

The field under validation must be included in the given list of values.

<a name="rule-integer"></a>
#### integer

The field under validation must have an integer value.

<a name="rule-ip"></a>
#### ip

The field under validation must be formatted as an IP address.

<a name="rule-max"></a>
#### max:_value_

The field under validation must be less than or equal to a maximum _value_. Strings, numerics, and files are evaluated in the same fashion as the `size` rule.

<a name="rule-mimes"></a>
#### mimes:_foo_,_bar_,...

The file under validation must have a MIME type corresponding to one of the listed extensions.

#### Basic Usage Of MIME Rule

	'photo' => 'mimes:jpeg,bmp,png'

<a name="rule-min"></a>
#### min:_value_

The field under validation must have a minimum _value_. Strings, numerics, and files are evaluated in the same fashion as the `size` rule.

<a name="rule-not-in"></a>
#### not_in:_foo_,_bar_,...

The field under validation must not be included in the given list of values.

<a name="rule-numeric"></a>
#### numeric

The field under validation must have a numeric value.

<a name="rule-regex"></a>
#### regex:_pattern_

The field under validation must match the given regular expression.

**Note:** When using the `regex` pattern, it may be necessary to specify rules in an array instead of using pipe delimiters, especially if the regular expression contains a pipe character.

<a name="rule-required"></a>
#### required

The field under validation must be present in the input data.

<a name="rule-required-if"></a>
#### required\_if:_field_,_value_

The field under validation must be present if the _field_ field is equal to _value_.

<a name="rule-required-with"></a>
#### required_with:_foo_,_bar_,...

The field under validation must be present _only if_ any of the other specified fields are present.

<a name="rule-required-with-all"></a>
#### required_with_all:_foo_,_bar_,...

The field under validation must be present _only if_ all of the other specified fields are present.

<a name="rule-required-without"></a>
#### required_without:_foo_,_bar_,...

The field under validation must be present _only when_ any of the other specified fields are not present.

<a name="rule-required-without-all"></a>
#### required_without_all:_foo_,_bar_,...

The field under validation must be present _only when_ the all of the other specified fields are not present.

<a name="rule-same"></a>
#### same:_field_

The given _field_ must match the field under validation.

<a name="rule-size"></a>
#### size:_value_

The field under validation must have a size matching the given _value_. For string data, _value_ corresponds to the number of characters. For numeric data, _value_ corresponds to a given integer value. For files, _size_ corresponds to the file size in kilobytes.

<a name="rule-unique"></a>
#### unique:_table_,_column_,_except_,_idColumn_

The field under validation must be unique on a given database table. If the `column` option is not specified, the field name will be used.

#### Basic Usage Of Unique Rule

	'email' => 'unique:users'

#### Specifying A Custom Column Name

	'email' => 'unique:users,email_address'

#### Forcing A Unique Rule To Ignore A Given ID

	'email' => 'unique:users,email_address,10'

#### Adding Additional Where Clauses

You may also specify more conditions that will be added as "where" clauses to the query:

	'email' => 'unique:users,email_address,NULL,id,account_id,1'

In the rule above, only rows with an `account_id` of `1` would be included in the unique check.

<a name="rule-url"></a>
#### url

The field under validation must be formatted as an URL.

> **Note:** This function uses PHP's `filter_var` method.

<a name="conditionally-adding-rules"></a>
## Conditionally Adding Rules

In some situations, you may wish to run validation checks against a field **only** if that field is present in the input array. To quickly accomplish this, add the `sometimes` rule to your rule list:

	$v = Validator::make($data, array(
		'email' => 'sometimes|required|email',
	));

In the example above, the `email` field will only be validated if it is present in the `$data` array.

#### Complex Conditional Validation

Sometimes you may wish to require a given field only if another field has a greater value than 100. Or you may need two fields to have a given value only when another field is present. Adding these validation rules doesn't have to be a pain. First, create a `Validator` instance with your _static rules_ that never change:

	$v = Validator::make($data, array(
		'email' => 'required|email',
		'games' => 'required|numeric',
	));

Let's assume our web application is for game collectors. If a game collector registers with our application and they own more than 100 games, we want them to explain why they own so many games. For example, perhaps they run a game re-sell shop, or maybe they just enjoy collecting. To conditionally add this requirement, we can use the `sometimes` method on the `Validator` instance.

	$v->sometimes('reason', 'required|max:500', function($input)
	{
		return $input->games >= 100;
	});

The first argument passed to the `sometimes` method is the name of the field we are conditionally validating. The second argument is the rules we want to add. If the `Closure` passed as the third argument returns `true`, the rules will be added. This method makes it a breeze to build complex conditional validations. You may even add conditional validations for several fields at once:

	$v->sometimes(array('reason', 'cost'), 'required', function($input)
	{
		return $input->games >= 100;
	});

> **Note:** The `$input` parameter passed to your `Closure` will be an instance of `Illuminate\Support\Fluent` and may be used as an object to access your input and files.

<a name="custom-error-messages"></a>
## Custom Error Messages

If needed, you may use custom error messages for validation instead of the defaults. There are several ways to specify custom messages.

#### Passing Custom Messages Into Validator

	$messages = array(
		'required' => 'The :attribute field is required.',
	);

	$validator = Validator::make($input, $rules, $messages);

> *Note:* The `:attribute` place-holder will be replaced by the actual name of the field under validation. You may also utilize other place-holders in validation messages.

#### Other Validation Place-Holders

	$messages = array(
		'same'    => 'The :attribute and :other must match.',
		'size'    => 'The :attribute must be exactly :size.',
		'between' => 'The :attribute must be between :min - :max.',
		'in'      => 'The :attribute must be one of the following types: :values',
	);

#### Specifying A Custom Message For A Given Attribute

Sometimes you may wish to specify a custom error messages only for a specific field:

	$messages = array(
		'email.required' => 'We need to know your e-mail address!',
	);

<a name="localization"></a>
#### Specifying Custom Messages In Language Files

In some cases, you may wish to specify your custom messages in a language file instead of passing them directly to the `Validator`. To do so, add your messages to `custom` array in the `app/lang/xx/validation.php` language file.

	'custom' => array(
		'email' => array(
			'required' => 'We need to know your e-mail address!',
		),
	),

<a name="custom-validation-rules"></a>
## Custom Validation Rules

#### Registering A Custom Validation Rule

Laravel provides a variety of helpful validation rules; however, you may wish to specify some of your own. One method of registering custom validation rules is using the `Validator::extend` method:

	Validator::extend('foo', function($attribute, $value, $parameters)
	{
		return $value == 'foo';
	});

The custom validator Closure receives three arguments: the name of the `$attribute` being validated, the `$value` of the attribute, and an array of `$parameters` passed to the rule.

You may also pass a class and method to the `extend` method instead of a Closure:

	Validator::extend('foo', 'FooValidator@validate');

Note that you will also need to define an error message for your custom rules. You can do so either using an inline custom message array or by adding an entry in the validation language file.

#### Extending The Validator Class

Instead of using Closure callbacks to extend the Validator, you may also extend the Validator class itself. To do so, write a Validator class that extends `Illuminate\Validation\Validator`. You may add validation methods to the class by prefixing them with `validate`:

	<?php

	class CustomValidator extends Illuminate\Validation\Validator {

		public function validateFoo($attribute, $value, $parameters)
		{
			return $value == 'foo';
		}

	}

#### Registering A Custom Validator Resolver

Next, you need to register your custom Validator extension:

	Validator::resolver(function($translator, $data, $rules, $messages)
	{
		return new CustomValidator($translator, $data, $rules, $messages);
	});

When creating a custom validation rule, you may sometimes need to define custom place-holder replacements for error messages. You may do so by creating a custom Validator as described above, and adding a `replaceXXX` function to the validator.

	protected function replaceFoo($message, $attribute, $rule, $parameters)
	{
		return str_replace(':foo', $parameters[0], $message);
	}

If you would like to add a custom message "replacer" without extending the `Validator` class, you may use the `Validator::replacer` method:

	Validator::replacer('rule', function($message, $attribute, $rule, $parameters)
	{
		//
	});
