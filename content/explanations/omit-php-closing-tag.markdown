---
type:          post
draft:          true
author:         "Henrique Dias"
title:          ""
date:           2015-09-02 10:00:00+00:00
description:    ""
categories:
- Explanations
tags:
- PHP
- Tip
- Tags
---

Some time ago, I noticed that many developers don't close the ```<?php``` tag in their code and I got curious. After a bit of searching, I found out **why** so many developers do that.

Firstly, I must say that this is a good practice when the file content is only PHP. When it contains HTML, for example, we have to use the closing tag.

## What if...?

Let's start asking the question **what happens when the closing tag is ommited**? The answer is very simple: if we omit the closing tag, all of the blank spaces afterwards are treated as useless things.

And **what happens when the closing tag is used**? So, if you put the ```?>``` at the end of your PHP file, everything (even the most futil character) after is sent to the browser. That can lead to some errors related with the [HTTP headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers).

## What do you mean?

In a lot of web sites and web applications that use PHP, the functions ```session_start``` and ```header``` are frequently used. Those are mainly used to manipulate the content of the HTTP headers.

So, if you let some blank lines after closing the PHP tag, you may get the error "Cannot modify header information – headers already sent" because it alredy has sent some blank information to the browser.

## Examples, please

Of course I didn't forget the examples. So, let's begin. If the file **only contains PHP** code, you don't have to worry about the closing tag. Simply don't use it.

{{< highlight php >}}
<?php

$myString = 'MyString';
$myArr = [0, 'item', $myString];

function my_function {
  //...
}
{{< /highlight >}}

And now, what if I want to **mix** PHP with HTML (which I highly don't recommend, it's a big mess)? But if you want to use both languages, you must **use** the closing that. If possible, avoid the blank spaces.

{{< highlight php >}}
<?php

//...PHP code

?><html>
  <head>

  </head>
  <body>

  </body>
</html>
{{< /highlight >}}

If you want to intercalate PHP into HTML for simple scripts, you have to use both opening and closing tags. Of course, if the code goes **after** the closing of ```html``` tag, you don't need to close the PHP tag. See:

{{< highlight php >}}
<div>
  <?php

  //...

  ?>
</div>
<p> <?php echo $myVar; ?> </p>
</body>
</html>

<?php
// no need to close here
{{< /highlight >}}

## Be careful!

When using both PHP and HTML in the same file, you must warrant that all of the functions which modify the HTTP headers are put *before* of any HTML output.

{{< highlight php >}}
<?php session_start(); ?>
<html>
  <!-- No error will be generated  -->
</html>
{{< /highlight >}}

## Your questions

Someone asked what would happen if a PHP file **without** the closing tag was included into an HTML file. It means something like in the following situation:

```class.php```

{{< highlight php >}}
<?php

$foo = "bar";

function MyFunc() {
  //...
}
{{< /highlight >}}

```index.php```

{{< highlight php >}}
<?php

include 'class.php'; // or require

?>

<html>
  <!-- ... -->
</html>
{{< /highlight >}}

The answer is **no** because after the inclusion of the file there is a closing tag. Everything after the closing tag is outputted to the browser as it is (if it doesn't include any server-side modifications). 
