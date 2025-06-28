# Markdown Examples

A collection of practical Markdown examples for common formatting needs, documentation, and technical writing.

## Headings

```markdown
# Heading 1
## Heading 2
### Heading 3
#### Heading 4
##### Heading 5
###### Heading 6
```

## Emphasis

```markdown
*Italic* or _Italic_
**Bold** or __Bold__
***Bold and Italic***
~~Strikethrough~~
```

## Lists

### Unordered List
```markdown
- Item 1
- Item 2
  - Subitem 2.1
  - Subitem 2.2
- Item 3
```

### Ordered List
```markdown
1. First item
2. Second item
   1. Subitem 2.1
   2. Subitem 2.2
3. Third item
```

### Task List
```markdown
- [x] Completed task
- [ ] Incomplete task
- [ ] Another task
```

## Links

```markdown
[Inline Link](https://www.example.com)
[Reference Link][1]

[1]: https://www.example.com
```

## Images

```markdown
![Alt text](https://via.placeholder.com/150 "Optional title")
```

## Blockquotes

```markdown
> This is a blockquote.
> 
> - It can span multiple lines.
> - It can include lists and other Markdown elements.
```

## Code

### Inline Code
```markdown
Use `code` for inline code.
```

### Code Block
```markdown
```
function helloWorld() {
  console.log('Hello, world!');
}
```

### Syntax Highlighting
```markdown
```python
def hello():
    print("Hello, Markdown!")
```
```

## Tables

```markdown
| Name     | Age | City      |
|----------|-----|-----------|
| Alice    |  30 | New York  |
| Bob      |  25 | London    |
| Charlie  |  35 | San Francisco |
```

## Horizontal Rule

```markdown
---
```

## Escaping Characters

```markdown
\*This text is not italic\*
```

## Footnotes

```markdown
Here is a footnote reference.[^1]

[^1]: This is the footnote.
```

## Task Lists (GitHub Flavored)

```markdown
- [x] Write the proposal
- [ ] Review the code
- [ ] Submit the report
```

## Emoji (GitHub Flavored)

```markdown
:smile: :rocket: :+1:
```

## HTML in Markdown

```markdown
<b>Bold HTML</b><br>
<i>Italic HTML</i>
```

## Advanced: Collapsible Sections (GitHub)

```markdown
<details>
  <summary>Click to expand!</summary>

  Hidden content here.
</details>
```

## Advanced: Mermaid Diagrams (GitHub)

```markdown
```mermaid
graph TD;
  A-->B;
  A-->C;
  B-->D;
  C-->D;
```
```

These examples cover the most common and advanced Markdown features for documentation, READMEs, and technical writing. 