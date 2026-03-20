# Faraz Mazhar - Portfolio Site

This is the source code for my personal portfolio website, designed to be hosted seamlessly on [GitHub Pages](https://farazmazhar.github.io). 

To keep the project clean and easily maintainable, the HTML layout is separated from the content. It leverages **Jekyll**, the default static site generator natively supported by GitHub Pages, allowing the content to be parameterized via a simple YAML configuration file.

## 🛠️ Modifying the Content

To update the text on the site (e.g., adding past experience, adding a new project, or fixing a typo), simply edit the `_data/profile.yml` file. 

You **do not** need to touch the `index.html` file unless you are making structural modifications to the layout or the Tailwind CSS configuration. The Liquid loops in the HTML specifically fetch their data from definitions inside the `profile.yml` file.

---

## 💻 Testing Locally

To see your changes live on your machine before pushing them to the internet, you can spin up the local Jekyll preview server.

### 1. Install System Dependencies
Since you use Fedora, ensure your system has Ruby and its required build tools by running:

```bash
sudo dnf install ruby ruby-devel zlib-devel make gcc
```

### 2. Configure & Install Jekyll
To avoid permission errors, configure your local gem paths and install the Jekyll tooling locally:

```bash
# Setup paths (you only need to run this block once)
echo '# Install Ruby Gems to ~/gems' >> ~/.bashrc
echo 'export GEM_HOME="$HOME/gems"' >> ~/.bashrc
echo 'export PATH="$HOME/gems/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc

# Install the dependencies
gem install jekyll bundler
```

> **Note:** Depending on your shell environment, you may need to add ruby's local package directory to your path. If `jekyll serve` fails, run this first: `export PATH="$HOME/.local/share/gem/ruby/bin:$HOME/.gem/ruby/$(ruby -e 'print RUBY_VERSION.shellsplit[0..2].join(".")')/bin:$PATH"`

### 3. Spin up the Server
Navigate to the root directory of this repository (`farazmazhar.github.io`) and execute:

```bash
jekyll serve --livereload
```

You can now visit your local site at **`http://127.0.0.1:4000`**. Every time you press save on your YAML file, the page in your browser will automatically refresh!

---

## 🚀 Deployment

Deployment requires virtually zero effort thanks to GitHub Pages. Because this repository is named `farazmazhar.github.io`, pushing to the default branch triggers an automatic internal Jekyll build on GitHub's servers without any extra configuration needed.

When you are satisfied with your local changes:

1. Stage your modifications:
   ```bash
   git add _data/profile.yml
   ```
2. Commit the changes:
   ```bash
   git commit -m "Updated portfolio details and recent projects"
   ```
3. Push to GitHub:
   ```bash
   git push
   ```

Within a matter of minutes, GitHub Actions will compile the source, inject your YAML data deeply into the `index.html` template, and serve the updated portfolio live for the world to see!
