# at-form-elements validation API

```javascript
validate: function () {
  /*
    This is validation flow only for non container elements; this doesn't work for at-core-form, at-form-complex and at-form-array
  */

  /* (@ij on 28.07.2016 written for AT-162)
    update at-form-text.value for at-core-form-enter.keypress usecase
    consider following scenario
    1. at-form-text is child of at-core-form
    2. at-core-form listens to keypress event on its children with intent to validate itself when ENTER is pressed
    3. at-form-text.$.input has focus
    4. user types characters into input
    5. user presses ENTER
    6. at-core-form.keypress event handler is executed and ENTER is detected
    7. at-core-form.validate is executed; this executes validate on each child
    8. at-form-text.validate is executed
    9. at this point at-form-text.$.input change event didn't trigger and at-form-text.$.input blur event didn't trigger
    10. so at-form-text.value is not updated with the value from at-form-text.$.input
    11. this needs to be done before validation occurs for at least two reasons
    11.R1 its expected that at-form-text.validate operates on latest user input
    11.R2 its expected that at-core-form.data is already populated with lastest values from child elements when at-core-form.validate is called
    12. since at-form-text.$.input change/focus events didn't trigger and ENTER is pressed we detect that at-form-text.value needs to be updated by comparing
        at-form-text.value with at-form-text.$.input.value
    13. if they are different at-form-text.value should be updated with at-form-text.$.input.value

    14. now there is a special case where at-form.text.value is set to null;
    15. by setting at-form.text.value to null at-form-text.$.input.value is set to empty string
    16. if at-form-text.validate is called and at-form-text.value === null and at-form-text.$.input.value === "", at-form-text.value should not be updated
        because validation should operate on at-form-text.value === null. at-form-text.$.input.value is empty string because that should be displayed to the user
    17. if at-form.text.value was set to null but then user types into at-form.text.$.input and then presses ENTER then at-form-text.value should be updated
  */
  /*
    TAG P1 (P1 as in  part 1)

    @include into behavior  {
      No. This behavior is specific to single line input elements at-form- [ text, password, number, lookup and it should work for date]
      other elements either do not need this (checkbox) or
      it should work differently (file, image) or
      it can't happen because elements handle ENTER already (codemirror, markdown, textarea)
    }
  */
  var inputValue = this.$.input.value;
  if (this.value !== null && inputValue !== "" && inputValue !== this.value) {
    this.value = inputValue;
  }

  // validation functions from behaviors will return validation result objects
  var validationResult = {
    isValid: true, // isValid is true if constraints checked in validation function are satisfied; false otherwise
    errorMessage: "" // errorMessage is empty string if isValid is true; errorMessage contains error message text is isValid is false
  };
  /*
    1. check disabled, hide and errorMessage first
    if any of these are set validating anything else is meaningless
   */
   /*
     TAG P2

     @include into behavior  {
       Yes. Put this into Tangere.behaviors.formUIGeneric
       because validation requirements for disabled, hide and errorMessage are the same for all elements
     }
   */
  validationResult = this._validateDisabledHideAndErrorMessage();
  if (!validationResult.isValid) {
    this._displayErrorTextInHint(validationResult.errorMessage);
    this._updateUIValidationState(false);
    this._setValid(false); // passing validationResult.isValid is meaningless because we already know the value is false
    return false; // returning this.valid or validationResult.isValid is meaningless because we already know the value is false
  }

  /*
    2. check required
    if required check fails validating anything else is meaningless
  */
  /*
    TAG P3

    @include into behavior  {
      Yes. Put this into Tangere.behaviors.formUIGeneric
      by default return value !== ""; this works for at-form-[ codemirror, date, daterange, lookup, markdown, number, password, radio, state, text and textarea ]
      override in at-form-checkbox to always return true
      override in at-form-file and at-from-image as is already implemented (see at-form-file Line 356 and at-form-image Line 450)
    }
  */
  validationResult = this._validateRequired();
  if (!validationResult.isValid) {
    this._displayErrorTextInHint(validationResult.errorMessage);
    this._updateUIValidationState(false);
    this._setValid(false); // passing validationResult.isValid is meaningless because we already know the value is false
    return false; // returning this.valid or validationResult.isValid is meaningless because we already know the value is false
  }

  /*
    3. check element specific constraints
    @behavior candidate: poor
    by default return true for all elements
    each element knows about its constraints, so each elements has to implement this itself and return true/false accordingly
  */
  /*
    TAG P4

    @include into behavior  {
      Yes. Put this into Tangere.behaviors.<atFormElementName>InputValidation
      each element knows about its constraints, so each elements has to implement this itself and return true/false accordingly
    }
  */
  validationResult = Tangere.behaivors.atFormTextInputValidation.validateData(this.properties, this.value);
  if (!validationResult.isValid) {
    this._displayErrorTextInHint(validationResult.errorMessage);
    this._updateUIValidationState(false);
    this._setValid(false); // passing validationResult.isValid is meaningless because we already know the value is false
    return false; // returning this.valid or validationResult.isValid is meaningless because we already know the value is false
  }

  /*
    4. validation passed, element is valid
  */
  /*
    TAG P5
  */
  this._displayErrorTextInHint("");
  this._updateUIValidationState(true);
  this._setValid(true); // returning this.valid or validationResult.isValid is meaningless because we already know the value is true
  return true; // returning this.valid or validationResult.isValid is meaningless because we already know the value is true

  /*
    TAG P6

    this._setValid(false); // passing validationResult.isValid is meaningless because we already know the value is false
    this._displayErrorTextInHint(validationResult.errorMessage);

    @include into behavior  {
      Yes. Put this into Tangere.behaviors.formUIGeneric
      Because formUIGeneric will contain valid property and
      every element has $.hint so it can be handled in the same way
    }
   */

  /*
    TAG P7

    this._updateUIValidationState(true);

    @include into behavior  {
      No. _updateUIValidationState must be present in each element
      Because html structure of every element is different. Css error classes can not be set in every element in the same way.
    }

  */

  // var validationBehavior = this.behaviors[1];
  // validationBehavior.validate.call(this, this.value);
},
```
