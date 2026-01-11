# Personal Blog & Portfolio

This repository contains my personal blog and portfolio, built with [Hugo](https://gohugo.io/) and the [Bear Cub](https://github.com/clente/hugo-bearcub) theme. It's automatically deployed to GitHub Pages.

**Purpose:**
- Writing technical notes
- Documenting my learning journey
- Showcasing projects and experiments

---

## Local Development

### Prerequisites
- [Hugo Extended](https://gohugo.io/installation/) (check version with `hugo version`)
- [Git](https://git-scm.com/downloads)

### Setup & Running
1.  **Clone the repository with the theme submodule:**
    ```sh
    git clone --recurse-submodules https://github.com/ahmaruff/blog.git
    ```
    *If you've already cloned it without the submodule, run `git submodule update --init --recursive`.*

2.  **Navigate to the project directory:**
    ```sh
    cd blog
    ```

3.  **Start the local Hugo server:**
    ```sh
    hugo server -D
    ```
    The `-D` flag builds draft posts as well. Your site will be available at `http://localhost:1313`.

---

## Content Creation

The site supports two languages: Indonesian (default) and English.

- **Indonesian Content:** Goes into the `content/` directory.
- **English Content:** Goes into the `content.en/` directory.

For Hugo to link translations, the content files **must have the same filename and be in the same sub-directory** (e.g., `content/writing/my-post.md` and `content.en/writing/my-post.md`). It's recommended to use English for filenames for cleaner URLs.

### 1. Writing (Blog Posts)

Standard blog posts for technical notes and articles.

**Create New Post:**
```sh
# For Indonesian version
hugo new writing/my-cool-post.md

# For English version
hugo new content.en/writing/my-cool-post.md
```

**Frontmatter Example:**
```toml
+++
title = "My Cool Post"
date = 2025-01-10T10:00:00+07:00
draft = false
tags = ["php", "backend", "learning"]
+++

Your content starts here...
```

### 2. Portfolio (Project Pages)

Specialized pages to showcase your projects. These have a different layout that includes a featured image and a summary/detail view.

**Create New Portfolio Page:**
```sh
# For Indonesian version
hugo new portfolio/my-awesome-project.md

# For English version
hugo new content.en/portfolio/my-awesome-project.md
```

**Frontmatter & Content Structure:**
The portfolio archetype uses a `<!--more-->` separator to distinguish the summary (for the list page) from the full content (for the single page).

```toml
+++
title = "My Awesome Project"
date = 2025-01-11T11:00:00+07:00
draft = false
featured_image = "/images/portfolio/project-banner.png" 
tags = ["integration", "chatbot"]
+++

This is the **rich summary** of the project. It will appear on the main portfolio page.
You can use markdown here.

<!--more-->

This is the **main, detailed content** for the single project page. Everything after the `<!--more-->` tag will only be visible when a visitor clicks through to the project page itself.
```

### Managing Images

- Place all images in the `static/images/` directory. You can create subfolders for organization (e.g., `static/images/portfolio/`).
- Link to them in your Markdown from the root, like this:
  ```markdown
  ![An example image](/images/my-image.png)
  ```

---

## Customization

You can customize the site without touching the original theme files by modifying files in the root directories.

- **Main Configuration (`hugo.toml`):** Edit this file to change site-wide settings like the title, baseURL, languages, menus, and other parameters.
- **Styling (`assets/light.css`):** This site uses a custom stylesheet that overrides the theme's default. To change the site's appearance, edit `assets/light.css`.
- **Layouts (`layouts/`):** This directory overrides the theme's layouts. For example, `layouts/portfolio/list.html` and `layouts/portfolio/single.html` are custom layouts created specifically for the portfolio section. If you want to change how a page type is rendered, you can copy the original layout from `themes/hugo-bearcub/layouts/` to this directory and modify it.
- **Post Templates (`archetypes/`):** These files (`default.md`, `portfolio.md`) are the templates used when you run `hugo new ...`. You can edit them to change the default frontmatter for new content.

---

## Deployment

Deployment is handled **automatically** by a GitHub Actions workflow defined in `.github/workflows/hugo.yml`.

**Workflow:**
1.  **Push** changes to the `main` branch.
2.  The GitHub Action triggers automatically.
3.  It installs Hugo, builds the site into the `public/` directory, and deploys it to GitHub Pages.

### Manual Deployment
If you need to deploy manually:
1.  Build the site:
    ```sh
    hugo --minify
    ```
2.  The output will be in the `public/` directory. You can then push the contents of this directory to the `gh-pages` branch of your repository.

---

## Key Directory Structure

```
.
├── archetypes/       # Content templates (for hugo new)
├── assets/           # Site assets, like custom CSS
├── content/          # Default language (Indonesian) content
├── content.en/       # English language content
├── layouts/          # Custom layouts that override the theme
├── static/           # Static files like images, fonts, etc.
├── themes/           # Git submodule for the hugo-bearcub theme
├── .github/          # GitHub Actions workflows for deployment
└── hugo.toml         # Main site configuration
```
