---
layout: page
type: text
title: Fun with Git Filter-Branch
categories: 
- program
---
I put it off for so long. It seemed like a good idea at the time to commit my raw data input files (CSV) that I was using for my R analysis, because it was time consuming to generate them and so this seemed like a good way to back them up. But then as the analysis evolved some of the files became redundant and so I wanted to purge them from the repository (yes, with hindsight it is obvious they should have never gone in there), but I  knew that required use of `git filter-branch` and that looked kind of scary.

Turns out it isn't actually too tricky and you can do some nifty things with it - plus you can always test on a dummy branch or even a fresh clone of a repository. 

	git filter-branch --prune-empty --tree-filter 'find -type d -name "read" -print0 | xargs -0 rm -rf' HEAD

You can do lots of useful things with `find`. Fortunately I had at least structured by repository sensible and all input files were in sub-directories called "read". So this `find` command is looking for all directories (`-type d`) called "read". `print0` is used to get the output into a format that can be piped into `xargs`; deals with spaces in names, etc. So what this does is iterate through each commit, running the `find` command each time, which in turn looks for any directories called "read" and on finding them deletes them and the contents. Lastly, the `--prune-empty` option is used to remove any empty commits (i.e. those commits where all I had done was to add the input files). All that was left to do was force push this to the remote repository.

Now I was a bit more comfortable with `git filter-branch` I used it to split some files out of an existing repository. Since I'm a bit of cheapskate I only have the Github Micro plan which means I have this "catch-all" repository for various bits of code and scripts. Unfortunately, this can result in the scenario where I start something off as a single file little script, which then grows into a multi-file programme, which then makes more sense in it's own repository, which means extracting it out of the "catch-all"  repository. Also unfortunately, in this case it wasn't as simple as [Splitting out a sub-directory into a new repo](https://help.github.com/articles/splitting-a-subpath-out-into-a-new-repo) as I had multiple files in multiple sub-directories (my "catch-all" repo is organised by language, rather than project - since it isn't meant to be for projects). What I did have, however, was consistent naming. All the files I required had the text "ihateponies" in their filename (not really, but ...) and all of the files I didn't require, didn't. Therefore I could use a `find` command that would filter out anything not required.

First I created a new clone of the repository. Then I ran this:

	git filter-branch --prune-empty --tree-filter 'find !  -path "*ihateponies*" ! -path "*git*" -type f -delete' HEAD

Then I had my new repository!

This `find` command looks for any path names (I'm sure when I tried I [couldn't get the negation to work](http://stackoverflow.com/questions/9816739/find-files-not-equal-to-pattern/9825586#9825586) with `-name`, but I can't seem to duplicate that problem now) that don't contain "ihateponies", where the type is a file (`-type f`) and deletes them. To be on the safe side I also excluded pathnames that contained "git" since I didn't want the find script deleting the `.git` directory. And again I used `--prune-empty` to clean up the commit history.

For the original "catch-all" repository I just did a `git rm` on the relevant files because I was fine with having the history of those files in there as well.
