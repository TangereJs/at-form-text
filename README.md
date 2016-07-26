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
