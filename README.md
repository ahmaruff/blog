# Blog

This repository contains my personal blog built with Hugo and deployed using GitHub Pages.

Purpose:  
- Writing technical notes
- Documenting learning progress
- Sharing small experiments & thoughts

## Getting Started

### Requirements:  
- Hugo Extended
- Git

Check  

```
hugo version
```

### Run locally

```
git clone https://github.com/ahmaruff/blog.git
cd blog
hugo server -D
```

Open:

```
http://localhost:1313
```

# Writing a New Post

Folder structure (Multi language)

```
content/
  writing/
    notes.md      → Indonesian (default)
content.en/
  writing/
    notes.md      → English version

```

Rules:  
- Indonesian → content/
- English → content.en/
- Slug is taken from filename
- Both files must have same name for translation mapping

### Create new post

```
hugo new writing/my-post.md
hugo new content.en/writing/my-post.md
```

Frontmatter Example

```
+++
title = "Post Title"
date = 2025-01-10T10:00:00+07:00
draft = false
menus = "writing"
tags = ["php", "backend"]
+++
```

## Multi Language

Supported:
- id (default → content/)
- en (content.en/)

Hugo will automatically map translation based on:
- Same filename
- Same folder structure


# Customization

### Main config

Edit

```
hugo.toml
```
Things you can customize:
- Languages
- Menus
- Params
- BaseURL
- Theme

### Archetypes (post template)

```
archetypes/
  default.md
```
Used when creating new post:

```
hugo new writing/post.md
```

### Layouts (override theme)

```
layouts/
```
Use this folder if you want to:
- Override theme layout
- Customize list page
- Customize single post view

# Upload images

Put your images here:  
```
static/images/
```
Use in markdown:  
```
![alt text](/images/example.png)
```

# Deployment

## 1. Automatic (GitHub Actions)

Workflow:  
```
.github/workflows/hugo.yaml
```

Flow:

1. Push to main
2. Action:
    - Install Hugo
    - Build site
    - Deploy to GitHub Pages

## 2. Manual deploy

Build manually:  
```
hugo --minify
```

Output folder:
```
public/
```

Then push public/ to your GitHub Pages branch:
- gh-pages
- or /docs (depends on repo setting)

OR manually:

1. Copy public/
2. Push to branch used by GitHub Pages

# Folder Structure

```
content/
content.en/
static/images/
layouts/
archetypes/
themes/
.github/workflows/
hugo.toml
```

# Notes

- Main language: Indonesian
- English is secondary
- Slug based on filename → better use English filename for SEO
