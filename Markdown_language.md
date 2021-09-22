# Markdown Language Syntax

## Headers

'#{space}{text}' : indicates header h1

# Header h1

'##{space}{text}' : indicates header h2

## Header h2

'###{space}{text}' : indicates header h3

### Header h3

and so on... '####{space}{text}', '#####{space}{text}', etc.

## Paragraphs/Text

Multiple lines should be seperated with an empty line

Line 1

{empty line}

Line 2

'>{space}{text}' : indicates block quote 

> Blockquote text here...

'>> {text}' : indicates indented block quote

> Blockquote Level 1
>
> > Blockquote Level 2
> >
> > > Blockquote Level 3

'** {withoutspace}{text}{withoutspace} **' or use __: indicates bold

The text is **bold!** .

'* {withoutspace}{text}{withoutspace} *' or use __: indicates italic

The text is *italic!* .

'_ {withoutspace} ** {withoutspace}{text}{withoutspace} ** {withoutspace} _' : indicates bold and italic

_**The text is bold and italic!**_

'~~ {withoutspace}{text}{withoutspace} ~~'

The text is ~~stroke through~~.

Adding backslash before * / *** / ~ to escape

## Bullet Points

'-{space}' or '+{space}' or '*{space}' : bullet point level 1

- Using '-'

+ Using '+'

* Using '*'

'{space}{space}-/+/*{space}' : bullet point level 2 (further indented bullet point) => WRONG?

Bullet points can be nested

- Level 1
    - Level 2
        - Level 3

'{number}.{space}' : add level using 1.

- Level 1
    1. Level 2
    2. Level 2
        - Level 3
        - Level 3

'- [ ]' : indicate checkbox (place x inside [ ] to indicate default as ticked)

- [ ] Checkbox 1

- [x] Checkbox 2

## Links

'{[description]}{(URL)}'

[Google](https://google.co.uk)

## Code

'{text}{open backtick}{code}{close backtick}{text}' : inline code

`variable = fetch()`

'{open triple backticks}{code language}{new line}{code}{new line}{close triple backticks}' : block code 

``` javascript
const response = await fetch('http://google.co.uk')
```

```html
<h1>
  Hello World!
</h1>
```

## Images

'!{[alt text]}{(URL)}'

![Random Image](http://picsum.photos/200/200)

Images can be linked to a website by wrapping syntax above in {[image syntax]}{({URL})}

## Tables

Tables are not officially supported but Github and other rendering websites will support tables.

'| {heading 1} | {heading 2} | {heading3}' |

'| --- | --- | --- |'

'| {content 1} | {content 2} | {content 3} |'

'| {more content 1} | {more content 2} | {more content 3} |'

'| :--- |' : text is left-aligned, '| ---: |' : text is right-aligned, '| :---: |' : text is centre-aligned

| Heading |    Header    | Head |
| :-----: | :----------: | :--: |
| Content | More content | Text |

## Section Drop Down

```markdown
<details>
	<summary>Section Header</summary>
	Section body text
</details>
```

<details>
    <summary>Section Header</summary>
    Section body text
</details>

## Colour

To add colour, inline HTML inside Markdown.

```markdown
<span style='color: lightblue'>Some **light blue** markdown text!</span>
```

<span style='color: lightblue'>Some **light blue** markdown text!</span>

