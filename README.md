# Personal Website

A personal website built with [Hugo](https://gohugo.io/) using the [Stack theme](https://github.com/CaiJimmy/hugo-theme-stack) by CaiJimmy.

## Features

- **Landing Page**: Welcome page with introduction
- **Blog Section**: Card-style blog layout for sharing thoughts and articles
- **Contact Page**: Contact information and social links
- **GitHub Pages Deployment**: Automated CI/CD pipeline using GitHub Actions

## Prerequisites

- [Hugo Extended](https://gohugo.io/installation/) (required for Stack theme)
- Git

## Installation

### Install Hugo

On macOS with Homebrew:
```bash
brew install hugo
```

Or download from the [official Hugo website](https://gohugo.io/installation/).

### Setup the Project

This project is already initialized locally. The Stack theme is included as a Git submodule.

If you need to initialize the theme submodule (e.g., after cloning from GitHub), run:

```bash
git submodule update --init --recursive
```

### Push to GitHub

1. Create a new repository on GitHub (e.g., `nityac-hugo`)
2. Add the remote and push:

```bash
git remote add origin https://github.com/YOUR_USERNAME/nityac-hugo.git
git branch -M main
git add .
git commit -m "Initial commit"
git push -u origin main
```

## Local Development

1. Start the Hugo development server:
```bash
hugo server
```

2. Open your browser and navigate to `http://localhost:1313`

3. The site will automatically reload when you make changes to content or configuration files.

## Project Structure

```
.
├── config.toml          # Hugo configuration
├── content/             # Site content
│   ├── _index.md       # Landing page
│   ├── contact.md      # Contact page
│   └── posts/          # Blog posts
├── themes/             # Hugo themes
│   └── hugo-theme-stack/  # Stack theme (Git submodule)
├── .github/
│   └── workflows/
│       └── hugo.yml    # GitHub Actions CI/CD workflow
└── README.md
```

## Adding Content

### Create a New Blog Post

```bash
hugo new posts/my-new-post.md
```

Edit the generated file in `content/posts/my-new-post.md` and add your content.

### Update Contact Information

Edit `content/contact.md` to update your contact details.

### Update Site Configuration

Edit `config.toml` to customize:
- Site title and description
- Base URL (update with your GitHub Pages URL)
- Menu navigation
- Theme parameters

## Deployment

This site is automatically deployed to GitHub Pages using GitHub Actions.

### Setup GitHub Pages

1. Push your code to a GitHub repository
2. Go to your repository Settings → Pages
3. Under "Source", select "GitHub Actions"
4. The workflow will automatically build and deploy your site on every push to the `main` branch

### Update Base URL

Before deploying, update the `baseURL` in `config.toml`:

```toml
baseURL = 'https://YOUR_USERNAME.github.io/REPOSITORY_NAME/'
```

If your repository is named `nityac-hugo`, it would be:
```toml
baseURL = 'https://YOUR_USERNAME.github.io/nityac-hugo/'
```

## Theme

This site uses the [Stack theme](https://github.com/CaiJimmy/hugo-theme-stack) by CaiJimmy, a card-style Hugo theme designed for bloggers.

For theme documentation and customization options, visit the [Stack theme repository](https://github.com/CaiJimmy/hugo-theme-stack).

## License

This project is open source and available under the [MIT License](LICENSE).

