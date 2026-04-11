(Needs proofreading! Written by: Dhruva)

# Contributing

The GitHub repository containing all of the markdown files that make up this
book and more can be found by clicking the GitHub icon in the top right.
From there, you can make changes just like any other Git repository.
If you are not familiar with Git, you may find it easier to use the web editor
in GitHub, which should be sufficient for most purposes - just click the "Add File"
button or click the edit button while viewing a specific file.

## What's in the repository?

An mdBook project contains a couple things.

1. The `src` folder contains all of the markdown files used to build this book.
2. The `theme` folder contains overrides for the theme. The is only used right now to override the `highlight.js` file.
3. The `book.toml` file contains a few configuration options for the book as a whole.
4. `src/SUMMARY.md` is a special file which specifies how all of the pages are organized.

## How to add or move pages

The `src/SUMMARY.md` file must be edited in order to add new chapter to the book or
change the ordering and organization of the chapters. The file looks something like this:

```markdown
# Summary

- [Name of Chapter 1](path/to/page_1.md)
- [Name of Chapter 2](path/to/page_2.md)
  - [This is a sub-chapter](chapter/sub_chapter.md)
    - [You can go further too](source_file.md)
```

If you're familiar with markdown, you probably recognize that this is just a bulleted
list of links. (If not, don't worry! It's quite simple and there are some resources below to
help you get started.) Simply add the name of the page you want to the list in brackets and
then the path to the file in parantheses after in order to add another section.

## Writing in markdown

Markdown (.md file extension) is a markup language used to style documents.
You have likely already seen it in README.md files on GitHub, Discord messages,
or somewhere else. It can do things such as **bold** or _italicize_ text, add images,
links, headings, code blocks, and more.

To get started, looking at a cheat sheet such as [this one](https://www.markdownguide.org/cheat-sheet/)
will show you how to format common things. Please **do not** write out code or equations in plain text.

Code can be written in `short code blocks` or longer ones like this:

```java
void myFunction() {

}
```

The markdown syntax for that code block is:

````markdown
```java
void myFunction() {

}
```
````

Additionally, mdBook supports several extensions to the markdown syntax which may be useful.
The full list of supported extensions can be found
[in mdBook's official documentation](https://rust-lang.github.io/mdBook/format/markdown.html#extensions).
Some of the supported features include tables and admonitions, which can be seen below:
|This is a | Table |
|----------|-------|
|Row 1 | Row 2 |

> [!NOTE]
> This is an example of an admonition.

## Adding equations

Rendering equations using LaTeX is supported in mdBook using MathJax.
This must be first enabled in the `book.toml` file, which has already been done.
If you are not familiar with LaTeX it looks a bit confusing but it uses backslashes to
insert special characters or functions into an equation. A cheat sheet can be found
[here](https://quickref.me/latex.html) and a longer guide can be found
[here](https://www.overleaf.com/learn/latex/Mathematical_expressions) (although it's more in the
context of using LaTeX for a whole document).

Wrap inline equations in `\\( \\)` and equations that need their own line in `\\[ \\]`. For example, this:

```markdown
\\( F_f = \mu \times F_N \\)
```

Looks like:
\\(F_f = \mu \times F_N \\)

## Linking to other parts of the documentation

Inserting hyperlinks in markdown is done using this syntax: `[display text](https://example.com)`.
The same syntax is used to link to other parts of the book similar
to how you would link to other parts of a normal website. Instead of a URL,
use the local file path or the filename of the page you want to link to.
If you are linking to a page found in the same folder as the current page, then simply use the filename. Otherwise, you would need to cd back into the `/src` folder using `../` and then follow it up with the path to the destination (you may need to write `../` more than once).
For example, this page is located at `src/about/contributing.md`. If you wanted to provide a link to `about.md`,
which is found in the same folder, then you would write `[about page](about.md)`.

If I wanted to link to the inventory page, which is found under `/src/parts/`, then I would write `[inventory page](../parts/inventory.md)`.
You can either use the markdown file ending (.md) or swap it out for .html, since an HTML file of the same name will be generated
when the book is built.

If you want to link to a specific section of a page, you can do that by adding
a '#' after the file and then the name of the section, converted to `kebab-case`
with special characters like '.' removed.

This is the same as the end of the URL you see in your browser when you visit
a certain page of the docs, so you can also just copy everything after the `#`
and use that to link to a specific section. For example, the link to this
section is `https://cryptohawks8726.github.io/docs/about/contributing.html#linking-to-other-parts-of-the-documentation`,
so to link to this section you would use `contributing.html#linking-to-other-parts-of-the-documentation`.

You can also use the full URL without the domain name, for example, `/docs/about/contributing.html`.
However, this will not work if the site is run locally or deployed somewhere other than /docs, so
it is recommended to use the local file paths instead.

## What's with `highlight.js`?

As mentioned earlier, the files in the `theme` folder overwrite
certain elements of the theme. The `highlight.js` file is responsible for syntax
highlighting in code blocks, and for some reason the default `highlight.js` file
only supports a small range of languages. It is notably missing support for Dart.

Because of this, it has been swapped out for a replacement file downloaded
from [the highlight.js library's website](https://highlightjs.org/download)
which supports many more languages.

## Building and Deploying

Anytime you make changes to the repository's `main` branch, GitHub actions will automatically
rebuild the book and deploy it to the GitHub Pages site, so you do not need to do anything
manually for your changes to appear on the website. This is configured in `.github/workflows/deploy.yml`,
which has been copied from [mdBook's automated deployment examples](https://github.com/rust-lang/mdBook/wiki/Automated-Deployment%3A-GitHub-Actions).
This tells GitHub to run a few commands to build and then deploy the book to the site.

If you want to build the book locally to test it out, you can install mdBook's command line tool
and run `mdBook build`, which will build a website in a folder titled `book`. Running `mdbook serve` will
host the website at localhost:3000. For more information and installation instructions,
see [mdBook's user guide](https://rust-lang.github.io/mdBook/guide/installation.html).

[this is a test](../parts/sparkmax.md)
[this is another test](about.md)
