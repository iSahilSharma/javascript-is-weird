# JavaScript is weird.
> I created this repository to document my dumbfound close encounters with JavaScript over the years. When I started learning JavaScript, the first thing I wrote was:
>
>   `console.log(alert(1));`
>
>I thought there wouldn't be any output on the console and only the alert popup will appear. However, the output was logged as `undefined` on the console along with the alert message.

## Close Encounters:
 #### `Array.prototype.sort()`
 
According to MDN, the `sort()` method sorts the elements of an array in place and returns the sorted array. The default sort order is ascending, built upon converting the elements into strings, then comparing their sequences of UTF-16 code units values.
The time and space complexity of the sort cannot be guaranteed as it depends on the implementation. 

Let's consider an example:

    const months = ['March', 'Jan', 'Feb', 'Dec'];
    months.sort();
    console.log(months); 
    
    Output: Array ["Dec", "Feb", "Jan", "March"]`

Seems working, right? Wait until you run the following code:

    const numbers = [1, 30, 4, 21, 100000];
    numbers.sort();
    console.log(numbers); 
 
 #### What do you thing will be the output of numbers?   
 As per the normal understanding, the strings should be sorted alphabetically and numbers numerically. Hence, we expect the output to be:
 
      Output: Array [1, 4, 21, 30, 100000]

However, the output comes to be:
    
      Output: Array [1, 100000, 21, 30, 4]
 
This is my first dumbfound encounter with JavaScript. The reason for this is sort method converts the elements inside the array into strings and then sort them alphabetically. This is called [lexicographical order](https://en.wikipedia.org/wiki/Lexicographical_order).

>This is why you'll see episode numberings on media files (S1E01) with a prepended 0 so a lexicographic sort doesn't mess things up and allows programs to simply play/display in alphabetical order.

To get the sort function to perform numerical sort, we need to tweak the method:

For ES5 and lower versions:

    const numbers = [1, 30, 4, 21, 100000];
    numbers.sort(function(a,b){ return a - b; });
    console.log(numbers); 
    
For ES6 onwards:

    const numbers = [1, 30, 4, 21, 100000];
    numbers.sort((a,b) => a - b);   // For ascending sort
    numbers.sort((a,b) => b - a);   // For descending sort
    console.log(numbers); 
    
 The expression `a-b` sorts the array based on the following three condtions:
 
 - If a-b = 0 `a and b should be considered equals and hence no sorting will be performed`
 - If a-b > 0 `Sort b to the lower index of a`
 - If a-b < 0 `Sort a to the lower index of b`
