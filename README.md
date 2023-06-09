# WebR workshop reader

## Getting past "Loading WebR..."
I've had some difficulty getting the reader to work reliably. There are some clues at [the Github repository of the webR quarto extension](https://github.com/coatless/quarto-webr). In particular, the javascript workers `webr-worker.js` and `webr-serviceworker.js` must be at the base level of the reader (or else pointed to by the `service-worker-url` yaml option, but I haven't tested that.) Even when that is the case, I've needed to manually change the workers to point to the "latest" versions, like:

```{js}
#| eval=false
importScripts('https://webr.r-wasm.org/latest/webr-serviceworker.js');
```

Somehow, Quarto overwrites that every time I render the document, so I have to then go into `docs/` and manually adjust the URIs in the `.js` files. Ideally, I'd figure out how to change the behavior of the `webr` Quarto extension, but I havent' figured that out yet.

## installing packages
The documentation lists multiple ways, but all that's worked for me is to run `webr::install()` at the console in the website.

# Quarto Template: Workshop Reader

This repository is a `Quarto` based template for workshop readers for the UC Davis DataLab.

This template uses [**quarto**](https://quarto.org/) to knit the reader. You can also optionally use **renv** to manage packages and Git Large File Storage to manage large files (instructions included).

To get started, create a new repo on GitHub from this template ([instructions](https://docs.github.com/en/github/creating-cloning-and-archiving-repositories/creating-a-repository-from-a-template)), then `git clone` your new repo.

Once you've cloned the repo, here's a checklist of things to do to prepare the repo:

1.  **renv** (optional): To set up **renv**, open R at the top level of the repo and run:

    ``` r
    renv::init()
    ```

    Restart R. Then, install packages required for project or presentation. Finally, run:

    ``` r
    install.packages(YOUR PACKAGES HERE)
    renv::snapshot()
    ```

    You can skip this step if you're not going to use **renv**.

2.  `index.qmd` and `_quarto.yml`: Replace the all-caps text with your workshop details.

    -   Title
    -   Author's name
    -   Repo name (in 4 places, 2 of them in the `output:` HTML block)
    -   Description, learning goals, & prerequisites

3.  `README.md`: Replace the all-caps text with your workshop details.

    -   Title
    -   Quarter & year
    -   Author's name & email
    -   Reader URL
    -   Event URL
    -   Description, learning goals, & prerequisites

4.  GitHub: In the repo's About section, add the reader URL and appropriate tags.

5.  GitHub: In the repo's Settings page, enable GitHub pages. Set the branch to `main` and the directory to `docs/`.

6.  `README.md`: Remove these template instructions, which end at the `#    Workshop:` header below

7.  `git add` all changed files, then `git commit` and `git push`.

# Workshop: YOUR WORKSHOP TITLE

[*UC Davis DataLab*](https://datalab.ucdavis.edu/)\
*QUARTER YEAR*\
*Instructor: YOUR NAME \<[YOUR_EMAIL\@ucdavis.edu](mailto:YOUR_EMAIL@ucdavis.edu){.email}\>*\
*Maintainer: MAINTAINER'S NAME \<[MAINTAINER_EMAIL\@ucdavis.edu](mailto:MAINTAINER_EMAIL@ucdavis.edu){.email}\>*

-   [Reader](https://ucdavisdatalab.github.io/YOUR_REPOSITORY/)
-   [Event Page](https://datalab.ucdavis.edu/eventscalendar/YOUR_EVENT/)

YOUR DESCRIPTION, LEARNING GOALS, PREREQUISITES, ETC

## Contributing

The course reader is a live webpage, hosted through GitHub, where you can enter curriculum content and post it to a public-facing site for learners.

To make alterations to the reader:

1.  Check in with the reader's current maintainer and notify them about your intended changes. Maintainers might ask you to open an issue, use pull requests, tag your commits with versions, etc.

2.  Run `git pull`, or if it's your first time contributing, see [Setup](#setup).

3.  Edit an existing chapter file or create a new one. Chapter files are Quarto Markdown files (`.qmd`) at the top level of the repo. Enter your text, code, and other information directly into the file. Make sure your file:

    -   Follows the naming scheme `##_topic-of-chapter.Rmd` (the only exception is `index.qmd`, which contains the reader's front page).
    -   Begins with a first-level header (like `# This`). This will be the title of your chapter. Subsequent section headers should be second-level headers (like `## This`) or below.
    -   Uses caching for resource-intensive code (see [Caching](#caching)).

    Put any supporting resources in `data/` or `img/`. For large files, see [Large Files](#large-files). You do not need to add resources generated by your R code (such as plots). The knit step saves these in `docs/` automatically.

4.  Run `quarto preview` to preview the HTML files in the `docs/`. You can do this in the shell or Terminal tab.

5.  Run `quarto render` to build and generate the site completely.

6.  Run `renv::snapshot()` in an R session at the top level of the repo to automatically add any packages your code uses to the project package library.

7.  When you're finished, `git add`:

    -   Any files you added or edited directly, including in `data/` and `img/`
    -   `docs/` (all of it)
    -   `renv.lock` (contains the **renv** package list)

<!-- `.gitattributes` (contains the Git LFS file list)-->

 8. Then `git commit` and `git push`. The live web page will update automatically after 1-10 minutes.

### Caching {#caching}

If one of your code chunks takes a lot of time or memory to run, consider caching the result, so the chunk won't run every time someone knits the reader. To cache a code chunk, add `cache=TRUE` in the chunk header. It's best practice to label cached chunks, like so:

````         
```{r YOUR_CHUNK_NAME, cache=TRUE}
# Your code...
```
````

If you ever want to clear the cache, you can delete the directory (or its subdirectories). The cache will be rebuilt the next time you knit the reader.

Beware that caching doesn't work with some packages, especially packages that use external libraries. Because of this, it's best to leave caching off for code chunks that are not resource-intensive.

````{=html}
<!--
### Large Files

If you want to include a large file (say over 1 MB), you should use git LFS.
You can register a large file with git LFS with the shell command:

```sh
git lfs track YOUR_FILE
```

This command updates the `.gitattributes` file at the top level of the repo. To
make sure the change is saved, you also need to run:

```sh
git add .gitattributes
```

Now that your large is registered with git LFS, you can add, commit, and push
the file with git the same way you would any other file, and git LFS will
automatically intercede as needed.

GitHub provides 1 GB of storage and 1 GB of monthly bandwidth free per repo for
large files. If your large file is more than 50 MB, check with the other
contributors before adding it.
-->
````

## Setup {#setup}

````{=html}
<!--
### Git LFS

This repo uses [Git Large File Storage][git-lfs] (git LFS) for large files. If
you don't have git LFS installed, [download it][git-lfs] and run the installer.
Then in the shell (in any directory), run:

```sh
git lfs install
```

Then your one-time setup of git LFS is done. Next, clone this repo with `git
clone`. The large files will be downloaded automatically with the rest of the
repo.

[git-lfs]: https://git-lfs.github.com/
-->
````

### R Packages

This repo uses [**renv**](https://rstudio.github.io/renv/) for package management. Install **renv** according to the installation instructions on their website.

Then open an R session at the top level of the repo and run:

``` r
renv::restore()
```

This will download and install the correct versions of all the required packages to **renv**'s package library. This is separate from your global R package library and will not interfere with other versions of packages you have installed.

[Back to Top](#top)
