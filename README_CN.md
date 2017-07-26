<br/>
<p align="center">
<img src="https://i.imgur.com/bYwl7Vf.png" alt="Learn Regex">
</p><br/>

## 什么是正则表达式 ?

> 正则表达式是一组字符或符号，用于从文本中找到特定的模式。

正则表达式是从左到右匹配主题字符串的模式。 “Regular expression” 通常会缩写为 “regex” 或 “regexp”。 正则表达式用于替换其中的文本 一个字符串、验证表单、根据模式匹配从字符串中提取一个子字符串、或者更多。

想象一下，您正在编写应用程序，并且您希望在用户选择用户名时设置规则。 我们想要用户名可以 包含字母，数字，下划线和连字符。 我们还想限制用户名中的字符数，因此它看起来不丑。 我们使用以下正则表达式来验证用户名：

<p align="center">
<img src="https://i.imgur.com/UrDb9qc.png" alt="Regular expression">
</p>

上图正则表达式可以接受字符串包括 john_doe, jo-hn\_doe and john12\_as. 并不会匹配 Jo 因为这个字符串包含大写字母，并且长度太短。

## 目录

- [基本匹配](#1-basic-matchers)
- [元字符](#2-meta-characters)
  - [Full stop](#21-full-stop)
  - [Character set](#22-character-set)
    - [Negated character set](#221-negated-character-set)
  - [Repetitions](#23-repetitions)
    - [The Star](#231-the-star)
    - [The Plus](#232-the-plus)
    - [The Question Mark](#233-the-question-mark)
  - [Braces](#24-braces)
  - [Character Group](#25-character-group)
  - [Alternation](#26-alternation)
  - [Escaping special character](#27-escaping-special-character)
  - [Anchors](#28-anchors)
    - [Caret](#281-caret)
    - [Dollar](#282-dollar)
- [Shorthand Character Sets](#3-shorthand-character-sets)
- [Lookaround](#4-lookaround)
  - [Positive Lookahead](#41-positive-lookahead)
  - [Negative Lookahead](#42-negative-lookahead)
  - [Positive Lookbehind](#43-positive-lookbehind)
  - [Negative Lookbehind](#44-negative-lookbehind)
- [Flags](#5-flags)
  - [Case Insensitive](#51-case-insensitive)
  - [Global search](#52-global-search)
  - [Multiline](#53-multiline)
- [Bonus](#bonus)

## 1. 基础匹配

基本匹配只是用于在文本中搜索字母和数字的模式.举个栗子 cat 字母 c, 其次是字母a, 其次是字母t

<pre>
"cat" => The <a href="#learn-regex"><strong>cat</strong></a> sat on the mat
</pre>

正则表达式 123 可以匹配到 “123”. 正则表达式是通过表达式中的匹配字符串针对输入字符串进行比较依次匹配,注意:正则表达式是正常的区分大小写.所以 Cat 并不能匹配到 “cat”

<pre>
"Cat" => The cat sat on the <a href="#learn-regex"><strong>Cat</strong></a>
</pre>

## 2. 元字符

元字符是正则表达式的组成部分。元字符不代表自己，而是 以某种特殊的方式解释。有些元字符在方括号内有特殊的含义。 元字符如下所示:

|元字符|描述描述|
|:----:|----|
|.|除了换行符外，句点匹配任何单个字符。|
|[ ]|字符类。匹配方括号中包含的任何字符。|
|[^ ]|否定字符类。匹配方括号中不包含的字符。|
|*|匹配前一个符号(子表达式) 0 或 多次。|
|+|匹配前一个符号(子表达式) 一次 或 多次。
|?|匹配前一个符号(子表达式)可选。eg: ca(t)? 可以匹配到 “cat” 或 “ca” 中的 “ca”。|
|{n,m}|花括号.匹配前一个符号 n < m 次 。|
|(xyz)|字符集合. 按照确切的顺序匹配字符 xyz。|
|&#124;|交替. 匹配之前的字符或符号后面的字符。|
|&#92;|转义下一个字符. 他允许你匹配保留的字符 [ ] ( ) { } . * + ? ^ $ \ | <code>[ ] ( ) { } . * + ? ^ $ \ &#124;</code>|
|^|匹配输入字符串的开始位置。|
|$|匹配输入字符串的结束位置。|

## 2.1 点

点 . 是最简单的元字符. . 匹配除 “\r\n” 之外的任何单个字符. For example the regular expression .ar 表示: 任意单个字符 + a + r

<pre>
".ar" => The <a href="#learn-regex"><strong>car</strong></a> <a href="#learn-regex"><strong>par</strong></a>ked in the <a href="#learn-regex"><strong>gar</strong></a>age.
</pre>

## 2.2 Character set

Character sets are also called character class. Square brackets are used to specify character sets. Use hyphen inside character set to
specify the characters range. The order of the character range inside square brackets doesn't matter. For example the regular
expression `[Tt]he` means: an uppercase `T` or lowercase `t`, followed by the letter `h`, followed by the letter `e`.

<pre>
"[Tt]he" => <a href="#learn-regex"><strong>The</strong></a> car parked in <a href="#learn-regex"><strong>the</strong></a> garage.
</pre>

A period inside a character set, however, means a literal period. The regular expression `ar[.]` means: a lowercase character `a`, followed by letter `r`, followed by a period `.` character.

<pre>
"ar[.]" => A garage is a good place to park a c<a href="#learn-regex"><strong>ar.</strong></a>
</pre>

### 2.2.1 Negated character set

In general the caret symbol represents the start of the string, but when it is typed after the opening square bracket it negates the
character set. For example the regular expression `[^c]ar` means: any character except `c`, followed by the character `a`, followed by
the letter `r`.

<pre>
"[^c]ar" => The car <a href="#learn-regex"><strong>par</strong></a>ked in the <a href="#learn-regex"><strong>gar</strong></a>age.
</pre>


## 2.3 Repetitions

Following meta characters `+`, `*` or `?` are used to specify how many times a subpattern can occurs. These meta characters act
differently in different situations.

### 2.3.1 The Star

The symbol `*` matches zero or more repetitions of the preceding matcher. The regular expression `a*` means: zero or more repetitions
of preceding lowercase character `a`. But if it appears after a character set or class that it finds the repetitions of the whole
character set. For example the regular expression `[a-z]*` means: any number of lowercase letters in a row.

<pre>
"[a-z]*" => T<a href="#learn-regex"><strong>he</strong></a> <a href="#learn-regex"><strong>car</strong></a> <a href="#learn-regex"><strong>parked</strong></a> <a href="#learn-regex"><strong>in</strong></a> <a href="#learn-regex"><strong>the</strong></a> <a href="#learn-regex"><strong>garage</strong></a> #21.
</pre>

The `*` symbol can be used with the meta character `.` to match any string of characters `.*`. The `*` symbol can be used with the
whitespace character `\s` to match a string of whitespace characters. For example the expression `\s*cat\s*` means: zero or more
spaces, followed by lowercase character `c`, followed by lowercase character `a`, followed by lowercase character `t`, followed by
zero or more spaces.

<pre>
"\s*cat\s*" => The fat<a href="#learn-regex"><strong> cat </strong></a>sat on the <a href="#learn-regex"><strong>cat</strong></a>.
</pre>

### 2.3.2 The Plus

The symbol `+` matches one or more repetitions of the preceding character. For example the regular expression `c.+t` means: lowercase
letter `c`, followed by any number of character, followed by the lowercase character `t`.

<pre>
"c.+t" => The fat <a href="#learn-regex"><strong>cat sat on the mat</strong></a>.
</pre>

### 2.3.3 The Question Mark

In regular expression the meta character `?` makes the preceding character optional. This symbol matches zero or one instance of
the preceding character. For example the regular expression `[T]?he` means: Optional the uppercase letter `T`, followed by the lowercase
character `h`, followed by the lowercase character `e`.

<pre>
"[T]he" => <a href="#learn-regex"><strong>The</strong></a> car is parked in the garage.
</pre>
<pre>
"[T]?he" => <a href="#learn-regex"><strong>The</strong></a> car is parked in t<a href="#learn-regex"><strong>he</strong></a> garage.
</pre>

## 2.4 Braces

In  regular expression braces that are also called quantifiers used to specify the number of times that a group of character or a
character can be repeated. For example the regular expression `[0-9]{2,3}` means: Match at least 2 digits but not more than 3 (
characters in the range of 0 to 9).

<pre>
"[0-9]{2,3}" => The number was 9.<a href="#learn-regex"><strong>999</strong></a>7 but we rounded it off to <a href="#learn-regex"><strong>10</strong></a>.0.
</pre>

We can leave out the second number. For example the regular expression `[0-9]{2,}` means: Match 2 or more digits. If we also remove
the comma the regular expression `[0-9]{2}` means: Match exactly 2 digits.

<pre>
"[0-9]{2,}" => The number was 9.<a href="#learn-regex"><strong>9997</strong></a> but we rounded it off to <a href="#learn-regex"><strong>10</strong></a>.0.
</pre>

<pre>
"[0-9]{2}" => The number was 9.<a href="#learn-regex"><strong>99</strong></a><a href="#learn-regex"><strong>97</strong></a> but we rounded it off to <a href="#learn-regex"><strong>10</strong></a>.0.
</pre>

## 2.5 Character Group

Character group is a group of sub-pattern that is written inside Parentheses `(...)`. As we discussed before that in regular expression
if we put quantifier after character than it will repeats the preceding character. But if we put quantifier after a character group than
it repeats the whole character group. For example the regular expression `(ab)*` matches zero or more repetitions of the character "ab".
We can also use the alternation `|` meta character inside character group. For example the regular expression `(c|g|p)ar` means: lowercase character `c`,
`g` or `p`, followed by character `a`, followed by character `r`.

<pre>
"(c|g|p)ar" => The <a href="#learn-regex"><strong>car</strong></a> is <a href="#learn-regex"><strong>par</strong></a>ked in the <a href="#learn-regex"><strong>gar</strong></a>age.
</pre>

## 2.6 Alternation

In regular expression Vertical bar `|` is used to define alternation. Alternation is like a condition between multiple expressions. Now,
you maybe thinking that character set and alternation works the same way. But the big difference between character set and alternation
is that character set works on character level but alternation works on expression level. For example the regular expression
`(T|t)he|car` means: uppercase character `T` or lowercase `t`, followed by lowercase character `h`, followed by lowercase character `e`
or lowercase character `c`, followed by lowercase character `a`, followed by lowercase character `r`.

<pre>
"(T|t)he|car" => <a href="#learn-regex"><strong>The</strong></a> <a href="#learn-regex"><strong>car</strong></a> is parked in <a href="#learn-regex"><strong>the</strong></a> garage.
</pre>

## 2.7 Escaping special character

Backslash `\` is used in regular expression to escape the next character. This allows to to specify a symbol as a matching character
including reserved characters `{ } [ ] / \ + * . $ ^ | ?`. To use a special character as a matching character prepend `\` before it.
For example the regular expression `.` is used to match any character except new line. Now to match `.` in an input string the regular
expression `(f|c|m)at\.?` means: lowercase letter `f`, `c` or `m`, followed by lowercase character `a`, followed by lowercase letter
`t`, followed by optional `.` character.

<pre>
"(f|c|m)at\.?" => The <a href="#learn-regex"><strong>fat</strong></a> <a href="#learn-regex"><strong>cat</strong></a> sat on the <a href="#learn-regex"><strong>mat.</strong></a>
</pre>

## 2.8 Anchors

In regular expression to check if the matching symbol is the starting symbol or ending symbol of the input string for this purpose
we use anchors. Anchors are of two types: First type is Caret `^` that check if the matching character is the start character of the
input and the second type is Dollar `$` that checks if matching character is the last character of the input string.

### 2.8.1 Caret

Caret `^` symbol is used to check if matching character is the first character of the input string. If we apply the following regular
expression `^a` (if a is the starting symbol) to input string `abc` it matches `a`. But if we apply regular expression `^b` on above
input string it does not match anything. Because in input string `abc` "b" is not the starting symbol. Let's take a look on another
regular expression `^(T|t)he` which means: uppercase character `T` or lowercase character `t` is the start symbol of the input string,
followed by lowercase character `h`, followed by lowercase character `e`.

<pre>
"(T|t)he" => <a href="#learn-regex"><strong>The</strong></a> car is parked in <a href="#learn-regex"><strong>the</strong></a> garage.
</pre>

<pre>
"^(T|t)he" => <a href="#learn-regex"><strong>The</strong></a> car is parked in the garage.
</pre>

### 2.8.2 Dollar

Dollar `$` symbol is used to check if matching character is the last character of the input string. For example regular expression
`(at\.)$` means: a lowercase character `a`, followed by lowercase character `t`, followed by a `.` character and the matcher
must be end of the string.

<pre>
"(at\.)" => The fat c<a href="#learn-regex"><strong>at.</strong></a> s<a href="#learn-regex"><strong>at.</strong></a> on the m<a href="#learn-regex"><strong>at.</strong></a>
</pre>

<pre>
"(at\.)$" => The fat cat sat on the m<a href="#learn-regex"><strong>at.</strong></a>
</pre>

##  3. Shorthand Character Sets

Regular expression provides shorthands for the commonly used character sets, which offer convenient shorthands for commonly used
regular expressions. The shorthand character sets are as follows:

|Shorthand|Description|
|:----:|----|
|.|Any character except new line|
|\w|Matches alphanumeric characters: `[a-zA-Z0-9_]`|
|\W|Matches non-alphanumeric characters: `[^\w]`|
|\d|Matches digit: `[0-9]`|
|\D|Matches non-digit: `[^\d]`|
|\s|Matches whitespace character: `[\t\n\f\r\p{Z}]`|
|\S|Matches non-whitespace character: `[^\s]`|

## 4. Lookaround

Lookbehind and lookahead sometimes known as lookaround are specific type of ***non-capturing group*** (Use to match the pattern but not
included in matching list). Lookaheads are used when we have the condition that this pattern is preceded or followed by another certain
pattern. For example we want to get all numbers that are preceded by `$` character from the following input string `$4.44 and $10.88`.
We will use following regular expression `(?<=\$)[0-9\.]*` which means: get all the numbers which contains `.` character and preceded
by `$` character. Following are the lookarounds that are used in regular expressions:

|Symbol|Description|
|:----:|----|
|?=|Positive Lookahead|
|?!|Negative Lookahead|
|?<=|Positive Lookbehind|
|?<!|Negative Lookbehind|

### 4.1 Positive Lookahead

The positive lookahead asserts that the first part of the expression must be followed by the lookahead expression. The returned match
only contains the text that is matched by the first part of the expression. To define a positive lookahead braces are used and within
those braces question mark with equal sign is used like this `(?=...)`. Lookahead expression is written after the equal sign inside
braces. For example the regular expression `(T|t)he(?=\sfat)` means: optionally match lowercase letter `t` or uppercase letter `T`,
followed by letter `h`, followed by letter `e`. In braces we define positive lookahead which tells regular expression engine to match
`The` or `the` which are followed by the word `fat`.

<pre>
"(T|t)he(?=\sfat)" => <a href="#learn-regex"><strong>The</strong></a> fat cat sat on the mat.
</pre>

### 4.2 Negative Lookahead

Negative lookahead is used when we need to get all matches from input string that are not followed by a pattern. Negative lookahead
defined same as we define positive lookahead but the only difference is instead of equal `=` character we use negation `!` character
i.e. `(?!...)`. Let's take a look at the following regular expression `(T|t)he(?!\sfat)` which means: get all `The` or `the` words from
input string that are not followed by the word `fat` precedes by a space character.

<pre>
"(T|t)he(?!\sfat)" => The fat cat sat on <a href="#learn-regex"><strong>the</strong></a> mat.
</pre>

### 4.3 Positive Lookbehind

Positive lookbehind is used to get all the matches that are preceded by a specific pattern. Positive lookbehind is denoted by
`(?<=...)`. For example the regular expression `(?<=(T|t)he\s)(fat|mat)` means: get all `fat` or `mat` words from input string that
are after the word `The` or `the`.

<pre>
"(?<=(T|t)he\s)(fat|mat)" => The <a href="#learn-regex"><strong>fat</strong></a> cat sat on the <a href="#learn-regex"><strong>mat</strong></a>.
</pre>

### 4.4 Negative Lookbehind

Negative lookbehind is used to get all the matches that are not preceded by a specific pattern. Negative lookbehind is denoted by
`(?<!...)`. For example the regular expression `(?&lt;!(T|t)he\s)(cat)` means: get all `cat` words from input string that
are after not after the word `The` or `the`.

<pre>
"(?&lt;!(T|t)he\s)(cat)" => The cat sat on <a href="#learn-regex"><strong>cat</strong></a>.
</pre>

## 5. Flags

Flags are also called modifiers because they modify the output of a regular expression. These flags can be used in any order or
combination, and are an integral part of the RegExp.

|Flag|Description|
|:----:|----|
|i|Case insensitive: Sets matching to be case-insensitive.|
|g|Global Search: Search for a pattern throughout the input string.|
|m|Multiline: Anchor meta character works on each line.|

### 5.1 Case Insensitive

The `i` modifier is used to perform case-insensitive matching. For example the regular expression `/The/gi` means: uppercase letter
`T`, followed by lowercase character `h`, followed by character `e`. And at the end of regular expression the `i` flag tells the
regular expression engine to ignore the case. As you can see we also provided `g` flag because we want to search for the pattern in
the whole input string.

<pre>
"The" => <a href="#learn-regex"><strong>The</strong></a> fat cat sat on the mat.
</pre>

<pre>
"/The/gi" => <a href="#learn-regex"><strong>The</strong></a> fat cat sat on <a href="#learn-regex"><strong>the</strong></a> mat.
</pre>

### 5.2 Global search


The `g` modifier is used to perform a global match (find all matches rather than stopping after the first match). For example the
regular expression`/.(at)/g` means: any character except new line, followed by lowercase character `a`, followed by lowercase
character `t`. Because we provided `g` flag at the end of the regular expression now it will find every matches from whole input
string.



<pre>
".(at)" => The <a href="#learn-regex"><strong>fat</strong></a> cat sat on the mat.
</pre>

<pre>
"/.(at)/g" => The <a href="#learn-regex"><strong>fat</strong></a> <a href="#learn-regex"><strong>cat</strong></a> <a href="#learn-regex"><strong>sat</strong></a> on the <a href="#learn-regex"><strong>mat</strong></a>.
</pre>

### 5.3 Multiline

The `m` modifier is used to perform a multi line match. As we discussed earlier anchors `(^, $)` are used to check if pattern is
the beginning of the input or end of the input string. But if we want that anchors works on each line we use `m` flag. For example the
regular expression `/at(.)?$/gm` means: lowercase character `a`, followed by lowercase character `t`, optionally anything except new
line. And because of `m` flag now regular expression engine matches pattern at the end of each line in a string.

<pre>
"/.at(.)?$/" => The fat
                cat sat
                on the <a href="#learn-regex"><strong>mat.</strong></a>
</pre>

<pre>
"/.at(.)?$/gm" => The <a href="#learn-regex"><strong>fat</strong></a>
                  cat <a href="#learn-regex"><strong>sat</strong></a>
                  on the <a href="#learn-regex"><strong>mat.</strong></a>
</pre>

## Bonus

* *Positive Integers*: `^\d+$`
* *Negative Integers*: `^-\d+$`
* *US Phone Number*: `^+?[\d\s]{3,}$`
* *US Phone with code*: `^+?[\d\s]+(?[\d\s]{10,}$`
* *Integers*: `^-?\d+$`
* *Username*: `^[\w\d_.]{4,16}$`
* *Alpha-numeric characters*: `^[a-zA-Z0-9]*$`
* *Alpha-numeric characters with spaces*: `^[a-zA-Z0-9 ]*$`
* *Password*: `^(?=^.{6,}$)((?=.*[A-Za-z0-9])(?=.*[A-Z])(?=.*[a-z]))^.*$`
* *email*: `^([a-zA-Z0-9._%-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,4})*$`
* *IPv4 address*: `^((?:(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.){3}(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?))*$`
* *Lowercase letters only*: `^([a-z])*$`
* *Uppercase letters only*: `^([A-Z])*$`
* *URL*: `^(((http|https|ftp):\/\/)?([[a-zA-Z0-9]\-\.])+(\.)([[a-zA-Z0-9]]){2,4}([[a-zA-Z0-9]\/+=%&_\.~?\-]*))*$`
* *VISA credit card numbers*: `^(4[0-9]{12}(?:[0-9]{3})?)*$`
* *Date (MM/DD/YYYY)*: `^(0?[1-9]|1[012])[- /.](0?[1-9]|[12][0-9]|3[01])[- /.](19|20)?[0-9]{2}$`
* *Date (YYYY/MM/DD)*: `^(19|20)?[0-9]{2}[- /.](0?[1-9]|1[012])[- /.](0?[1-9]|[12][0-9]|3[01])$`
* *MasterCard credit card numbers*: `^(5[1-5][0-9]{14})*$`

## Contribution

* Report issues
* Open pull request with improvements
* Spread the word
* Reach out to me directly at ziishaned@gmail.com or [![Twitter URL](https://img.shields.io/twitter/url/https/twitter.com/ziishaned.svg?style=social&label=Follow%20%40ziishaned)](https://twitter.com/ziishaned)

## License

MIT © [Zeeshan Ahmed](mailto:ziishaned@gmail.com)
