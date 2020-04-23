---
type: quick-start
---

# Clone repositories

The first step is to clone all the repositories needed to create the content,
and run the code preview.

## Clone the content

All the content for the developer documentation guides resides in the
[`developer.box.com`](https://github.com/box/developer.box.com) repository on
GitHub.

The content can be cloned using Git.

```bash
git clone git@github.com:box/developer.box.com.git
```

<Message>You will need to have an SSH key set up to download from GitHub using
this method</Message>

All the guides for this repository can be found in the `content/guides` folder.

```yaml;highlight=2
- content
  - guides
  - pages
  - microcopy
```

## Clone the documentation framework

To get an accurate preview of the guide we want to write, we need to download
the framework used for our developer documentation.

```bash
git clone git@github.com:box/developer.box.com-framework.git
```

<Message>You will need to have access to this repository to clone it, as it's
currently not a public repository.</Message>

<Next>I've downloaded the repositories</Next>