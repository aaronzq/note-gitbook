# Gitbook Notes

Previsouly I have been using gitbook.com to take down notes. It's pretty convenient to have a notebook that renders into webpage so that I could take a look using browser on whatever device and wherever I work. But I cannot find a way to export the markdown files to my local machine. This could be annoying since one advantage of markdown-based notebook is that you can easily publish your written note on other platforms as long as they support markdown. So I tried to pick up the old version of gitbook-cli (they don't maintain it since 2018). The general pipeline of a online notebook will be: 1. Edit markdown files locally (e.g. VSCode + [Markdown Preview Enhanced plugin](https://shd101wyy.github.io/markdown-preview-enhanced/#/zh-cn/image-helper)) 2. Build the book using gitbook-cli 3. Commit the files to Github.com 4. Synchronize the gitbook page with the github repository 5. (optinal) Edit on gitbook.com and sync the github repository with it

## Install gitbook locally

Gitbook is based on Note.js, so we first install [Node.js v12.18.1](https://nodejs.org/download/release/v12.18.1/). And then install gitbook-cli by running in the cmd terminal:

```
npm install -g gitbook-cli@2.1.2 --global
```

Note `npm` could be installed duing Node.js installation (check that option). **The latest version of Node.js and gitbook-cli might not work** and errors would raise when running `gitbook`. I got the following [error on "data" argument](https://github.com/GitbookIO/gitbook-cli/issues/113), [error on pipesCount](https://stackoverflow.com/questions/61538769/gitbook-init-error-typeerror-err-invalid-arg-type-the-data-argument-must-b) and [error on graceful-fs](https://stackoverflow.com/questions/64211386/gitbook-cli-install-error-typeerror-cb-apply-is-not-a-function-inside-graceful). After adjusting package versions following these references, I managed to get it to work.

## Initize the (note)book

Create the notebook folder. Navigate the cmd terminal to this folder and run

```
gitbook init
```

You will get two markdown templates SUMMARY.md and README.md. The SUMMARY.md is the table of content of the notebook. Open it and put something like:

```
# Table of contents

* [Introduction](README.md)
* [Gitbook Notes](gitbook.md)
* [Git Notes](git.md)
* [C++ Notes](cpp/README.md)
  * [Standard Library](cpp/std.md)
  * [Algorithms](cpp/alg.md)
```

There are some files and folders here we have not create yet. But once you run

```
gitbook build
```

gitbook will automatically create them for you. If you use `gitbook serve`, you can also render them into html on your localhost so you can check your book. Pretty easy!

## Commit to github.com and Sync the gitbook.com

Create the repository on your **github** account and push the local notebook files to it. And go to your **gitbook** account. On your **gitbook** you can open a **space** (I think they call it **space** instead of **book** now). In the **Integrations** tab we can choose to _integrate with GitHub_. Follow the instruction and choose the correct repository. Done.

## Update the notebook

We can always edit/create files locally and upload to GitHub. Gitbook will automatically synchronize with it. But it's also possible that we make changes on Gitbook using its online editor. Gitbook will also make commits to the Github repository. Remember to pull the repo locally before editting in such case. Lastly, we can put the notebook files any server we want, including Github pages. And the notebook is neat!
