What is CommonMark? Spec Code Try It!
Skip to main content
CommonMark Discussion
Tables in pure Markdown
Extensions

Log In
​
:loudspeaker: Please note the CommonMark spec is currently frozen with respect to features, but there are supersets of or extensible implementations of CommonMark that may support what you need.

Sep 2014
Nov 2022

kvaps

2

kvaps
May '17
Or like this:

| Server | IP | Description |
|--------|----|-------------|
| cl1 
| 192.168.100.1
| This is my first server in the list
|
| cl2 
| 10.10.1.22
| This is another one server
|
| windows-5BSD567DSLOS
| 127.0.0.12
| This is customer windows vm. dont touch this! 
|
| DFHSDDFFUCKENLONGNAME
| 192.168.1.50
| Some printer
|
...
And this:

| Item | Amount | Cost |
|:-----|-------:|-----:|
| Orange
| 10
| 7.00
|
| Bread 
| 4
| 3.00
|
| Butter
| 1
| 5.00
|
| Total |      | 15.00 |
just allow set “newline” before “|” symbol.

This solution gives you more space for creativity. Besides:

It’s no need global changes. (only one micro fix is needed)
It does not break anything.
Everyone will happy
This is the best solution, isn’t it?

1 Reply
3


notriddle

kvaps
May '17
I agree with you that pipe tables aren’t very good for big tables. I still think they should be standardized; pipe tables are such a widely-supported Markdown extension that people expect them to work whether they’re in the spec or not.

On the other hand, I think you’re unfairly characterizing HTML tables. You don’t have to put in the end tags for rows or cells. This is fine (at least, it’s specified by HTML5 to work the way we want it to):

<table>
<tr>
<th> Server
<th> IP
<th> Description
<tr>
<td> cl1
<td> 192.168.100.1
<td> This is my first server in the list
<tr>
<td> cl2
<td> 10.10.1.22
<td> This is another one server
<tr>
<td> windows-5BSD567DSLOS
<td> 127.0.0.12
<td> This is customer windows vm. dont touch this!
<tr>
<td> DFHSDDFFUCKENLONGNAME
<td> 192.168.1.50
<td> Some printer
..
</table>
I guess that MediaWiki tables are better. I’m not convinced that they’re so much better that CommonMark should add them to the spec when they’re so rare among existing implementations.

3


kvaps
May '17
Yes, I see, html5 tables it’s quite simple, thanks for this. But I love markdown and I want to make it better to using with tables too.
I described the main problem to using markdown tables to you.

I don’t want fully mediawiki tables implementation in the markdown, but I want to have opportunity to make tables in vertically mode.

This solution, is fully solves this problem.
It is needed only simple check for newline symbol before pipe in the code.
So it will allow to use common markdown tables in vertically mode too.
Why not? It is not breaking anything, but may be very useful for all users.
And it is same yet markdown tables…

2


ClikCode

kvaps
May '17
I agree with your implementation.
But I would remove the alingment beneath the headers and put them into the headers themselves.

I think this is the best solution. and the most flexable. and people can have a grid table or a list style table Which is something I like

2

18 days later

selfthinker
Jun '17
I find it odd that no-one mentioned the necessity of row headers (i.e. ths to describe rows not columns). For some reason all the current Markdown flavours which support tables only support column headers. But it is really important for accessibility to support both!

Just consider the following table of pizzas and their cost:

				Small    Large
Salami			8.99     10.99
Hawaii			9.49     11.49
Margherita		7.99      9.99
For screen reader users to understand, e.g. the cell with “11.49”, they would need to have read out the pizza (row header = Hawaii) and the size (column header = Large). Just the one or the other is not enough to understand what the content of a cell means.

Most simple text markup languages allow a header cell anywhere in a table with a quite simple syntax, e.g.

Creole: |=
txt2tags: ||
DokuWiki: ^
Textile: |_.
I personally like: |# (because users already associate # with a heading in Markdown)
Colspans and rowspans are also important for accessibility. Wrongly empty cells can confuse screen readers. But unless they happen in header cells, they are not a big barrier.

Depending on length and complexity, not having header cells marked up correctly can make a table very difficult or impossible to understand for screen reader users.

4


Crissov

1
Jun '17
I agree that row headers are important, but marking them up may become clumsy – perhaps add a colon before the pipe:

|          | Small | Large |  
| -------- | ----- | ----- |  
|  Salami :|  8.99 | 10.99 |  
|  Hawaii :|  9.49 | 11.49 |  
|  Marge. :|  7.99 |  9.99 |  
Another observation I want to throw in here: Fallback rendering for pipe tables is improved in most implementations if each line is ended by a double space. The only exception is Blackfriday which does not render a table then.


babelmark.github.io

babelmark3 | Compare Markdown Implementations 81
babelmark3 allows to compare various implementations of Markdown

## With double space

| Column Header | Second Column |  
| ------------- | ------------- |  
| data cell     | second cell   |  
| third cell    | fourth cell   |  

## Without double space

| Column Header | Second Column |
| ------------- | ------------- |
| data cell     | second cell   |
| third cell    | fourth cell   |
1


chrisalley
Jun '17
I agree that row headers are important, but marking them up may become clumsy – perhaps add a colon before the pipe:

Could the parser automatically figure out that it is a row header based on the empty cell in the top row?




Crissov
Jun '17
It could, but that’s not always the case. Too unreliable.



8 months later

vas

jgm
Feb '18
John, given that it’s been 3½ years since you wrote that, and since in that time GitHub and many others have adopted CommonMark, should the core be settled without further delay?



2 months later

Jitsusama
Mar '18
I would like to say at this point, I don’t care much about what table format is supported, but rather, that any are.




chrisalley
Mar '18
I would like to say at this point, I don’t care much about what table format is supported, but rather, that any are.

There is already a tables extension as part of the GitHub Flavored Markdown spec 51 (which is a superset of CommonMark). You can use this today. If a tables extension is ever formalised as part of the CommonMark project, it would need to aim for compatibility with GFM since the GFM table syntax is already widely used and the goal of CommonMark is to be highly compatible with existing implementations.

2 Replies
2


jgm
Pandoc, PEG-markdown, BabelMark2

1
Mar '18
I agree that compatibility with the widely used pipe table syntax is a good idea. Here are some thoughts I wrote up a couple years ago, towards a spec for tables that is largely compatible with existing pipe tables but more flexible:

I tentatively agree with the current syntax’s decision that pipes | create cell structure, and that literal pipes need to be escaped even inside code backticks. This is a departure from the general principle that nothing needs to be escaped inside code backticks, but it conforms to the general commonmark idea that block structure is discerned prior to inline structure.

Headers should be optional. So, this should be a table:

| a | b |
This too:

|:--:|--:|
| a  | b |
I think | should be required at the beginning and end of the row. I’d like to reserve this syntax for line blocks:

| 15 Main St.
| Chicago, IL
Note that existing pipe tables allow the leading and trailing pipe to be skipped. However, this makes it harder for parsers to tell right away whether we have a table line, and there’s also the issue flagged above about line blocks. Finally, especially if headers are not required, this increases the probability that a line with a literal | will be wrongly treated as a table.

Alignments should be supported, in the now-standard way:

| right | left | center |
| ---:  | :--- | :----: |
What to do if the rows have different numbers of columns? Presumably just add empty columns to all the rows. But there’s a question whether we should take the headers to determine the number of columns (and truncate body rows if needed) or just take the maximum number of cells in any row.

That would be the minimum.

I think colspans could be supported thus:

| this spans two columns  || third column |
| this spans three columns              |||
| aaa      | bbb          |  cccc         |
This means that if you want a blank cell you need space between the ||s. This would be a departure from the way pipe tables currently work.

Maybe for rowspans:

| this spans two rows |  bbb |
|^                    |  ccc |
These can be combined:

| this spans two rows and two colums|| bbb |
|^                                  || ccc |
| aaa             | bbb              | ccc |
One more thing that would be attractive in table syntax is a way to include long paragraphs in cells (or even multiple block elements).

For long paragraphs, one could do something like this:

| aaa | this is really long so I just |
!     | continue down here            |
| new | row here                      |
Here the exclamation marks say: add the contents of these cells to the cells above them. One could even have a list in a table cell using this syntax:

| aaa | - item one         |
!     | - item two         |
| new | row here           |
The ! is very similar to the |, which has its good points and its bad points. Alternatively one could choose a different character with more contrast.

| aaa | - item one         |
+     | - item two         |
| new | row here           |
Note that allowing long cell contents, and especially block level content inside cells, raises some issues about table layout. In HTML output this isn’t a problem, since browsers compute column widths that (usually) make sense for the content. But in other output formats, like latex, one must explicitly specify column widths. So one question is whether to have something in the syntax that represents relative column widths. Pandoc does this by using the lines of - under the headers, but only in cases where the cells are too long to be represented without wrapping.

EDIT: fixed initial |s in code blocks, which this forum converted to > for some reason… @codinghorror - this seems to be a bug in discourse’s processing of posts received by HTML.

2 Replies
2


Jitsusama

chrisalley
Mar '18
I’ve tried to find an implementation of it to no avail.

Joel Gerber

1 Reply



chrisalley
Apr '18
I’ve tried to find an implementation of it to no avail.


GitHub

github/cmark-gfm 202
GitHub's fork of cmark, a CommonMark parsing and rendering library and program in C - github/cmark-gfm

2

4 months later

Mathieu_Duponchelle
Aug '18
This discussion will soon be four years old, it would be lovely if at some point something was standardized :slight_smile:

3

5 months later

aoudad

2
Jan '19
A pipe table syntax with lots of features:

|              | Header 1        | Header 2                       || Header 3                       ||
|              | Subheader 1     | Subheader 2.1  | Subheader 2.2  | Subheader 3.1  | Subheader 3.2  |
|==============|-----------------|----------------|----------------|----------------|----------------|
| Row Header 1 | 3row, 3col span                                 ||| Colspan only                   ||
| Row Header 2 |       ^                                         ||| Rowspan only   | Cell           |
| Row Header 3 |       ^                                         |||       ^        | Cell           |
| Row Header 4 |  Row            |  Each cell     |:   Centered   :| Right-aligned :|: Left-aligned  |
:              :  with multiple  :  has room for  :   multi-line   :    multi-line  :  multi-line    :
:              :  lines.         :  more text.    :      text.     :         text.  :  text.         :
|--------------|-----------------|----------------|----------------|----------------|----------------|
[Caption Text]
rendered-html-table
rendered-html-table.png
1862×702 63.8 KB
Multiple rows of headers and subheaders (Thank you vas)

Row headers, which are indicated by replacing dashes - with equals signs = in the first column’s delimiter row (Thank you again vas)

Row spans using a carat/circumflex ^ (Thank you jgm)

Column spans using multiple pipes ||| (MultiMarkdown 129, Maruku 18)

Caption surrounded by brackets [ ] on the line just below the table (MultiMarkdown 129)

Multi-line cell continuation using a colon : in place of a pipe | (like in PostreSQL’s interactive terminal 6, as discussed by David Wheeler in RFC: A Simple Markdown Table Format 27 and suggested above by illionas)

Per-cell alignment using colon(s) : inside a cell, to the left/right/both of the cell’s first line of text (inspired by the |:---:| syntax for per-column alignment)

An alternative syntax, for better compatibility with existing pipe tables:

| Caption Text |                 |                |                |                |                |
|--------------|-----------------|----------------|----------------|----------------|----------------|
|              | Header 1        | Header 2       |        <       | Header 3       |        <       |
|              | Subheader 1     | Subheader 2.1  | Subheader 2.2  | Subheader 3.1  | Subheader 3.2  |
|==============|-----------------|----------------|----------------|----------------|----------------|
| Row Header 1 | 3row, 3col span |       <        |        <       | Colspan only   |        <       |
| Row Header 2 |       ^         |       <        |        <       | Rowspan only   | Cell           |
| Row Header 3 |       ^         |       <        |        <       |       ^        | Cell           |
| Row Header 4 |  Row            |  Each cell     |:   Centered   :| Right-aligned :|: Left-aligned  |
|.            .|. with multiple .|. has room for .|.  multi-line  .|.   multi-line .|. multi-line   .|
|.            .|. lines.        .|. more text.   .|.     text.    .|.        text. .|. text.        .|
A future extension could implement this syntax.

For now, in GFM and other existing Markdown flavors, it falls back to an ordinary table whose cells contain symbols that visually represent the additional features:

rendered-gfm-table-revised
rendered-gfm-table-revised.png
1712×762 60 KB
One row in the rendered table consists of cells containing only dashes - and cells containing only equals signs =. This resembles a “delimiter row”. Cells above this row represent headers and subheaders. Cells below that row represent other table cells.

In the “delimiter row”, if the first cell contains only equals signs =, it indicates that the cells in the first column represent row headers.

A row span is represented by cells containing only a carat/circumflex ^.

A column span is represented by cells containing only a “less-than” symbol <. (Inspired in part by 0x666C697473’s above proposal to use a “greater-than” symbol > for column spans.)

A caption is represented by text in the top-left cell (above all other cells, headers, etc.)

Multi-line cell continuation is represented by row(s) whose cells each start and end with a single dot that is whitespace-separated from the cell’s text content. |. (text) .| Visually, the dots in each column resemble a vertical ellipsis which indicates that the above cell continues downward.

Per-cell text alignment is indicated if a cell (or, in a multiline cell, the first line) starts and/or ends with a colon that is whitespace-separated from the cell’s text content. | (text) :|, |: (text) |, |: (text) :|

In this table, the Markdown text can be compressed to remove extra dashes, equals signs, and whitespace, as long as there remains whitespace next to colons |: :| for alignment and next to dots |. .| for cell continuation.

EDIT: I have revised the above syntax proposal to simplify it. The earlier version was as follows:

|==============|                 |                |                |                |                |
|--------------|-----------------|----------------|----------------|----------------|----------------|
|              | Header 1        | Header 2       |        <       | Header 3       |        <       |
|              | Subheader 1     | Subheader 2.1  | Subheader 2.2  | Subheader 3.1  | Subheader 3.2  |
|==============|=================|================|================|================|================|
| Row Header 1 | 3row, 3col span |       <        |        <       | Colspan only   |        <       |
| Row Header 2 |       ^         |       <        |        <       | Rowspan only   | Cell           |
| Row Header 3 |       ^         |       <        |        <       |       ^        | Cell           |
| Row Header 4 |  Row            |  Each cell     |:   Centered   :| Right-aligned :|: Left-aligned  |
|.            .|. with multiple .|. has room for .|.  multi-line  .|.   multi-line .|. multi-line   .|
|.            .|. lines.        .|. more text.   .|.     text.    .|.        text. .|. text.        .|
|--------------|-----------------|----------------|----------------|----------------|----------------|
| Caption Text |
2 Replies
4

29 days later

urzds
Feb '19
There is a related topic discussing the introduction of figure environments and captions for images: Image tag should expand to figure when used with title 42 It would be nice if a unified caption syntax would emerge from these two topics.




chrisalley

1
Feb '19
It would be nice if a unified caption syntax would emerge from these two topics.

Captions could be added as a table row at the bottom (or perhaps top) of a pipe table. But since an image isn’t made up of rows it wouldn’t make sense for image captions to also use this syntax. However, both transcluded images and CSV table content blocks do share the same caption syntax 17.

1

2 months later

aoudad
Apr '19
Modifying meg’s proposal, here’s a table syntax based on key-value pairs:

title: The Title | name: The Name | ph: The Phone
-|-|-
title: value 1
name:  value 2
ph:    value 3
||
title: value 4
name:  value 5
ph:    value 6
An alternative, using headers as keys:

The Title | The Name | The Phone
-|-|-
The Title: value 1
The Name:  value 2
The Phone: value 3
||
The Title: value 4
The Name:  value 5
The Phone: value 6
A future extension could convert the above code into this table:

GFM-table-2

In current GitHub-Flavored Markdown, it falls back to a table with keys/values in the first column:

GFM-table-1
GFM-table-1.png
788×562 22.7 KB



vas

1
Apr '19
@aoudad: This syntax violates the Prime Directive 139. It’s worth reading the discussion in that thread.

Even though it’s spelled out both by Gruber when he introduced Markdown and by @jgm in the introduction to CommonMark, a lot of ideas and proposals on this forum lose sight of it, and it confuses efforts to solidify and advance this standard. Maybe create a topic titled “The Philosophy and Spirit of Markdown” or “The Markdown Prime Directive”, and pin it to the top of the forum? @jgm, @codinghorror, what do you think? I’d be happy to make the initial post (I’ve been drafting something about this), though it might be best if it came from John. I realize, John, that you’ve already done this in What Is Markdown? 8 Maybe just post and pin that at the top of the forum?

I think it important that the philosophy and spirit stay in the forefront of everyone’s minds as we try to get to v1.0 as well to v1.1 or v2. Any new directions that ditch the original philosophy are fine, but they shouldn’t be called Markdown.

5


aoudad
Apr '19
For better readability, the table could be written as follows:

| The Title | The Name | The Phone |
|-----------|----------|-----------|
| The Title: value 1               |
| The Name:  value 2               |
| The Phone: value 3               |
| ||                               |
| The Title: value 4               |
| The Name:  value 5               |
| The Phone: value 6               |
These additional pipes and dashes make it look more table-like. And as before, it falls back to a valid table in existing GitHub-Flavored Markdown.

The question is, does readability take absolute priority over everything else? If so, “Prime Directive” would seem to be an appropriate metaphor.

I would argue, though, that sometimes readability should be weighed against other considerations. For example, compatibility with existing Markdown flavors is essential to CommonMark’s mission of specifying Markdown. Also, CommonMark needs to respect the Principle of Uniformity 6 (i.e. text content has the same meaning whether or not it is inside a container block), since the spec for lists and block quotes presupposes this principle.

In any case, I like your idea for a topic about the philosophy and spirit of Markdown. This is becoming a longer discussion, and I think it’s worthy of its own place on the forum.

3

3 months later

skystar0227

1
Jul '19
A pipe table syntax with lots of features:

|              | Header 1        | Header 2                       || Header 3                       ||
|              | Subheader 1     | Subheader 2.1  | Subheader 2.2  | Subheader 3.1  | Subheader 3.2  |
|==============|-----------------|----------------|----------------|----------------|----------------|
| Row Header 1 | 3row, 3col span                                 ||| Colspan only                   ||
| Row Header 2 |       ^                                         ||| Rowspan only   | Cell           |
| Row Header 3 |       ^                                         |||       ^        | Cell           |
| Row Header 4 |  Row            |  Each cell     |:   Centered   :| Right-aligned :|: Left-aligned  |
:              :  with multiple  :  has room for  :   multi-line   :    multi-line  :  multi-line    :
:              :  lines.         :  more text.    :      text.     :         text.  :  text.         :
|--------------|-----------------|----------------|----------------|----------------|----------------|
[Caption Text]
I really like this.
My idea is improvement for more machine- and human-readable.

Simple One Rule: Always start from a pipe character (|) for machine-readability.
This rule would avoid conflicts with other syntax.

|######################################## Caption Text ##########################################
|_______________________________________________________________________________________________,
|              | Header 1      || Header 2                     || Header 3                      |
|              | Subheader 1   | Subheader 2.1 | Subheader 2.2 |  Subheader 3.1 | Subheader 3.2 |
|==============|---------------|---------------|---------------|----------------|---------------|
| Row Header 1 ||| 3row, 3col span                             || Colspan only                  |
|______________|                                               |________________|_______________|
| Row Header 2 |^                                              |  Rowspan only  | Cell          |
|______________|                                               |                |_______________|
| Row Header 3 |^                                              |^               | Cell          |
|______________|_______________________________________________|________________|_______________|
| Row Header 4 | Row           | Each cell     |:   Centered  :| Right-aligned :|: Left-aligned |
|~             | with multiple | has room for  |   multi-line  |    multi-line  |  multi-line   |
|~             | lines.        | more text.    |      text.    |         text.  |  text.        |
|______________|_______________|_______________|_______________|________________|_______________/
For human-readability, rule lines are sometimes useful. For machines, however, these have no mean.
So lines starting from |_ can be introduced, which can be ignored like comment lines.

Let’s remove lines starting from |_.

|######################################## Caption Text ##########################################
|              | Header 1      || Header 2                     || Header 3                      |
|              | Subheader 1   | Subheader 2.1 | Subheader 2.2 |  Subheader 3.1 | Subheader 3.2 |
|==============|---------------|---------------|---------------|----------------|---------------|
| Row Header 1 ||| 3row, 3col span                             || Colspan only                  |
| Row Header 2 |^                                              |  Rowspan only  | Cell          |
| Row Header 3 |^                                              |^               | Cell          |
| Row Header 4 | Row           | Each cell     |:   Centered  :| Right-aligned :|: Left-aligned |
|~             | with multiple | has room for  |   multi-line  |    multi-line  |  multi-line   |
|~             | lines.        | more text.    |      text.    |         text.  |  text.        |
I think that it is better to use double or more pipe character BEFORE a table cell.
It makes parser a little easier for the colspan attribute creation.
Also, it allows us omit the last pipe character.

|######################################## Caption Text ##########################################
|              | Header 1      || Header 2                     || Header 3
|              | Subheader 1   | Subheader 2.1 | Subheader 2.2 |  Subheader 3.1 | Subheader 3.2
|==============|---------------|---------------|---------------|----------------|---------------
| Row Header 1 ||| 3row, 3col span                             || Colspan only
| Row Header 2 |^                                              |  Rowspan only  | Cell
| Row Header 3 |^                                              |^               | Cell
| Row Header 4 | Row           | Each cell     |:   Centered  :| Right-aligned :|: Left-aligned
|~             | with multiple | has room for  |   multi-line  |    multi-line  |  multi-line
|~             | lines.        | more text.    |      text.    |         text.  |  text.
Lines starting from |# constitute a caption text.
For this example, I write a caption text before a table because a <caption> element should be the first child of <table> element, but it’s not important.
Like lines for h1, h2, …, enclosing text with # should be allowed but its count does not matter.
The simplest form is |# Caption Text.

Keyword |^ increases rowspan, but no space should be allowed between | and ^ to simplify parser.
This no space rule would be also useful for other keywords.

To describe a row with multiple lines, keyword |~ can be used at the first of subsequent lines, instead of : use as a column separator.
Optionally |~ can be used not only at the first but also at each separator, but the first |~ is required.

Finally, I think that table syntax can be a extension of CommonMark, but I will be happy if it is released as a formal specification!

2 Replies
2

1 month later

juanmirocks
Aug '19
Let’s finish 1.0 69, then we focus on tables!

2


shorty66

skystar0227
Aug '19
Would it be possible to have single row header for multiple rows? For column headers, this is already implied: Header 2 spans multiple subheaders.

In this example, row header 2 would be spanning row 2 and 3:

|######################################## Caption Text ##########################################
|              | Header 1      || Header 2                     || Header 3
|              | Subheader 1   | Subheader 2.1 | Subheader 2.2 |  Subheader 3.1 | Subheader 3.2
|==============|---------------|---------------|---------------|----------------|---------------
| Row Header 1 ||| 3row, 3col span                             || Colspan only
| Row Header 2 |^                                              |  Rowspan only  | Cell
|^             |^                                              |^               | Cell
| Row Header 4 | Row           | Each cell     |:   Centered  :| Right-aligned :|: Left-aligned
|~             | with multiple | has room for  |   multi-line  |    multi-line  |  multi-line
|~             | lines.        | more text.    |      text.    |         text.  |  text.
How would we do sub-rowheaders?
Here is an example with subheaders:

|######################################## Caption Text ##########################################
|               |           |Header 1      || Header 2                     || Header 3
|               |           | Subheader 1   | Subheader 2.1 | Subheader 2.2 |  Subheader 3.1 | Subheader 3.2
|===============|===========|---------------|---------------|---------------|----------------|---------------
|| Row Header 1             ||| 3row, 3col span                             || Colspan only
|| Row Header 2 | subheader1|^                                              |  Rowspan only  | Cell
||^             | subheader2|^                                              |^               | Cell
|| Row Header 4             | Row           | Each cell     |:   Centered  :| Right-aligned :|: Left-aligned
|~                          | with multiple | has room for  |   multi-line  |    multi-line  |  multi-line
|~                          | lines.        | more text.    |      text.    |         text.  |  text.
But i get the feeling that this gets complicated to read & write quickly…




skystar0227
Aug '19
Would it be possible to have single row header for multiple rows?

Yes. Just use |^.
For your example with subheaders, single pipe (|) should be used instead of double pipe (||) before “Row Header 2” and the below cell.

Corrected example:

|######################################## Caption Text ##########################################
|               |            | Header 1      || Header 2                     || Header 3
|               |            | Subheader 1   | Subheader 2.1 | Subheader 2.2 |  Subheader 3.1 | Subheader 3.2
|===============|============|---------------|---------------|---------------|----------------|---------------
|| Row Header 1              ||| 3row, 3col span                             || Colspan only
| Row Header 2  | subheader1 |^                                              |  Rowspan only  | Cell
|^              | subheader2 |^                                              |^               | Cell
|| Row Header 4              | Row           | Each cell     |:   Centered  :| Right-aligned :|: Left-aligned
|~                           | with multiple | has room for  |   multi-line  |    multi-line  |  multi-line
|~                           | lines.        | more text.    |      text.    |         text.  |  text.
“Row Header 1” is a merged 1 x 2 header cell, equal to <th colspan="2">Row Header 1</th>.
“Row Header 2” is a merged 2 x 1 header cell, equal to <th rowspan="2">Row Header 2</th>.
“Row Header 4” is a merged 1 x 2 header cell, equal to <th colspan="2">Row Header 4</th>.
But i get the feeling that this gets complicated to read & write quickly…

Is it because you thought that the new syntax ||^ is needed?




Crissov

2

skystar0227
Aug '19
Row-spanning should allow multiple circumflexes, as in underlined headings and row headers.

| foo | bar |
|-----|-----|
|^^^^^| baz |
|^    |    ^|
| quz | ^^^ |
Column-spans would then naturally use one or more less-than signs.

| foo | bar |<    | <<< |
| --- |-----|-    |    -|
| baz |<<<<<|    <| quz |



amix
Aug '19
Since community input is desired - and most people seem to vote for ‘piped’ tables - I would like to add myself to the list of those, who’d prefer a more simple style.

To me, the “first rule” of Markdown is, as expressed by Gruber (emphasis is mine):

The overriding design goal for Markdown’s formatting syntax is to make it as readable as possible. The idea is that a Markdown-formatted document should be publishable as-is, as plain text, without looking like it’s been marked up with tags or formatting instructions.

To me, that means Pandoc style ‘simple’ and ‘multi line’ tables 60:

Simple Table:

  Right     Left     Center     Default
-------     ------ ----------   -------
     12     12        12            12
    123     123       123          123
      1     1          1             1

Table:  Demonstration of simple table syntax.
Multiline Table:

-------------------------------------------------------------
 Centered   Default           Right Left
  Header    Aligned         Aligned Aligned
----------- ------- --------------- -------------------------
   First    row                12.0 Example of a row that
                                    spans multiple lines.

  Second    row                 5.0 Here's another one. Note
                                    the blank line between
                                    rows.
-------------------------------------------------------------

Table: Here's the caption. It, too, may span
multiple lines.
I would add an optional, numeric sequence to the caption, like in the following example.

ID    Name         Street            ZIP      City         Phone
--    ---------    --------------    -----    ---------    ---------
01    Tom Smith    Main St. 324      49322    Supertown    0233-4545
02    Jane Doe     Upper Rd. 3234    23234    Homeplace    0434-1343

Table 1: Caption (optional, numeric count is also optional)
Getting tables to align correctly is difficult enough already! It seems wisest, to first add each table row, check which is the longest, and then, as a last step, format the rest of the table according to that line.

I would like this kind of table to go into the “core” spec.

Introducing | or :, already contradicts: “without looking like it’s been marked up with tags or formatting instructions” Such tables could be a “blessed extension” To those I would add other, basic, formatting symbols people came up with.

1 Reply
3


alan
Sep '19
Where do we track the status of this being accepted into the standard or refused as a wontfix?

This feature has been talked about a lot and it seems that the time has come to move on with something like GHFM’s use of piped tables and call it good for this version and iterate as needed. Otherwise, it just appears to be stalled as a feature request and years later, we still don’t have tables in the spec.




chrisalley
Sep '19
I don’t believe there was ever a plan to add tables to the core CommonMark spec - it is categorised under “extensions” for this reason. The goal has been to finalise the core feature set first, and then consider extensions at a later point (official extensions not being a concrete plan either). GitHub Flavoured Markdown is a well supported third party extension of the core CommonMark spec which can be used today.

1


alan
Sep '19
Pushing this feature to an extension causes it to not be a feature of the core and then not be supported by folks who are only supporting core. It creates a lot of downstream confusion.

How would a group of folks lobby for this to be pushed into core?

2


amix
Sep '19
How would a group of folks lobby for this to be pushed into core?

I also would be interested in this.

Tables are so common in many forms of literature, that they belong into the core.

That is also, why I vote for the simple table type, since that is unobtrusive and totally matches the Ur-Markdown spirit, IMO.

1


Crissov
Sep '19
Well, tables wonʼt be part of the v1.0 core specification, but it should probably take care of special rules for the pipe character | already: CM-based GHFM requires it to be backslash-escaped even inside code spans in tables.

As soon as we have agreed upon a common way to document extensions, people should start to write specifications for their pet features which are at least as detailed as the core specification, so @jgm does not have to take care of everything.

2

1 year later

xenoterracide
Dec '20
Although at this point I don’t actually believe that CommonMark is going to truly improve the markdown markup. And I’m sure this has been commented on before, we at least need a way to do multiline markup, so that the raw markdown continues to be readable if a cell has a long line. I don’t really care how it’s accomplished.
