---
title: "Building this logbook : Astro and GitHub"
date: 2026-11-15
draft: false
---
This is the first project entry of the logbook: building the logbook itself. I built it step by step with an AI assistant that explained each command. I wrote down what went wrong along the way.

## What I wanted to do

A simple place to publish my learning journal. It had to be minimal (a list of dates and titles, dark mode), cheap, and something I could understand instead of copying a ready-made template.

I started from zero on the web side. I had seen blogs built from Markdown files stored in Git, so I looked at a few options. A friend uses Hugo with Amazon S3 and CloudFront. A video I watched used Obsidian, Hugo and a paid host. I chose Astro with GitHub Pages because it is free, readable for a beginner, and easy to move to AWS later.

**In scope:** a home page listing entries, one page per entry, a dark theme, automatic publishing.
**Out of scope for now:** custom domain, AWS, RSS feed, Obsidian, comments, analytics.

## What I did

The plan had six steps:

1. Install Git, Node.js and VS Code.
2. Create the GitHub repository and an empty Astro project.
3. Write the home page in plain HTML, then add the CSS.
4. Store entries as Markdown files and generate the list from them.
5. Add a layout and one page per entry.
6. Publish with GitHub Actions.

Astro turns Markdown files into web pages. Each entry is a `.md` file with a small header (title, date, draft or not). Astro reads these files as a "content collection", checks that each one has the right fields, and builds the list on the home page, newest first. A shared layout holds the menu and the page frame, so I do not repeat it in every page.

GitHub Pages hosts the result. A GitHub Actions file builds the site and publishes it every time I push my code. Writing an entry now means: create a Markdown file, run `git add`, `git commit`, `git push`, and wait about two minutes.

## What went wrong

### Setup

- **Git refused my name.** The error was `key does not contain a section`. I had replaced the setting name in `git config --global user.name "..."` instead of only the text in quotes.
- **PowerShell blocked npm.** Windows disables scripts by default. `Set-ExecutionPolicy -Scope CurrentUser RemoteSigned` allows them for my user only.
- **Wrong repository name.** I had created `jrnl.github.io`, but my account is `jlnrcs`. GitHub Pages builds the address from the account name, so the repository had to be `jlnrcs.github.io`. I renamed it.

### Content

- **Entries not updating.** I deleted the example entry but it stayed on the page, and my edits did not show. The content folder had the wrong name and place, and Astro kept an old cache in its `.astro` folder.
- **Astro could not read my entry.** The error said the title was required and the date had the wrong type, although both were written. Astro could not read the header block. My own template also contained `AAAA-MM-JJ`, which is not a date.
- **The day missing from the date.** The date function was copied into two files and one was out of date. I moved it to a single shared file.

### Style

- **CSS leaking into entries.** My rules for the home page list also applied to every list inside an entry, so bullets disappeared and text was squeezed into a narrow column. I limited those rules to a class used only on the home page.
- **Styles ignored on the home page.** Links were purple, bullets were visible and there was no space after the date. I replaced the whole CSS file and forced the link color for visited links too.

## Time spent

A few hours, spread over two days.

## Result

The logbook is live at https://jlnrcs.github.io and the deployment works. Every push publishes the site in about two minutes.

## What I learned

- Read the error message first. It is often precise about what is wrong.
- Keep a rule as narrow as possible. A general CSS rule hits more than I expect.
- Windows ignores upper and lower case in folder names, but GitHub's build servers run Linux, which does not. A folder called `Blog` instead of `blog` can work on my machine and break the published site.
- Change one thing at a time and check the result before moving on.

## Next steps

The next entry of the logbook is not about the blog: it is my full migration to Linux. For the blog itself, here is what comes after:

1. Move the hosting to AWS (S3 and CloudFront). That means a bucket, a distribution, a certificate, permissions and DNS. It will be its own project entry.
2. Add a custom domain on GitHub Pages first, so the address stays the same after the move.
3. Write entries in Obsidian, with the vault set on the entries folder, and add an RSS feed.
4. Test images and code blocks in an entry.

Smaller items: a 404 page, a page description, a favicon, a short About page. I will track them as GitHub Issues, as I would on a real project.