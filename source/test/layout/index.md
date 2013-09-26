---
layout: page
title: "Blog Layout Test"
comments: false
sharing: false
footer: true
---

Following examples from:

* The [Discount documentation](http://www.pell.portland.or.us/~orc/Code/discount/)
* The [PHP Markdown Extra documenation](http://michelf.ca/projects/php-markdown/extra/)
* [Octopress Snippets](http://octopress.org/docs/blogging/code/)
* [Octorpess Image Tag documenation](http://octopress.org/docs/plugins/image-tag/)

Lorem ipsum dolor sit amet, consectetuer adipiscing elit. Mauris magna.
Suspendisse accumsan elit non tellus.

Lorem ipsum dolor sit amet, consectetuer adipiscing elit. Mauris magna.
Suspendisse accumsan elit non tellus.

Heading One
===========

Above is an `h1` header. Lorem ipsum dolor sit amet, consectetuer adipiscing
elit. Mauris magna. Suspendisse accumsan elit non tellus.

Heading Two
-----------

Above is an `h2` header. Lorem ipsum dolor sit amet, consectetuer adipiscing
elit. Mauris magna. Suspendisse accumsan elit non tellus.

### Heading Three ###

Above is an `h3` header. Lorem ipsum dolor sit amet, consectetuer adipiscing
elit. Mauris magna. Suspendisse accumsan elit non tellus.

#### Heading Four ####

Above is an `h4` header. Lorem ipsum dolor sit amet, consectetuer adipiscing
elit. Mauris magna. Suspendisse accumsan elit non tellus.

##### Heading Five #####

Above is an `h5` header. Lorem ipsum dolor sit amet, consectetuer adipiscing
elit. Mauris magna. Suspendisse accumsan elit non tellus.

###### Heading Six ######

Above is an `h6` header. Lorem ipsum dolor sit amet, consectetuer adipiscing
elit. Mauris magna. Suspendisse accumsan elit non tellus.

### Unordered List ###

* Item one
* Item two
* Item three
    * Item three-1
    * Item three-2
* Item four

### Ordered List ###

1. Item one
1. Item two
1. Item three
    1. Item three-1
    1. Item three-2
1. Item four

### Alpha List ###

a. Item one
b. Item two
c. Item three

### Definition List ###

Apple
:   Pomaceous fruit of plants of the genus Malus in 
    the family Rosaceae.

Orange
:   The fruit of an evergreen tree of the genus Citrus.

Multiple definitions:

Apple
:   Pomaceous fruit of plants of the genus Malus in 
    the family Rosaceae.
:   An American computer company.

Orange
:   The fruit of an evergreen tree of the genus Citrus.

Multiple terms:

Term 1
Term 2
:   Definition a

Term 3
:   Definition b

In paragraphs:

Apple

:   Pomaceous fruit of plants of the genus Malus in 
    the family Rosaceae.

Orange

:    The fruit of an evergreen tree of the genus Citrus.

Multiple paragraphs:

Term 1

:   This is a definition with two paragraphs. Lorem ipsum 
    dolor sit amet, consectetuer adipiscing elit. Aliquam 
    hendrerit mi posuere lectus.

    Vestibulum enim wisi, viverra nec, fringilla in, laoreet
    vitae, risus.

:   Second definition for term 1, also wrapped in a paragraph
    because of the blank line preceding it.

Term 2

:   This definition has a code block, a blockquote and a list.

        code block.

    > block quote
    > on two lines.

    1. first list item
    2. second list item

### Inline Stuff ###

[A link](/), **bold text**, *italic text*, `Some code`, ~~strikethrough text~~
"A quotation", from `single-quote guy`, don't you know. Characters:

* (tm)
* (c)
* (r)
* 1/4
* 1/2
* 3/4
* ...
* . . .
* an --- mdash
* an -- ndash
* A^B
*  A^(B+2)

### Images ###

{% img /images/theory.jpg David E. “Theory” Wheeler %}

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec a diam lectus.
Sed sit amet ipsum mauris. Maecenas congue ligula ac quam viverra nec
consectetur ante hendrerit. Donec et mollis dolor. Praesent et diam eget
libero egestas mattis sit amet vitae augue.

{% img left /images/theory.jpg 250 David E. “Theory” Wheeler %}

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec a diam lectus.
Sed sit amet ipsum mauris. Maecenas congue ligula ac quam viverra nec
consectetur ante hendrerit. Donec et mollis dolor. Praesent et diam eget
libero egestas mattis sit amet vitae augue. Nam tincidunt congue enim, ut
porta lorem lacinia consectetur. Donec ut libero sed arcu vehicula ultricies a
non tortor. Lorem ipsum dolor sit amet, consectetur adipiscing elit. Aenean ut
gravida lorem.

{% img right /images/theory.jpg 150 David E. “Theory” Wheeler %}

Ut turpis felis, pulvinar a semper sed, adipiscing id dolor. Pellentesque
auctor nisi id magna consequat sagittis. Curabitur dapibus enim sit amet elit
pharetra tincidunt feugiat nisl imperdiet. Ut convallis libero in urna
ultrices accumsan. Donec sed odio eros. Donec viverra mi quis quam pulvinar at
malesuada arcu rhoncus. Cum sociis natoque penatibus et magnis dis parturient
montes, nascetur ridiculus mus. In rutrum accumsan ultricies. Mauris vitae
nisi at sem facilisis semper ac in est.

### Preformatted Blocks ###

Simple Markdown indented code:

    my $dbh = DBI->connect($dsn, $user, $pass, {
        PrintError           => 0,
        RaiseError           => 1,
        AutoCommit           => 1,
    });

Example With Syntax Highlighting a Caption and Link:

``` ruby Discover if a number is prime http://www.noulakaz.net/weblog/2007/03/18/a-regular-expression-to-check-for-prime-numbers/ Source Article
class Fixnum
  def prime?
    ('1' * self) !~ /^1?$|^(11+?)\1+$/
  end
end
```

Gist Embedding:

(Disabled for now pending a fix for [this bug](https://github.com/imathis/octopress/pull/1363).)

    {-% gist 5908699 %-}

Include:

{% include_code A DBI Sqitch test thingy try.pl %}

Inline code block with title and download URL:

{% codeblock Some Objective-C lang:objc https://github.com/theory/DFURLRegularExpression/blob/master/Classes/DFURLRegularExpression.h Get the file %}
[rectangle setX: 10 y: 10 width: 20 height: 20];
{% endcodeblock %}

### Block Quote ###

> Ut turpis felis, pulvinar a semper sed, adipiscing id dolor. Pellentesque
> auctor nisi id magna consequat sagittis. Curabitur dapibus enim sit amet elit
> pharetra tincidunt feugiat nisl imperdiet. Ut convallis libero in urna
> ultrices accumsan.

> Donec sed odio eros. Donec viverra mi quis quam pulvinar at malesuada arcu
> rhoncus. Cum sociis natoque penatibus et magnis dis parturient montes,
> nascetur ridiculus mus. In rutrum accumsan ultricies. Mauris vitae nisi at
> sem facilisis semper ac in est.

Using Octopress syntax:

{% blockquote %}
Last night I lay in bed looking up at the stars in the sky and I thought to
myself, where the heck is the ceiling.s
{% endblockquote %}

From a printed work:

{% blockquote Douglas Adams, The Hichhikers Guide to the Galaxy %}
Flying is learning how to throw yourself at the ground and miss.
{% endblockquote %}

From Twitter:

{% blockquote @allanbranch https://twitter.com/allanbranch/status/90766146063712256 %}
Over the past 24 hours I've been reflecting on my life & I've realized only one thing. I need a medieval battle axe.
{% endblockquote %}

From the Web:

{% blockquote Seth Godin http://sethgodin.typepad.com/seths_blog/2009/07/welcome-to-island-marketing.html Welcome to Island Marketing %}
Every interaction is both precious and an opportunity to delight.
{% endblockquote %}

### New pseudo-protocols for [] links ###

* [abbr](abbr:bar)
* [class](class:foo)
* [id](id:blah)
* [raw](raw:bar)
* [span](class:author-mark)

### Div with Class ###

> %warning%
> Ut turpis felis, pulvinar a semper sed, adipiscing id dolor. Pellentesque
> auctor nisi id magna consequat sagittis. Curabitur dapibus enim sit amet elit
> pharetra tincidunt feugiat nisl imperdiet. Ut convallis libero in urna
> ultrices accumsan.

> Donec sed odio eros. Donec viverra mi quis quam pulvinar at malesuada arcu
> rhoncus. Cum sociis natoque penatibus et magnis dis parturient montes,
> nascetur ridiculus mus. In rutrum accumsan ultricies. Mauris vitae nisi at
> sem facilisis semper ac in est.

### Tables ###

First Header  | Second Header
------------- | -------------
Content Cell  | Content Cell
Content Cell  | Content Cell

Aligned:

| Item      | Value |
| --------- | -----:|
| Computer  | $1600 |
| Phone     |   $12 |
| Pipe      |    $1 |

Inline Markup:

| Function name | Description                    |
| ------------- | ------------------------------ |
| `help()`      | Display the help window.       |
| `destroy()`   | **Destroy your computer!**     |

### Pull Quote ###

{% pullquote %}
Surround your paragraph with the pull quote tags. Then when you come to
the text you want to pull, {" surround it like this "} and that's all there is to it.
{% endpullquote %}

### Footnote ###

That's some text with a footnote.[^1]

[^1]: And that's the footnote.
