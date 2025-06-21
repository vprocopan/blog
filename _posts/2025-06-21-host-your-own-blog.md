Hosting a blog with **Jekyll** using **GitHub Actions** is a powerful way to automate your static site deployment to GitHub Pages. Here's a **step-by-step guide** to help you do just that.

---

## ✅ Prerequisites

1. **GitHub repository** (`<your-username>/<your-repo>`)
2. **Jekyll blog** source code (use `jekyll new` or clone an existing theme)
3. **GitHub Pages** enabled in the repository settings
4. Optional: A **custom domain** set up via DNS

---

## 🧱 Directory Structure

Make sure your repo has the Jekyll site source in the root or a specific folder (`src`, etc.):

```bash
.
├── _config.yml
├── _posts/
├── index.md
├── ...
└── .github/
    └── workflows/
        └── jekyll.yml
```

---

## ⚙️ GitHub Actions Workflow

Create `.github/workflows/jekyll.yml` with this content:

```yaml
name: Build and Deploy Jekyll Blog

on:
  push:
    branches:
      - main  # or 'master' if that's your default

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4

      - name: Setup Ruby
        uses: ruby/setup-ruby@v1
        with:
          ruby-version: 3.1  # or your desired version
          bundler-cache: true

      - name: Install Dependencies
        run: |
          gem install bundler
          bundle install

      - name: Build Site
        run: bundle exec jekyll build

      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v4
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./_site
```

---

## 📦 Gemfile

Make sure you have the necessary dependencies in your `Gemfile`:

```ruby
source "https://rubygems.org"
gem "jekyll"
gem "github-pages", group: :jekyll_plugins
```

Then run:

```bash
bundle install
```

Commit the `Gemfile.lock`.

---

## 🌐 GitHub Pages Settings

1. Go to your repo: **Settings → Pages**
2. Source: Select `GitHub Actions` as the build and deployment method

---

## 🌍 Optional: Use a Custom Domain

If using a custom domain:

* Create a `CNAME` file in the root of your repo with your domain name.
* Ensure DNS is pointed to GitHub Pages servers (e.g., `yourusername.github.io` CNAME or A records).

---

## 🧪 Test Locally

Before pushing, run locally:

```bash
bundle exec jekyll serve
```

---

## 🚀 Push and Watch

Push to the `main` branch and watch the Actions tab for your deployment to go live.

---
