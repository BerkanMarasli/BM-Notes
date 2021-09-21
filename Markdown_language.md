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

## Links

'{[description]}{(URL)}'

[Google](https://google.co.uk)

## Code

'{text}{open backtick}{code}{close backtick}{text}' : inline code

`variable = fetch()`

'{open triple backticks}{code language}{new line}{code}{new line}{close triple backticks}' : block code 

``` javascript
await response = fetch('http://google.co.uk')
```

```html
<h1>
  Hello World!
</h1>
```



