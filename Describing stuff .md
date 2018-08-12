## Describing stuff 

* an element name needs no other syntax to be to be a reference for an element, as you can see with our references with `child`
* `/` look only 1 level deep, so only for immediate children of the element proceeding it
* `//` look anywhere in the descendants of the element proceeding it
* `.` indicate the current node (you'll usually use this inside of functions)
* `..` indicate the parent node of the current element.  You can use this to have it `look` up to a previous element.  For example, "find this specific element, but then select the element parent to it"
* `@` this is used before a name to indicate that you are talking about an attribute name instead of an element name.  For example, `@href` for an `a` element.  
* `element[position number]`:  index starting at 1, allows you to indicate the "nth" instant of that element.  Example, `a[3]` would be the third a found with that query.
* `element[logical check]` You can place a variety of functions and other boolean checks inside the `[]`.  There are multiple things you can put in here (https://www.w3schools.com/xml/xsl_functions.asp).
* `element[@attribute = 'something']` you can use this to select an element with an attribute that has a specific value.  You cas alse reference the current node's element values with `.`, so `//p[. = "thing"]` finds all the p elements that have the element text value of exactly "thing".  Warning! This will look deeper into the children of that node.  If you want to look at the text value of the current element, you should use `text()` instead of `.`.  So `element[text() = "thing"]`.  You can use `text()` or `.` in any of the logical checks that you want.
* You can use compound boolean checks inside the [], such as `//element[@attribute = "something" and contains(., "another")]` . Connect these boolean checks with `and` or `or`.
* `*` is a wildcard used to represent "any element". So, you could say `//*[@class = "title"]` to find any element that has a class atribute value of "title".

Of course, all these things are used to just select the element in question.  From there, you have te extract out what you want.  This is a bit opaque when using a pretty normal xpath tool, but made much more explicit when dealing with things in python.  Particularly with lxml.  You'll only get an element object if you don't select the content that you want.  

## Extracting stuff 

* the attribute value
    * you can get this by adding `@attribute` to the end of a query
    * example:  `//a/@href`
* the element content
    * you have to use a spific function at the end of the query to get the text: `text()`  Note the `()` in there, that are required.
    * Example:  `//a/text()`
    * Remember that `/` will only look for text that belongs to the element that you have selected.  Some tools are mare permissive about this, but things like lxml are not.  If there are additional elements in there, such as text in a `<b>` tag, it will grab all the text around that tag, but none of the contents.  
    * For example, in `<p>Hello <b>world</b> humans.</p>`, if you use `//p/text()` you'll only get back "Hello . humans.".
    * You can ask it to grab all text at any descendant level under the element that you have selected.  You already know the `//` notation, and you can add it to `text()` to achieve this effect.  So `//p//text()` would grab all the text in there. 