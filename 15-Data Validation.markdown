#Data Validation

Although ChocolateChip-UI does not provide any type of data validation, it's not that hard to roll your own. If you're using ChocolateChip-UI with jQuery, you might even prefer to use a jQuery plugin for form validation.

If you're gathering information from your users in your app and the data is input by the users, you will probably want to have some type of validation to ensure that you are receiving the type of data you need. This can also help protect you from cross-site scripting attacks, etc. 

In general, mobile apps have limited needs for data entry, and this is a good thing. Users don't like having to fill out long, complex forms on a tiny mobile screen, nor do they have the patience. As such we're going to keep the following examples simple. If you have complex data validation needs, then you should look at incorporating an industrial strength data validation library and make sure that your server side code is sufficient to handle your requirements.

For these examples we are going to use plain ole JavaScript, no jQuery or similar libraries. That means that when you see `input`, that is the actual input element, not a jQuery-like object represeting the input. As such, if you want to use these examples with jQuery, you need to convert your jQuery elements into DOM nodes. This is really easy. Just append `[0]` after the jQuery element when you pass it as an argument:

```
var input = $('input[type=text]');
// Pass in the actual node for validation:
validateMyInput(input[0]);
```


###Validate Empty

The first thing you would want to validate is whether the user has entered a value or not. This is very easy to check. Just examine the value of the input. If the user failed to enter anything in the input, it will have no value.


```
var isNotEmpty = function(input) {
  return input && input.value;
};
```

###Validate Alphabetic

Sometimes you might want to make sure that whatever the user has provided is plain text, no numbers included. We can test if a string is purely alphabetic with a simple regular expression:

```
var validateAlphabetic = function(input) {
  var letters = /^[A-Za-z]+$/;
  var value = input.value;
  return value.match(letters);
};
```

###Validate Number

Sometimes you may request a number from users. You can check that they did so without accidentally including letters with the following simple regular expression:

```
var validateNumber = function(input) {
  var numbers = /^[0-9]+$/; 
  var value = input.value;
  return value.match(numbers);
};
```

###Validate Alpha-Numeric

There may be times when you want to enable the user to enter any mix of letters and numbers. We can check for this by combining the regular expressions for letters and numbers:

```
var validateAlphaNumeric = function(input) {
  var letters = /^[0-9a-zA-Z]+$/; 
  var value = input.value;
  return value.match(letters);
};
```

###Validate Username

Allowing the user to create a username is a little complicated. You want to allow them to enter any combination of letters and numbers, but you don't want them to just enter numbers. And you might want to limit usernames to a certain minimum length for security reasons. With these in mind, we came up with this regular expression and the ability to check for a minimum username length:

```
var validateUserName = function(input, minimum) {
  var username = input.value;
  if (!isNaN(username)) {
    return false;
  } else {
    return username.length >= minimum;
  }
};
```

###Validate Password

Passwords, that's a big one. In this example we're only going to look at how to test that a password consists of any possible mix of letters, number and punctuation marks, and that the inital password and its second entry are identical. We have a regular expression to check that the password does indeed contain at least a mix of letters and numbers. If the user only enters letter or numbers, this will invalidate. Of course, it expects two different inputs to compare their values. 

```
var validatePassword = function(input1, input2) {
  var letters = /^(?=.*[a-zA-Z])(?=.*[0-9]).+$/;
  if (!letters.test(input1.value) && !letters.test(input2.value)) return false;
  return input1.value === input2.value;
};
```

###Validate Email

We often need to get a user's email address. We test the entered data to see if it follows this format:

```
aa@aa.aaa
```

Here's the regular expression to do it:

```
var validateEmail = function(input) {
  var value = input.value;
  var email = /^\w+([\.-]?\w+)*@\w+([\.-]?\w+)*(\.\w{2,3})+$/;
  return value.match(email);
};
```

###Validate Phone Number

Validating phone numbers can be tricking. The first problem is that users don't always use group delimiters consistently. Secondly, there are a number of different phone number formats in use around the world, sometimes even in the same country. For this example we're going to present a regular expression that works for North American phone numbers. This covers both Canada and the US.

We want to be able to match any of the following entries for a phone number:

- 1-800-123-1234
- (123) 456-7890
- 123-456-7890
- 123.456.7890
- 1234567890

The following regular expression can do that for us:

```
var validatePhoneNumber = function(input) {
  var phoneNumber = input.value.replace(/[\(\)\.\-\ ]/g, '');  
  var phonePattern = /^1?[(]{0,1}[0-9]{3}[)]{0,1}[-\s\.]{0,1}[0-9]{3}[-\s\.]{0,1}[0-9]{4}$/
  return phoneNumber.match(phonePattern);
};
```

###Validate URL

Sometimes we may want to enable the user to enter a url for something. Generally, outside of the browser, urls will not work unless they have the protocal included: http, https, ftp, etc. When the user enters something like `www.someplace.com` in the address bar of a browser, the browser guesses at the correct protocal for that url. Browsers are very good at this. In other situations such truncated urls may not work. The following regular expression forces the user to provide a proper url with an appropriate protocal.

```
var validateUrl = function(input) {
  var url = /^(ftp|http|https):\/\/(\w+:{0,1}\w*@)?(\S+)(:[0-9]+)?(\/|\/([\w#!:.?+=&%@!\-\/]))?$/;
  return input.value.match(url);
};
```

###Validate Age

Depending on the nature of your app, you may want to verify that the user is of a minimum age. This is easy to do, we just check the value the user entered and check it against our expected minumum age:

```
var validateAge = function(input, minimum) {
  var age = input.value;
  return age >= minimum;
};
```

###Checkboxes

To validate whether the user selected a checkbox or not you need to examine it's `checked` property:


```
var validateCheckbox = function(input) {
  return input.checked === true;
};
```

###Radio Buttons

Like checkboxes, you verify whether a radio button was selected by examining its `checked` property. Since radio buttons are organized in groups, you need to get the relevant group of buttons and loop over them to see if any has been selected:

```
var radioButtons = [].slice.apply(document.querySelectorAll('input[type=radio]'));
var validateRadioButtons = function(radioButtons) {
  var choice = false;
  radioButtons.forEach(function(button) {
    if (button.checked === true) {
      choice = true;
    }
  });
  return choice;
}; 
```

###Select Box

Determining if a select box has a selection is easy, just check for its `selectedIndex` value. 


```
var validateSelectBox = function(selectbox) {
  return selectbox.selectedIndex;
};
```

If you want to have the first option as a label and not trigger a selection if the user chooses it, just set its value to empty:

```
<select name="selectbox" id="selectbox">
  <option value="">Select a fruit</option>
  <option value='Apple'>Apple</option>
  <option value='Orange'>Orange</option>
  <option value='Banana'>Banana</option>
</select>
```

In the above markup, the first option, "Select a fruit", will show when the select box is rendered. If the user selects it, because its value is empty, it will not result in a valid `selectedIndex` value.