---
title: "Markdown Cheatsheet"
date: 2023-07-06
draft: false
---

My first post will be a cheatseet for **Markdown**. Mainly because I need to refresh the syntax for this blog :)

It includes some specifics for Markdown in combination with [Hugo](https://gohugo.io).

For each section there will be shown the raw sytax, followed by the rendered result.

## Headers
---

    # H1
    ## H2
    ### H3
    #### H4
    ##### H5
    ###### H6

# H1
## H2
### H3
#### H4
##### H5
###### H6

## Emphasis
---

    Emphasis, aka italics, with *asterisks* or _underscores_.

    Strong emphasis, aka bold, with **asterisks** or __underscores__.

    Combined emphasis with **asterisks and _underscores_**.

    Strikethrough uses two tildes. ~~Scratch this.~~

Emphasis, aka italics, with *asterisks* or _underscores_.

Strong emphasis, aka bold, with **asterisks** or __underscores__.

Combined emphasis with **asterisks and _underscores_**.

Strikethrough uses two tildes. ~~Scratch this.~~


## Lists
---

    1. First ordered list item
    2. Another item
    ...* Unordered sub-list. 
    1. Actual numbers don't matter, just that it's a number
    ...1. Ordered sub-list
    4. And another item.
    
    ⋅⋅⋅You can have properly indented paragraphs within list items. Notice the blank line above, and the leading spaces (at least one, but we'll use three here to also align the raw Markdown).
    
    ⋅⋅⋅To have a line break without a paragraph, you will need to use two trailing spaces.⋅⋅
    ⋅⋅⋅Note that this line is separate, but within the same paragraph.⋅⋅
    ⋅⋅⋅(This is contrary to the typical GFM line break behaviour, where trailing spaces are not required.)
    
    * Unordered list can use asterisks
    - Or minuses
    + Or pluses

1. First ordered list item
2. Another item
   * Unordered sub-list. 
1. Actual numbers don't matter, just that it's a number
   1. Ordered sub-list
4. And another item.

   You can have properly indented paragraphs within list items. Notice the blank line above, and the leading spaces (at least one, but we'll use three here to also align the raw Markdown).

   To have a line break without a paragraph, you will need to use two trailing spaces.  
   Note that this line is separate, but within the same paragraph.  
   (This is contrary to the typical GFM line break behaviour, where trailing spaces are not required.)

* Unordered list can use asterisks
- Or minuses
+ Or pluses

## Links
---
There are two ways to create links.

    [I'm an inline-style link](https://duckduckgo.com/)

    [I'm an inline-style link with title](https://duckduckgo.com/ "DuckDuckGo Frontpage")

    [I'm a reference-style link][Arbitrary case-insensitive reference text]

    [I'm a relative reference to a repository file](../ctfs/)

    [You can use numbers for reference-style link definitions][1]

    Or leave it empty and use the [link text itself].

    URLs and URLs in angle brackets will automatically get turned into links. 
    http://www.example.com or <http://www.example.com> and sometimes 
    example.com (but not on Github, for example).

    Some text to show that the reference links can follow later.

    [arbitrary case-insensitive reference text]: https://www.mozilla.org
    [1]: https://gchq.github.io/CyberChef/
    [link text itself]: http://www.reddit.com

[I'm an inline-style link](https://duckduckgo.com/)

[I'm an inline-style link with title](https://duckduckgo.com/ "DuckDuckGo Frontpage")

[I'm a reference-style link][Arbitrary case-insensitive reference text]

[I'm a relative reference to a repository file](../ctfs/)

[You can use numbers for reference-style link definitions][1]

Or leave it empty and use the [link text itself].

URLs and URLs in angle brackets will automatically get turned into links. 
http://www.example.com or <http://www.example.com> and sometimes 
example.com (but not on Github, for example).

Some text to show that the reference links can follow later.

[arbitrary case-insensitive reference text]: https://www.mozilla.org
[1]: https://gchq.github.io/CyberChef/
[link text itself]: http://www.reddit.com

## Images

    Inline-style: 
    ![alt text](https://github.com/stempst0r/stempst0r.github.io/blob/main/static/android-chrome-192x192.png?raw=true "Logo Title Text 1")

    Reference-style: 
    ![alt text][logo]

    [logo]: https://github.com/stempst0r/stempst0r.github.io/blob/main/static/android-chrome-192x192.png?raw=true "Logo Title Text 2"

Inline-style: 
![alt text](https://github.com/stempst0r/stempst0r.github.io/blob/main/static/android-chrome-192x192.png?raw=true "Logo Title Text 1")

Reference-style: 
![alt text][logo]

[logo]: https://github.com/stempst0r/stempst0r.github.io/blob/main/static/android-chrome-192x192.png?raw=true "Logo Title Text 2"

## Code and Syntax Highlighting
---
Code blocks are part of the Markdown spec, but syntax highlighting isn't. However, many renderers -- like Github's and Markdown Here -- support syntax highlighting. Which languages are supported and how those language names should be written will vary from renderer to renderer.

    Inline `code` has `back-ticks around` it.

Inline `code` has `back-ticks around` it.

Blocks of code are either fenced by lines with three back-ticks ```, or are indented with four spaces. I recommend only using the fenced code blocks -- they're easier and only they support syntax highlighting.

    ```javascript
    var s = "JavaScript syntax highlighting";
    alert(s);
    ```
     
    ```python
    s = "Python syntax highlighting"
    print s
    ```
     
    ```
    No language indicated, so no syntax highlighting. 
    But let's throw in a <b>tag</b>.
    ```
```javascript
var s = "JavaScript syntax highlighting";
alert(s);
```
 
```python
s = "Python syntax highlighting"
print s
```
 
```
No language indicated, so no syntax highlighting. 
But let's throw in a <b>tag</b>.
```

## Tables
---
    Colons can be used to align columns.

    | Tables        | Are           | Cool  |
    | ------------- |:-------------:| -----:|
    | col 3 is      | right-aligned | $1600 |
    | col 2 is      | centered      |   $12 |
    | zebra stripes | are neat      |    $1 |

    There must be at least 3 dashes separating each header cell.
    The outer pipes (|) are optional, and you don't need to make the 
    raw Markdown line up prettily. You can also use inline Markdown.

    Markdown | Less | Pretty
    --- | --- | ---
    *Still* | `renders` | **nicely**
    1 | 2 | 3

Colons can be used to align columns.

| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |

There must be at least 3 dashes separating each header cell.
The outer pipes (|) are optional, and you don't need to make the 
raw Markdown line up prettily. You can also use inline Markdown.

Markdown | Less | Pretty
--- | --- | ---
*Still* | `renders` | **nicely**
1 | 2 | 3

## Blockquotes
---
    > Blockquotes are very handy in email to emulate reply text.
    > This line is part of the same quote.

    Quote break.

    > This is a very long line that will still be quoted properly when it wraps. Oh boy let's keep writing to make sure this is long enough to actually wrap for everyone. Oh, you can *put* **Markdown** into a blockquote.

> Blockquotes are very handy in email to emulate reply text.
> This line is part of the same quote.

Quote break.

> This is a very long line that will still be quoted properly when it wraps. Oh boy let's keep writing to make sure this is long enough to actually wrap for everyone. Oh, you can *put* **Markdown** into a blockquote. 

## Inline HTML
---
Inline HTML has to be activated first for Hugo. Under the top-level Hugo website Git directory, add file `layouts/shortcodes/rawhtml.html` containing:
    
    {{.Inner}}
    
Afterwards in the Markdown file for the particular blog post, it is possible to use the `rawhtml` shortcode. **Removing the space between the left brace and the left caret is mandatory**.

    {{ < rawhtml >}}
    <dl>
      <dt>Definition list</dt>
      <dd>Is something people use sometimes.</dd>

      <dt>Markdown in HTML</dt>
      <dd>Does *not* work **very** well. Use HTML <em>tags</em>.</dd>
    </dl>
    {{ < /rawhtml >}}

{{< rawhtml >}}
<dl>
  <dt>Definition list</dt>
  <dd>Is something people use sometimes.</dd>

  <dt>Markdown in HTML</dt>
  <dd>Does *not* work **very** well. Use HTML <em>tags</em>.</dd>
</dl>
{{< /rawhtml >}}

## Horizontal Rule
---
    Three or more:

    ---

    Hyphens

    ***

    Asterisks

    ___

    Underscores
    
Three or more:

---

Hyphens

***

Asterisks

___

Underscores

## Line Breaks
---
    Here's a line for us to start with.

    This line is separated from the one above by two newlines, so it will be a *separate paragraph*.

    This line is also a separate paragraph, but...
    This line is only separated by a single newline, so it's a separate line in the *same paragraph*.

Here's a line for us to start with.

This line is separated from the one above by two newlines, so it will be a *separate paragraph*.

This line is also a separate paragraph, but...
This line is only separated by a single newline, so it's a separate line in the *same paragraph*.

## YouTube Videos
---
They can't be added directly but you can add an image with a link to the video like this:
If using Hugo, don't forget to combine the HTML block with the `rawhtml` shortcode!

    <a href="http://www.youtube.com/watch?feature=player_embedded&v=YOUTUBE_VIDEO_ID_HERE
    " target="_blank"><img src="http://img.youtube.com/vi/YOUTUBE_VIDEO_ID_HERE/0.jpg" 
    alt="IMAGE ALT TEXT HERE" width="240" height="180" border="10" /></a>

{{< rawhtml >}}
<center>
<a href="http://www.youtube.com/watch?feature=player_embedded&v=dQw4w9WgXcQ
" target="_blank"><img src="http://img.youtube.com/vi/dQw4w9WgXcQ/0.jpg" 
alt="IMAGE ALT TEXT HERE" width="240" height="180" border="10" /></a>
</center>
{{< /rawhtml >}}

---
Thanks to [adam-p](https://github.com/adam-p) for his [version](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet).
