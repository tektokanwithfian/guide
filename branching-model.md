## [Trunk Based Development](https://dora.dev/capabilities/trunk-based-development/)

Main characteristics of Trunk Based Development:

- There's only one main branch, called the `main` (trunk) branch.
- Working branch should be created from the `main` branch, and called `release` branch, `release/[feature-name]` or `release/[major.minor.fix]` branch. 
- When releasing, always `tag` the merged commit on the main branch.

### Initially
* create a new repository
* clone the newly created project local development machine
  `git clone git@[host_url.tld_alias]:[path/to-repo].git`


### Daily

#### Start your day with a main branch checkout
```
/> cd ~/path/to/project
~/path/to/project> git checkout main
```

#### Fetch and pull the latest changes from the remote repository

```
~/path/to/project> git fetch && git pull
```

#### Create a new branch from the main branch

```
~/path/to/project> git checkout -b release/login
```

#### If this is a work in progress, then `checkout` to the wip branch

```
~/path/to/project> git checkout release/login
// if there's changes, in the main branch
~/path/to/project> git merge main
// if there's conflict, resolve the conflicts
~/path/to/project> git add .
~/path/to/project> git commit
```

#### Start working 

* Work on a small problem one at a time, and immediately commit as soon as you finish the work (commit often). 
* Commit messages should be [meaningful and descriptive](https://cbea.ms/git-commit/).
  * start your commit message with a one word imperative verb (mood); `[ADD] | [FIX] | [MODIFY] | [REMOVE] | [REFACTOR]`
  * Followed by a space, and then a short description of the commit.
* Push your finished work to the remote repository.
```
~/path/to/project> git push origin release/login
```
* Create a pull request, from the `release/login` branch as the source to the `main` branch as the destination.


## TODO

* gitflow
* more details on git
  * rebase