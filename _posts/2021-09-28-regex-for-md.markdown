---
layout: post
title:  "Regex for Markdown Syntax"
date:   2021-09-28 09:12:17 +0330
categories: interesting
---

### Preface

First of all, to those who are interested, I have made this extremely fun Clickbait Title generator. And to make it, I used Google's T5 transformer. Yes! The same model with a conditional head which Google uses in its translator. I did not use ther conditional head and fed the data simply by tokenizing the sentences, extracting the proper nouns, adjectives, nouns and named entities as the input and the corresponding sentence as the output. This was a simple, fun job and I just did it to show to my customers how I can make their lives easier using Google Colab. You can view the Colab Gist here:

```
https://chubakbidpaa.com/cbgen
```

Now, onwards to the main topic!

### Markdown, What is it?

Markdown was created by Aaron Swartz and John Gruber in 2002. Their aim was to create a common language that would replace the demented BBCode and other garbage that floated non-WYSIWYG editors. I still remember the PAIN that was the BBCode as a child. So when I joined Reddit, and later Stack Overflow, I really appreciated how slick Markdown was.

The format quickly took over the web in the 2010s and today most if not all platforms support it, in a way at least. Everyone has their own interpretation of Markdown. I'm currently writing down this blogpost in its purest form which is recognized by Jekyll. 

### Why did I wrote these?

I was thinking about useful CLI tools I can make in Golang, and it struck me that Markdown to Word does not exist. Even if it does, it's not very good I bet! So I decided to fire up Go and write one myself. And to recognize the syntax, of course I turned to Regex (what else?). Please star [the repo](https://github.com/Chubek/md2docx). So far I've only written the Regex patterns but the rest is coming, since these days I'm pretty much free from my usual burden of "making money to survive" and I've never felt better!

### So Chubak, stop lingering and tell us the patterns!

Ok, let's start!

**Note 1**: You can test all the stuff [here](https://regex101.com/).

**Note 2**: I've based everything on the flavour of MD found [here](https://www.markdownguide.org/basic-syntax/) --- except for the tables which is on another page.

#### One: Headings

In MD, headings are pretty simple. You just do this:

```
# Header1
## Header2
### Header3
.
.
###### Header 6
```

So the corresponding regexes (regexi?) for them will be:

```
	headerOne        = `(#{1}\s)(.*)`
	headeTwo         = `(#{2}\s)(.*)`
	headerThree      = `(#{3}\s)(.*)`
	headerFour       = `(#{4}\s)(.*)`
	headerFive       = `(#{5}\s)(.*)`
	headerSix        = `(#{6}\s)(.*)`
```

The `{n}` tells regex to match the character `#` in exctly that amount. The `\s` after that says there MUST be a whitespace character. Then, you can match absolutely EVERYTHING!

#### Two: Bold and Italic Text

There are soooo many ways to write bold, italic and bold/italic text in MD that it won't be really easy to catch them all using Regex solely by itself. However, what we can do is to catch the "possible" combinations and then count the occurances to assert if it's bold, italic, or bold/italic. So for example, `**BOLD TEXT**` is the same as `__BOLD TEXT__`, which is the same as `_*BOLD TEXT*_` which is the same as... You get the gist! So let's match all the possiblities with:

```
boldItalicText   = `(\*|\_)+(\S+)(\*|\_)+`
```
The `(\*|\_)+` says "match either * or _ in multiple occurances" and after that we have `\S+` which is what comes next. Problem with this is, it matches more than 3 occurances. Even if we say match three like we did before, then `\S+` will get the asterisks and underlines AS IT SHOULD. As I said, we clearly need some logic  to deal with possible combinations.

#### Three: Links

In MD, here's how we use links:

```
[Link Text](https://url.com "Optionl Alt")
```

We need to match the optional alt "optinally" --- but we don't need it in Word so we can get rid of it through code. So the regex is:

```
linkText         = `(\[.*\])(\((http)(?:s)?(\:\/\/).*\))`
```

So what `(\[.*\])` does that it matches titles like `[Hello!]`. Then we have `(http)(?:s)?` that will match both `http` and `https`. Then we catch the rest.

#### Four: Images

Images are similar to links:

```
![Alternative text](path/to/img.png "Text")
```

So what we do here is this:

```
imageFile        = `(\!)(\[(?:.*)?\])(\(.*(\.(jpg|png|gif|tiff|bmp))(?:(\s\"|\')(\w|\W|\d)+(\"|\'))?\)`
```

At first we match the `!`. Then we do the same thing we did with links with `(\[(?:.*)?\])` --- however, this time, we make the text optional. We then match everything BUT we then look for `(\.(jpg|png|gif|tiff|bmp)` which matches `.png`, `.jpg`, `.tiff`, `.gif` and `.bmp`. After that we match the text with `(?:(\s\"|\')(\w|\W|\d)+(\"|\'))?` but we use `(?:)?` to make it optional.


#### Five: Unordered List

So, unordered lists in MD are pretty straightforwrd:

```
* Item 1
* Item 2
* Item 3
```

So to match them, we could write:

```
	listText         = `(^(\W{1})(\s)(.*)(?:$)?)+`
```

So first we have `^(\W{1})`. This says "look for punctuations, exactly one, at the beginning of the line". We then have `(\s)` which says there MUST be a whitespace. We then match everything. At the end we use the ol' reliable optional lookahead `(?:$)?` to say it MAYBE the end of the line, to match the last list line. We don't NEED to make it optional, but just to to be safe! At the very end we wrap everything in a supergroup and add a `+` to say look for additionals.

#### Six: Numbered Text

It's pretty similar to the latter:

```
	numberedListText = `(^(\d+\.)(\s)(.*)(?:$)?)+`
```

Except, instead of `\W` we use `\d+` which matcher one or more numbers. We could have combined it with the last one, but I need the parser to differntiate between them.

#### Seven: Block Quotes

So a blockquote in MD is like:

```
> I said a very important thing!
```

So to match this:

```
	blockQuote       = `((^(\>{1})(\s)(.*)(?:$)?)+`
```

Basically, we say, with `^(\>{1})`, to look for exactly ONE `>` at the beginning of the line. Then match everything, and finish the line. I don't know why I made it optional... HELP! I'm addicted to `(?:)?`!

#### Eight: Inline Code

So inline code needs a LOT of additional parsing. But to match it:

```
inlineCode       = "(\\`{1})(.*)(\\`{1})"
```

Which basically matches a `\``. Writing this, I realized the parsing could be difficult because We have cases where we need need to escape the backtick.

#### Nine: Code Block

So in code block, which is wrapped inside three backticks, we can't use `$`. We'll have to match the newline character, `\n`. 

```
	codeBlock        = "(\\`{3}\\n+)(.*)(\\n+\\`{3})"
```

So we match three backticks, one or more newline characters, and then wrap everything inbetween. The reason we can't use `$` is that we also need the SPACE inbwtween. Try it on regex101.com and you'll see why.

#### Ten: Horizontal Line

So in MD, a horizontal line is inserted with `---`, `***`, or `===`. It's pretty simple, and straightforward:

```
	horizontalLine   = `(\=|\-|\*){3}`
```

#### Eleven: Email Text

The purpose of this pattern is not to validate the email, otherwise it would have required [THOUSANDS of characters](https://emailregex.com/)! This is way beyond the scopre of this pattern. We don't care if the emails are valid, we just want it to match any email-like string:

```
	emailText        = `(\<{1})(\S+@\S+)(\>{1})`
```

So we match `<` and `>` at the beginning and the end. Then we match `(\S+@\S+)`  ANY non-whitespace character coupled with an at sign. 

#### And finally... TABLES!!!

So this si the hardest part and I know I'll be spending hours to parse this. In MD tables are written like this:

```
|New|World|Order|
|---|-----|-----|
|It's|A Very Wonderful|Thing|
|And I|Freaking Love|It!|
```

The thing about this md2docx project is, I could care less if regex is "correct" or not because for some reason that's unbenownest to me, I'm not using a regex engine to convert the regex to HTML and then convert that to docx. This is not something I'm doing for a job, it's something I'm doing for fun and **FUN IS ALWAYS IN THE CHALLENGE!**

So let's see what I've come up with:

```
	tableText        = `(((\|)([a-zA-Z\d+\s#!@'"():;\\\/.\[\]\^<={$}>?(?!-))]+))+(\|))(?:\n)?((\|)(-+))+(\|)(\n)((\|)(\W+|\w+|\S+))+(\|$)`
```

So WHERE should we start? It's such a convoluted pattern! First, we get the first two lines, the table header and the table separator. We then get the rest using a catch-all pattern. I think you'll get a better sense of this pattern by testing it, removing some parts, etc.



### Disclaimer

These patterns are just used to MATCH the syntax. For parsing it, I'm plan to use over 15 functions. These functions will parse the matched patterns, check for other possible patterns inside it, then create the correspondign Word text.

I hope you've enjoyed this. Thanks.

**I think Chubak is a pretty cool guy. Eh writes code and he isn't afraid of anything! Find more about thi specimen on the [about page](https://chubakbidpaa.com/about).**