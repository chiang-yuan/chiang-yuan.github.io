---
layout: post
title: How to clone and manage a jekyll website for personal portfolio and blog
date: 2023-05-06 18:00:00-0800
description: 
tags: web-development markdown jekyll frontend
categories: web-development hack
giscus_comments: true
---

- [Have your first site online in 10 mins](#have-your-first-site-online-in-10-mins)
- [Create a local branch before pulling from upstream](#create-a-local-branch-before-pulling-from-upstream)
- [Enable autoregeneration on WSL](#enable-autoregeneration-on-wsl)
- [Preview drafts during development](#preview-drafts-during-development)
- [Too many commits? Try `git squash`](#too-many-commits-try-git-squash)
- [TODO](#todo)

---

Nowadays the digital presence becomes increasingly important for professionals in all spheres. It's undeniable that the ones who can reach higher and wider visibility online control more power and opportunities. Fortunately, there are many available and convenient options out there to fast build the website easily from scratch, such as [Wix](https://www.wix.com/), [WordPress](https://wordpress.com/), [Weebly](https://www.weebly.com/), *etc*. 

However, as a control freak and a nerd who loves elegant and neat solutions to every problem and likes to present (~~show off~~) their coding skills, the GUI options are not an option, and having a static and minimalist [jekyll](https://jekyllrb.com/) website as a platform to showcase the work and idea is almost **the most popular and professional choice**. (You can simply google how other programmers, engineers, and scientists build their sites.)

Of course, there are new and fancy alternatives, like [Gatsby](https://www.gatsbyjs.com/) and [Next.js](https://nextjs.org/), that are empowered by JavaScript to have dazzling animations and interactive functions. They are definitely suitable for organizations and companies that need powerful applications and integration with backend and database. I once used Gatsby to build my personal website and thought this new technology can bring me a lot of convenience. It turned out that it never feels flexible enough for me to adjust style and to create technical content that looks close to the community I identify myself with. *It just doesn't feel right.* 

Since I have been using [al-folio](https://github.com/alshedivat/al-folio) theme for a while and have modified a large portion of the code to suit my purpose and taste, it might be a good idea to keep track of the developing process along the way and meanwhile to keep my website up-to-date with the upstream to incorporate new functionality, _e.g._ copy button for markdown post ([PR #1267](https://github.com/alshedivat/al-folio/pull/1267))! This post serves both as my note and technical writing practice. Hope this can also help other young professionals build their own digital garden, knowledge vault, or project archives 😃!

## Have your first site online in 10 mins

Having your first site online is very simple. To begin with, just choose one template from github repositories ([official docs](https://jekyllrb.com/docs/themes/), [github topics](https://github.com/topics/jekyll-theme), [jekyll-themes.com](https://jekyll-themes.com/)) and follow the instruction to fork, clone and deploy through GitHub Pages. Each template may have different instructions but essentially only three to four steps:

1. Fork and rename the repository
2. Clone make change locally
3. Commit and push the change to remote repository
4. [Modify repository setting](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site) to ensure correct workflow for automated deploy

If successful, the site will appear correctly at `<username>.github.io` or `<username>.github.io/<reponame>` or custom domain if you have configured. 

My friend [Enze Chen](https://enze-chen.github.io/) also shares his [10-step tutorial](https://enze-chen.github.io/website) to build a simple jekyll site from zero to hero. Check out his post 🙌!

## Create a local branch before pulling from upstream

Once I had nightmare when I tried to upgrade the my repository version by directly pulling from the upstream to my master branch. I followed the instruction to rebase the repository to the newest version. However, I already had a too many commits that cause compatibility issue with the newest version, and it turned out I couldn't just cancel the rebase in a clean way halfway through the process. In the end I just killed my local repo and clone it back from my github 💀.

<iframe src="https://giphy.com/embed/wloGlwOXKijy8" width="480" height="271" frameBorder="0" class="giphy-embed" allowFullScreen></iframe>

In the hindsight, I should first clone a local branch (and of course it is a good habit) that is not tied to my remote branch and thus would not affect my website even if I couldn't rebase and resolve the conflicts. By doing so, I can safely pull new functionality from the upstream without hurting my own repo. Here are the step-by-step procedures:

1. Create a new branch named `update` based on the current HEAD (in my case just `master` branch) 
    
    ```shell
    git branch update
    ```

2. Since we may want to make quiet, small update on `master` for convenience and only leave `update` branch for upgrading, the `master` might be several commits ahead of the `update` so need to merge change from `master` to `update` before rebasing. 

    ```bash
    git checkout update
    git merge master
    ```

3. Rebase `update` branch on `upstream`

    ```bash
    git remote add upstream https://github.com/alshedivat/al-folio.git
    git fetch upstream
    git rebase <version>
    ```

4. After resolving all the conflicts, we can safely merge `update` back to `master`

    ```bash
    git checkout master
    git fetch 
    git pull
    git merge update
    ```

5. Push the updated local `master` to remote repository

    ```bash
    git commit -am 'whatever message'
    git push
    ```

## Enable autoregeneration on WSL 

After knowing how to build and update the website, it is time to actively develop the site content. Usually we want to visualize our site before pushing to the repository. To preview the change, simply run the following command at the site home directory: 

```bash
bundle exec jekyll serve
```

[Ruby](https://www.ruby-lang.org/en/) will generate a local url (*e.g.* [http://127.0.0.1:4000/](http://127.0.0.1:4000/)) that can be opened by the browser. 

However, when I use windows PC, it is tricky to enable site autoregeneration on WSL. (I am switching to macbook soon anyway 😇) If you execute `bundle exec jekyll serve` on WSL, you will see the warning: 

```bash
Auto-regeneration may not work on some Windows versions. Please see: https://github.com/Microsoft/BashOnWindows/issues/216
If it does not work, please upgrade Bash on Windows or run Jekyll with --no-watch.
```

and jekyll cannot regenerate the site for you automatically after you modify your site content. 

![](../../../assets/post/2023-05-06-18-37-32.png)

![](../../../assets/post/2023-05-06-18-55-14.png)

To resolve this, we need to add flag behind the original command as [@gamepad-coder](https://github.com/microsoft/WSL/issues/216#issuecomment-756424551) shared: 

```bash
bundle exec jekyll serve --force_polling --livereload
```

## Preview drafts during development

Jekyll offers a [functionality](https://jekyllrb.com/docs/posts/#drafts) to preview drafts locally when building the site. To enable this functionality, we simply add `--drafts` after `bundle exec jekyll serve`: 

```bash
bundle exec jekyll serve --force_polling --livereload --drafts
```

## Too many commits? Try `git squash`

```bash
git reset --hard HEAD~n # goes back to last n commit
git reset --hard <sha1-commit-id>
```

```bash
git merge --squash HEAD@{1}
git commit
git push
```

## TODO

- [ ] [Add heading anchor/link button for subtitles](https://blog.briandrupieski.com/generate-anchors-in-jekyll-blog-post)
- [x] [Code block copy button]()


