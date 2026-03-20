# Portfolio Maintenance Instructions

This document is a quick-start guide for easily updating your live website content without needing to dig into raw HTML files or Tailwind CSS configurations.

## How to Edit Your Resume/Profile Details

Your site separates its content from its design using Jekyll. You do not need to know HTML to add a new project or change your resume link!

All of the website's text and links live neatly organized inside **`_data/profile.yml`**.

1. **Open `_data/profile.yml`** in any text editor.
2. Locate the section you want to change:
   - **hero:** This is the top section introducing you and containing the headline.
   - **experience:** Contains a timeline list of your past roles. To add a new job, simply copy any block starting from `- year:` inside the file and paste it at the top or bottom of the list, modifying the nested `title`, `company`, etc.
   - **skills:** Modifies your core tech stack blocks. Note: The icons correspond to [Google Material Symbols](https://fonts.google.com/icons). You can browse that site to change icons!
   - **projects:** Changes the Flagship Projects list.
   - **contact & footer:** Configures your social links, email address, and the copyright tagline.
3. Save the file.

---

## 📄 Uploading a New Resume PDF

When you update your physical resume in the future:

1. Save your newest resume PDF into the **`assets/`** folder of this repository (for instance, `Resume_2026.pdf`).
2. Open `_data/profile.yml`.
3. Locate `resume_url:` under the `hero` section and change it to point to your new file path exactly like this:
   ```yaml
   hero:
     resume_url: "assets/Resume_2026.pdf"
   ```

---

## 🚀 Deploying Your Changes to GitHub Pages

Once you have modified and saved the `profile.yml` file, publishing the changes to the internet requires just these three Git commands in your terminal:

```bash
# 1. Stage the files you changed
git add _data/profile.yml assets/

# 2. Package your changes with a short descriptive message
git commit -m "Updated experience section and resume PDF"

# 3. Securely upload the update to GitHub
git push
```

Within a minute or two of the push completing, GitHub will automatically re-render your Jekyll site entirely in the cloud and instantly display the updates live at `https://farazmazhar.github.io`!
