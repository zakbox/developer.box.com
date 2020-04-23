---
hide: true
---

<!-- does not need translation -->

# Code Blocks

<!-- markdownlint-disable code-fence-style -->

Not all code samples exist in the SDK or CLI documentation. You can add new code
samples using standard Markdown back ticks.

~~~sh
```js
console.log('Hello, World!')
```
~~~

<H>

```js
console.log('Hello, World!')
```

</H>

<Message>
  Please make sure to add a valid language to every code block to ensure
  appropriate syntax highlighting is applied.
</Message>

## Highlighting lines

It is possible to highlight some lines in code blocks. This follows the
[Prism line highlight syntax](https://prismjs.com/plugins/line-highlight/).

~~~sh
```yaml;highlight=2,4
- A yaml list
- This one is highlighted
- This one looks as usual
- And this one is highlighted too
```
~~~

<H>

```yaml;highlight=2
- A yaml list
- This one is highlighted
- The rest looks as usual
- Just plain and simple
```

</H>

<!-- markdownlint-enable code-fence-style -->
