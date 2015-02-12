#Data Converters

Data converters are convenience methods that allow you to manipulate data in a human-meaningful way before presenting it to a user. Angularjs offers a convenient way of formatting common data types: filters. By pipping your data into a filter, Angularjs will automatically format it as needed. ChocolateChip-UI does not provide any type of data converters. However, depending on what you are using to build your app, your library or framework may provide such features. If this is not the case, create data convertes is easier than it sounds. In this blog post we show how to make provide some common data formatting needs.


##Formatting Numbers

When dealing with numbers, the first thing you should be aware is that although number primitives are nubmers, you can not always execute number methods on them, however passing a number primitive to the Number method will allow you to do so:

```
123.toFixed(2);
// Will generate an error.

Number(123).toFixed(2);
// Returns 123.00
```

As shown in the above code, adding decimal points to a number is quite easy by using the `toFixed` method. You execute it on a Number object with the number of digits you want. If the number of digits you provide for `toFixed` are less than the number of digits in the number, it will be rounded down to the number of digits you've idicated:

```
Number(123.12345).toFixed(2);
// Returns 123.12

Number(123.129).toFixed(2);
// Returns 123.13
```

Because `toFixed` always returns the average of decimal values, you should not use it on any number that you intend to use for calculations but instead wait until you are ready to display the results. Also, be aware that `toFixed` returns the value as a string. So if you wanted to do math operations, you'd have to convert it back to a number. The `toFixed` method will display the correct decimal indicator for the locale of the OS. 

Having decimals is nice, but with large numbers it helps to have thousands separators as well. We can create a simple function to do this for us. We want to make this function as flexible as possible. We of course need to pass in the numerical amount we want to format, but we also want to be able to indicate the thousands separator and if we want automatic decimal places. Although in English-speaking countries we use commas to separate thousands, other countries may use other characters. `toFixed` will use the decimal indicator appropriate for the locale. Here's the shell for our function.  For now, we'll check if the user provided a separator value. If none was provided, we'll use the default of ',':


```
var formatNumber = function(amount, separator, decimal) {
  var sep = separator || ",";
};
```

To deal with the number, we first need to make sure that it is a number and not a string. If we pass the value to the `Number()` function first, we'll always be dealing with a number object.

```
$.extend({
  formatNumber : function(amount, separator, decimal) {
    var sep = separator || ",";
    return Number(amount);
  }
});
```

Next we need to parse the number and determine if the number needs thousands separators. 


```
$.extend({
  formatNumber : function(amount, separator, decimal) {
    var sep = separator || ",";
    return Number(amount).toString().replace(/(?=(?:\d{3})+$)(?!^)/g, sep);
  }
});
```

At the moment, this is a little fragile, we aren't actually dealing with a decimal value. It will just return the number as is. So we need to check if a decimal was not provided:


```
$.extend({
  formatNumber : function(amount, separator, decimal) {
    var sep = separator || ",";
    if (decimal === undefined) {
      return Number(amount).toString().replace(/(?=(?:\d{3})+$)(?!^)/g, sep);
    }
  }
});
```

The above regular expression will work fine for whole numbers, but we need a slightly different regular expression to handle decimal places. Here's an improved version where we check if the amount is an integer or a float:

```
$.extend({
  formatNumber : function(amount, separator, decimal) {
    var sep = separator || ",";
    if (decimal === undefined) {
      // Check if amount is a float:
      if (typeof amount === 'number' && amount % 1 !== 0) {
        return Number(amount).toString().replace(/\d(?=(\d{3})+\.)/g, '$&' + sep);
      // Otherwise treat it as an integer:
      } else {
        return Number(amount).toString().replace(/(?=(?:\d{3})+$)(?!^)/g, sep);
      }
    }
  }
});
```

Next, if the user provides a decimal value, we need to accomodate that in the output by using `toFixed`:

```
$.extend({
  formatNumber : function(amount, separator, decimal) {
    var sep = separator || ",";
    if (decimal === undefined) {
      // Check if amount is a float:
      if (typeof amount === 'number' && amount % 1 !== 0) {
        return Number(amount).toString().replace(/\d(?=(\d{3})+\.)/g, '$&' + sep);
      // Otherwise treat it as an integer:
      } else {
        return Number(amount).toString().replace(/(?=(?:\d{3})+$)(?!^)/g, sep);
      }
    // If a decimal value was provided,
    // format it to that amount:
    } else {
      return Number(amount).toFixed(decimal).replace(/\d(?=(\d{3})+\.)/g, '$&' + sep);
    }
  }
});
```

We want to add one last feature to this method. We want to be able to round a float to a whole number:

```
$.extend({
  formatNumber : function(amount, separator, decimal) {
    var sep = separator || ",";
    // Allow the user to round a float to a whole number:
    if (decimal === 0) {
      alert('rounding');
      var num = Math.round(amount);
      return Number(num).toString().replace(/(?=(?:\d{3})+$)(?!^)/g, sep);
    }
    if (decimal === undefined) {
      // Check if amount is a float:
      if (typeof amount === 'number' && amount % 1 !== 0) {
        return Number(amount).toString().replace(/\d(?=(\d{3})+\.)/g, '$&' + sep);
      // Otherwise treat it as an integer:
      } else {
        return Number(amount).toString().replace(/(?=(?:\d{3})+$)(?!^)/g, sep);
      }
    // If a decimal value was provided,
    // format it to that amount:
    } else {
      return Number(amount).toFixed(decimal).replace(/\d(?=(\d{3})+\.)/g, '$&' + sep);
    }
  }
});
```

Now we can do the following:

```
$.formatNumber(9123456789)
// Returns: 9,123,456,789

$.formatNumber(9123456789.2382)
// Returns: 9,123,456,789.2382

// Round it off to two decimal places:
$.formatNumber(9123456789.2382, ',', 2)
// Returns: 9,123,456,789.24


// Add decimal places to a whole number:
$.formatNumber(9123456789, ',', 2)
// Returns: 9,123,456,789.00
```

With our number formatter we can add thousands separators, we can round down the decimal value, or we can add decimal places. However, if you want to get rid of the decimal values, rounding it down to whole numbers, just pass in a decimal value of `0`:

```
$.formatNumber(9123456789.9382, ',', 0);
Returns: 9123456790
```

##Sum of Numbers

If you have a list of values, you may wish to show a sum of all the values. Fortunately, JavaScript has an easy way of getting the sum of a series of numbers. We can use the ECMAScript 5 `Array.reduce` method. Just turn the serious of numbers as an array an execute the reduce method on it:

```
[1,2,3].reduce(function(a, b) {
  return a + b;
});
// Returns: 6
```

We can turn this into a reusable function:

```
$.extend({
  sum: function(arr) {
    return arr.reduce(function(a, b) {
      return a + b;
    });
  }
});

var nums = [1, 2, 3, 4, 5, 6, 7, 8, 9];
var total = $.sum(nums);
// Returns: 45

$.sum([1, 2, 3, 4, 5, 6, 7, 8, 9, 10.12])
// Returns: 55.12
```

##Currency

This is a big one. Currency is much more complicated than just numbers. Using what we learned from formatting numbers, we can create a data converter for currency. One thing about math operations and currency. If you want accuracy for your final currency amount, do not round off your decimal values until the end. Like our number formatter, we need to deal with decimal places and thousands separators. But we also need to allow for different currency marks.

```
var currency = function(amount, symbol, separator, decimal) {
  var sym = symbol || "$";
  var sep = separator || ",";
  var dec = decimal || 2;
};
```

We also want to be able to check if the user passed a decimal value of `0` because we will then need to round the amount to whole numbers:


```
$.extend({
  currency: function(amount, symbol, separator, decimal) {
    var sym = symbol || "$";
    var sep = separator || ",";
    var dec = decimal || 2;
    var zero = false;
    if (decimal === 0) {
      zero = true;
    }
  }
});
```

We can reuse the regular expressions from our number formatter to user he:


```
$.extend({
  currency: function(amount, symbol, separator, decimal) {
    var sym = symbol || "$";
    var sep = separator || ",";
    var dec = decimal || 2;
    var zero = false;
    if (decimal === 0) {
      zero = true;
    }
    // Private function to format amounts:
    var formatNumber = function(amount, sep) {
      // A decimal value of '0' means
      // we need to round the amount off
      // before adding in thousands separators:
      if (zero) {
        var num = Math.round(amount);
        return Number(num).toString().replace(/(?=(?:\d{3})+$)(?!^)/g, sep);
      } else {
        // Otherwise, we can just add the thousands
        // separators with the decimal placement
        // provided by the user or the default:
        return Number(amount).toFixed(dec).replace(/\d(?=(\d{3})+\.)/g, '$&' + sep);
      }
    };
    return sym + formatNumber(amount, sep);
  }
});
```

With our currency formatter we can do the following:

```
// Format dollars with two decimal points:
$.currency(9123456789.2382, '$')
// Returns: $9,123,456,789.24

// Round off decimal values to dollars:
$.currency(9123456789.99, '$', ',', 0)
// Returns: $9,123,456,790

```

##Dates and Time

JavaScript has a date object for dealing with both dates and times. The value returned by the date object can look a little odd:

```
new Date();
// Returns something like, depending on your locale:
// Wed Sep 17 2014 15:19:23 GMT-0700 (PDT)
```

To get a usable date value we can just use the native JavaScript method: `toLocaleDateString`. To create a human-readable date, we can do this:

```
var date = new Date();
var currentDate = date.toLocaleDateString()
console.log(currentDate);
// Might return something like this:
// September 17, 2014
```

As you can see from the above example, its very easy to get a standard, human-friendly date from the JavaScript date object. And the results will be appropriate for the user's locale.

Getting the time format can be a little tricky. 

```
var date = new Date();
var time = date.toLocaleTimeString();
// time equal: 3:23:28 PM PDT
```

The result of `toLocaleTimeString` is not as useful as `toLocaleDateString`. But we can create a simple function to make this better. We want to strip out the timezone and the second and just leave the hours, minutes and AM/PM value:

```
$.extend({
  formatTime: function(time) {
    var temp = time.split(':');
    var temp2 = temp[0] + ':' + temp[1];
    var ampm = time.split(' ')[1];
    return temp2 + ' ' + ampm;
  }
});
```

With this we can do the following:

```
var date = new Date();
$.formatTime(date.toLocaleTimeString())
// Would return something like: 3:30 PM
```

##Conclusion

We hope this was informative about how to accompish basic data converters. If you have more sophisticated needs for large amounts of data, you might want to think about using a framework, like Angularjs, Ember, etc., that have these backed in. Or you might look into using some very advanced libraries that support every imaginable feature, such as [numeral.js](http://numeraljs.com), [moment.js](http://momentjs.com), [date.js](http://www.datejs.com).

However, if size is a big concern and you want your app to be mean and lean, then think about if you can get by with the simple data converters we've presented here. Feel free to hack them to suit your needs.

Although data converters currently require custom code to implement, there is a Web standard that is shipping in some browsers as part of ECMAScript 6: `Number.toLocalseString` and `Date.toLocaleString`. These methods will allow you to pass in options to automatically format numbers, currency, dates and time for a locale without the need of extra code. If you know your target platform definitely supports these two methods already, then do use them. They will save you a lot of trouble. Otherwise, practically speaking, it will be several years before the current crop of mobile OSes have all been either replaced or upgraded to a browser that supports these.