# at-form-text

### null value handling

`at-form-text` value can be set to null via attribute and via property.

When value is set to null via attribute polymer-js converts `null` to `"null"`, because `at-form-text.value` is specified to be a `string` and attribute to property deserialization converts attribute value to property value using specified property type.

When value is set to null via property polymer-js doesn't do conversion so at-form-text.valueChanged handler gets null value as javascript null object.

#### 1. `at-form-text.value = null`

1. `null` here is javscript null object.

When `null` is assigned
 * `at-form-text.value` should remain `null`,
 * `at-form-text` text input value should become `""`, and
 * `at-form-text.valid` should be true; `null` is valid value for `at-form-text`


#### 2. `at-form-text.value = null` and `at-form-text.required = true`

1. `null` here is javscript null object.

When `null` is assigned and `at-form-text.required = true`
 * `at-form-text.value` should remain `null`,
 * `at-form-text` text input value should become `""`, and
 * `at-form-text.valid` should be `false`; when `required` is `true` `null` is invalid value for `at-form-text`

#### 3. `at-form-text.value = null` and `at-form-text.maxChars = n`

1. `null` here is javscript null object.
2. `n` is integer greater than 0

When `null` is assigned and `at-form-text.maxChars > 0`
 * `at-form-text.value` should remain `null`,
 * `at-form-text text` input value should become `""`, and
 * `at-form-text.valid` should be `true`; `null` is valid value for `at-form-text` and has no length so `maxChars` validation function should return `true`

#### 4. `at-form-text.value = "null"`

1. `null` here is `"null"` string value.

When `"null"` is assigned
* `at-form-text.value` should remain `"null"`,
* `at-form-text` text input value should become `"null"`, and
* `at-form-text.valid` should be true; `"null"` is a valid string as any other for `at-form-text`


#### 5. `at-form-text.value = "null"` and `at-form-text.required = true`

1. `null` here is `"null"` string value.

When `"null"` is assigned and `at-form-text.required = true`
* `at-form-text.value` should remain `"null"`,
* `at-form-text` text input value should become `"null"`, and
* `at-form-text.valid` should be true; `"null"` is a valid string as any other for `at-form-text`

#### 6. `at-form-text.value = null` and `at-form-text.maxChars = n`

1. `null` here is `"null"` string value.
2. `n` is integer greater than 0

When `null` is assigned and `at-form-text.maxChars > 0`
* `at-form-text.value` should remain `"null"`,
* `at-form-text` text input value should become `"null"`, and
* `at-form-text.valid` should be `true` if `maxChars > 4` and `false` otherwise; that is standard `maxCharsValidation` applies as for any other string value

### Polymer 2 validation handling

`at-form-text` can be made invalid as follows
1. by setting `errorMessage` property to `non-empty string`
2. by setting `required` to `true` and value to `empty string`
3. by setting `maxChars` to `non-zero positive value` and `value` to `string` that has more than `maxChars` characters

`at-form-text` can be instructed to display errors in the UI in two ways
1. by settings `autoValidate` property
1. by calling `validate` function with `showError` value of `true`


#### Case 1. `at-form-text` is made invalid by setting `errorMessage`

It is a design decision to always show errors in UI when `errorMessage` property is set.

When `at-form-text` is made invalid by setting `errorMessage` calling `validate` function should return `false`.

After attaching the element to the DOM, UI should show invalid state per following table

| autoValidate | showError | validate function result | show errors in UI |
| --- | --- | --- | --- |
| false | false | false | true |
| false | true | false | true |
| true | false | false | true |
| true | true | false | true |


#### Case 2. `at-form-text` is made invalid by setting `required` and `value`

It is a design decision to show errors in UI when `required` property is set when `autoValidate` is `true` or `validate` is called with `showError` value of `true`.

When `at-form-text` is made invalid by setting `required` to `true` and `value` to `empty string` calling `validate` function should return `false`.

After attaching the element to the DOM, UI should show invalid state per following table

| autoValidate | showError | validate function result | show errors in UI |
| --- | --- | --- | --- |
| false | false | false | false |
| false | true | false | true |
| true | false | false | false |
| true | true | false | true |

#### Case 3. `at-form-text` is made invalid by setting `maxChars` and `value`

It is a design decision to show errors in UI when `maxChars` property is set when `autoValidate` is `true` or `validate` is called with `showError` value of `true`.

When `at-form-text` is made invalid by setting `maxChars` to `non-zero positive value` and `value` to `string` that has more than `maxChars` characters calling `validate` function should return `false`.

After attaching the element to the DOM, UI should show invalid state per following table

| autoValidate | showError | validate function result | show errors in UI |
| --- | --- | --- | --- |
| false | false | false | false |
| false | true | false | true |
| true | false | false | false |
| true | true | false | true |
