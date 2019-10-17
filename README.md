# Dealing with different versions of Jupyter notebooks: a brief tutorial

[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/brian-rose/notebook_diff_tutorial/master)

## The problem

As well all know by now, Jupyter notebook `*.ipynb` files are awesome because they combine formatted text, source code and rich graphical output. However they don't play well with traditional text-based version control because a `diff` of two notebook versions is kind of a mess.

## The goal

We would like to do the following:
- save a `*ipynb` in version control (git)
- make some changes to it
- See a rich visual comparison of the two versions, highlighting changes to all three elements of the notebook (text, code, graphics)

Furthermore, we would like to be able to do this comparison at *two different points* in our collaborative workflow:
1. within our actual work environment (e.g. jupyterlab running on our laptop, or on a remote server like https://ocean.pangeo.io) -- so we can view and compare changes that we've made before we push them to GitHub
2. On GitHub, so that we can collaborate with others on revisions to a notebook and understand what's actually different in each pull request.

## A solution to problem 1: `nbdime`

[nbdime](https://nbdime.readthedocs.io/en/latest/) ("notebook diff and merge") is a nice open-source tool that solve problem 1. Once installed in your Jupyter environment, it provides a push-button solution for rich visual side-by-side comparison of notebook versions.

### Basic usage:

- Commit your notebook using git, or clone a repo that already contains a notebook.
- Make changes to the notebook. *Make sure to **save** *your changes or else the next step won't work.*
- Click the button on the Jupyter toolbar called `git nbdiff`
- You should now see a split-screen view comparing your new version to the committed version.
- Enjoy this rich visual comparison, decide if you're happy with the changes.
- Commit your changes.

Check this out yourself in [binder](https://mybinder.org/v2/gh/brian-rose/notebook_diff_tutorial/master)

### Pros and cons

- `nbdime` is open source and seems well supported by the Jupyter ecosystem.
- Plays well with git locally, i.e. command-line `git diff` will give useful output.
- Is easy and intuitive to use once installed.
- Basically a great solution to problem 1 but doesn't help directly with problem 2.

## A solution to problem 2: `ReviewNB`

[ReviewNB](https://www.reviewnb.com) is a plug-in app for GitHub. It provides similar rich visual diffs between notebook versions with one click from GitHub. It also provides code review tools for notebooks, so we can do things like comment on specific cells in the notebook during a pull request.

### Basic usage:

- Start with a GitHub repo containing some notebooks. E.g. try forking [this repository](https://github.com/brian-rose/notebook_diff_tutorial)
- Go to https://www.reviewnb.com and click the "Install GitHub App" button.
- Start by enabling this only for your tutorial repository
- Make a new branch, make some changes to `example1.ipynb`, and try a pull request to your own master branch.
- You should now see a button saying "Check out this pull request on ReviewNB"
- Click on this and enjoy a rich visual difference.
- Try making comments on individual cells in the notebook.

### Pros and cons

- Point and click convenience for collaborating on notebooks in GitHub.
- Solves the common irritation of not being able to view notebooks properly within GitHub.
- A great tool for collaboration.
- ReviewNB is a third-party commercial product. While it is free for unlimited use with open-source repos, its own source code is closed. My *guess* is that it actually uses nbdime under the hood, but I can't verify this.

## An alternative approach: `Jupytext`

[Jupytext](https://jupytext.readthedocs.io/en/latest/?badge=latest)

This is a different kind of solution. It’s a tool for saving notebooks in plain text formats that are more amenable to version control and standard diff tools.

Basically when we save a `*.ipynb` file we also save companion files in text formats like `*.md` and `*.py`. The strategy then is for collaborators to only put the text files under version control and not actually share the `*.ipynb` files at all. Each user regenerates the notebook from source.

This is somewhat like stripping the notebook of all output before putting in version control, but easier. This doesn't give the rich visual difference in the *output* that we get with `nbdime`, but may be a good solution if you don't think the output (i.e. the graphics) should be in version control in the first place.
