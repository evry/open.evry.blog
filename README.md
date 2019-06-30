## Welcome to open.evry.blog

You can use the [editor on GitHub](https://github.com/evry-bergen/open.evry.blog/edit/master/README.md) to maintain and preview the content of Markdown formatted posts.

Whenever a change is merged to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the blog, from the content in the Markdown formatted posts and pages.

### Structure

| Path         | Description |
|--------------|-------------|
| `_layouts`   | Contains HTML layout template for rendering markdown content |
| `_posts`     | Contains markdown formatted blog posts, see [#posts](#posts) for more information |
| `assets/css` | CSS overrides for the theme; this will be compiled automatically |
| `CNAME`      | Custom domain for GitHub Pages |
| `_config.yml` | [Jekyll configuration file](https://jekyllrb.com/docs/configuration/default/) |
| `docker-compose.yml` | Docker Compose setup for [running locally](#local-development) |

### Posts

All posts are located in the `_posts` directory using [Markdown](#markdown) and [Jekyll Front Matter](https://jekyllrb.com/docs/front-matter/). You can just look at existing blog posts to see the structure, but a simple example looks like this:

```yaml
---
layout: post
title: The Hacker Mindset
---

Content goes here!
```

### Local Development

For running the blog locally for development purposes you only need Docker and Docker Compose installed. Clone the repository and run the following command from the repo root:

```
$ docker up
```

The blog will be accessible over port `:4000` and reload automatically on code changes. 

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

This Blog uses the layout and styles from the Jekyll theme you have selected in your the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [open an issue](./issues) and we’ll help you sort it out.
