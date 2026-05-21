# Great Scott! A Chronicle of the Dangers of Time Traveling in R

Presented at the SoCal R User's Group May 21, 2026

## Abstract

R has a distinct quality: someone who sits down to their computer, downloads the latest version of R, and installs some packages from CRAN will largely have no issues. This is one of R’s greatest strengths, particularly for newcomers: you can get right to work. This is by design: CRAN provides packages in such a way that we receive certain guarantees that everything is going to (mostly) work together on a given day. Revisiting a project in R later, however, is often painful: all those things that worked beautifully together suddenly fail. Either you have to update all your code to match the changes that have been made in the packages you use, or you have to find a way to restore your R session to the state that it was in when you were working last.

R has a long history of community tools for managing package environments, such as packrat and renv. Yet, it’s still rarely an easy process. Exposure to the uv framework for managing Python environments—itself a harrowing history—showed me that there could be a better way. I set out to revisit R package environments and came out the other side with a new point of view.

In this talk, I’ll discuss why we face issues restoring R package environments, improving renv workflows, thinking through system dependencies, and exploring other tools like Posit Package manager, Docker, and rv.

Below, I include summarized advice from the talk.

## Do not split yourself across time

R work's best *today*. That means the current, released version of R together with packages on CRAN today. R Core and CRAN give us certain guarantees and benefits that make it easy to work with R.

Setting up a project for successful future restoration means working like this. It is dangerous for a time traveler to split themselves across time. Instead, work at a particular moment in time:

1. Pick a day.
2. Identify a source of binaries and the R version for that day. Posit Package Manager is currently a good source, which takes daily-ish snapshots of CRAN and supports binaries for many operating systems.
3. Sync your packages and R version to that day. You can use the rversions package to identify the date of release of an R version.
4. Move forward in time all at once: Update to the newest version of R and use a snapshot of CRAN from a recent day.

## Recipes

### renv

**New project**
1. Create a project
2. `renv::init()`
3. Write code, install packages
4. `renv::snapshot()`
5. Iterate

**Restoring a project**
1. Clone and open the project
2. `renv::restore()`

**Time-traveling**
- For new projects, set `options(repos = "...")` (e.g., a dated Posit Package Manager snapshot). Check the lock file to confirm renv is playing along.
- For existing projects, `renv::checkout(date = "...")`. You may need to manually change repos in the lock file.

### rv

rv is a new, fast, and impressive R package manager that takes inspiration from the uv Python package manager. It's very promising!

**New project**
1. Create a project
2. `rv init`
3. Write code, `rv add <R package>`
4. `rv run script.R` or work directly in R
5. Iterate

**Restoring a project**
1. Clone and open the project
2. `rv sync`

**Time-traveling**
- For new projects: `rv configure repository`
- For existing projects: `rv migrate renv`

### rig (R version)

- Going back in time: look up the release date with `rversions::r_versions()`, then `rig add <version>` and `rig default <version_name>`.
- Going forward in time: `rig add release` and `rig default release` to update to today's release.

### Containers

Containers help solve 

- Pairing a specific day with a binary source makes container restores substantially easier.
- Containers extend the reproducibility half-life of a project past the point where package managers alone can carry it.

## Links

### Resources
- [Shannon Pileggi's talk](https://www.youtube.com/watch?v=l01u7Ue9pIQ)
- [Posit Package Manager](https://packagemanager.posit.co/client/#/)
- [rig](https://github.com/r-lib/rig)
- [rv](https://a2-ai.github.io/rv-docs/)
- [lemon-lucifer](https://github.com/andrewheiss/lemon-lucifer)
- [uv](https://github.com/StanfordHPDS/uv)

### Image sources
- [Unsplash](https://unsplash.com/photos/a-house-with-a-grass-roof-and-a-tree-G2GBz0ysToc)
- [xkcd](https://xkcd.com/1987/)
- [NASA](https://science.nasa.gov/mission/cassini/grand-finale/overview/)
