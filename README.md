# guide 

A central reference to streamline and homoginize a team's workflow. 

- [Useguide](#useguide)
	- [Avoid straying from conventions](#avoid-straying-from-conventions)
	- [Use 'git pull --rebase' when getting up to date](#use-git-pull---rebase-when-getting-up-to-date)
	- [Understand git branching models](#understand-git-branching-models)
		- [Github-flow](#github-flow)
		- [Gitflow](#gitflow)
		- [Trunk](#trunk)
	- [Keep branches up to date](#keep-branches-up-to-date)
	- [Delete a branch](#delete-a-branch)
	- [Visualize branch flow](#visualize-branch-flow)
	- [Rebase commits for readability](#rebase-commits-for-readability)
	- [Properly setup](#properly-setup)
	- [Write good commit messages](#write-good-commit-messages)
		- [Template of a good commit message](#template-of-a-good-commit-message)
		- [Fixing bugs and closing issues](#fixing-bugs-and-closing-issues)
		- [Keep the subject line under 50 chars, and the body under 80](#keep-the-subject-line-under-50-chars-and-the-body-under-80)
		- [Line-break the commit body](#line-break-the-commit-body)
		- [Use a present tense imperative in the subject line](#use-a-present-tense-imperative-in-the-subject-line)
		- [Don't blend syntax with English](#dont-blend-syntax-with-english)
- [Styleguide](#styleguide)

## Useguide

Use these conventions to improve your workflow and dev experience.
### Avoid straying from conventions

Always consider how other developers and programmers will view their code and workflow – In one question, **what will others expect out of your source code and workflow?** If a developer uses strange filetrees, branching models, capitalization, or indentation, other developers will be forced to think harder about their code, leading to more fatigue and poor performance.

Synchronizing and conforming to conventions is self-fulfilling – as more programmers use conventions workflows become more streamlined and collaboration becomes easier.

With that said, conforming to conventions doesn't change what is most *effective* for programming. That is a different beast entirely, and is the mechanism the drives shifts in conventions. 

### Use 'git pull --rebase' when getting up to date

```git
$ git pull --rebase
```

When working in a group, local repos become outdated often, and have to pull down changes made by other devs. `git pull` is made just for this, but by default **causes an extra merge commit just to update the local repo.** This isn't pretty, and gets even nastier with dozens of devs. Use `git pull --rebase` to cleanly update the repo with what's going on.

### Understand git branching models

Using a branching model will amplify the usefullness of branching, allowing each dev to be more creative with creating code and designing changes without stress.

However, teams can use a variety of models, and it's important to choose the right one for the group. **Be aware of the three main models – gitflow, github-flow, and trunk – and choose which is most effective for each project.**

#### Gitflow

While not having an official name, 'gitflow' is the most advocated branching model amongst Github developers, while also being the most complicated.

```
Hotfix        -----
             /     \
Master  ------------------------------
         \                          /
          \                        /
Release    \                  -----
            \                /     \
Develop      -------------------------
                 \     /
Feature           -----
```

- `Master` hosts the ``production`` – the code currently being used by the end-user.
- `Develop` branches off `master`, and is where developers work to make changes and improvements to code. QA would most often be done here and in `release`.
- `Release` branches off `develop`, which is given final QA, no new features, and is tagged with versioning.
- `Feature` branches off `develop`. There are many features operating simultaneously – some by solo developers, and some by teams. They are merged into `develop` once finished.
- `Hotfix` branches off `master`, and can be merged into `master` and `develop`.

In comparison to [Github-flow](#github-flow),  gitflow allows for stricter versioning and segregation of code.

#### Github-flow

>If your code has only one version in production at all times (e.g., web sites, web services, etc) you may use github-flow. Main reason is that you don't need to complex things for the developer. When a developer finishes a feature or bugfix it is immediately promoted to production.
>– Gayan Pathirage via Stack Overflow

```
                 hotfix-sqli
                    -----
                   /     \
 Master  ----------------------------
             \         /             \
              ---------               --- release-2.0
             feat-mobile
```

- `Master` hosts production.
- Any `features`, `hotfixes`, and `releases` branch off `master`.

The appeal of Github-flow is simple:

- Provide an easy to use branch model for **continuous integration (CI)**.
- Have each dev work with the same parent branch of code
- Much, much less likelihood that any codebase will diverge and cause conflicts.
- Simplify the codebase behind pull requests

#### Trunk

Trunk-based development is another model focused on **continuous integration (CI)** that has been around for many years. Facebook uses this model, pushing new, production-ready code every day.

```
              2.x   2.1.x            3.x
Release       --------x-----         -------
             /                      /
Master  ----------------------------------------
```

Trunk is very unique in its aspects –

- `Master` is **not** production, and instead is the universal development environment.
- `Master` is colloquially known as the 'trunk.'
- Developers exclusively commit to `master`.
- Each commit is production-ready. Ergo, no commit ever breaks the build.
- The model employs **release engineers**, who have specific rights –
    - Branch off `release` branches, which are then sent to production
    - Cherry pick commits from `master`. Useful for bugs.
- **Multiple productions** can be simultaneously hosted and debugged.

Knowing these three models in mind, you can effectively use what works best for your project and team.

### Keep branches up to date

Realistically, there will be ongoing features and a release at work for any repository.

```
Develop  ------------------x---x
             \     /    \
Feature       -----      ---- feature-mobile
```

To update `feature-mobile`,

```
$ git checkout feature-mobile
$ git rebase develop
```

`Feature-mobile` is now at the tip of `develop`.

```
Develop  ------------------x---x
             \     /            \
Feature       -----              ---- feature-mobile
```

### Delete a branch
 
Delete unused or recently implemented branches to keep the repository clean. This must be done locally and remotely.

```
$ git checkout develop
$ git branch -D feature-mobile
$ git push origin --delete feature-mobile
$ git push
```

### Visualize branch flow

Many teams don't unanimously use a git GUI (although I heard they're pretty cool), so each dev should have a visual of the branch flow. `lol` and `lola` are popular ways to visualize the current branch tree and 'all' branches.

```
$ git config --global alias.lol log --graph --decorate --oneline -20
$ git config --global alias.lola log --graph --decorate --oneline --all -20
```

To modify local aliases, head over to `~/.gitconfig` 

### Rebase commits for readability

Rebasing is a fantastic tool enabling devs to make repo histories more readable and streamlined. If you haven't already, set up git to point to your editor.

```
$ git rebase -i HEAD~2
``` 

Opens an editor to *interactively* work with the `HEAD` and its parent.

```
pick f7f3f6d changed my name a bit
r 310154e updated README formatting and added blame
```

*Reword* the commit message. Useful when accidentally commiting with typos or lackluster language.

```
pick 4e64052 Revise animations to use CSS in homepage
f 4bdc7dc Fix IE compatibility with homepage animations
```

*Fixup* the commit, as if fixing a bug in the previous commit. This discards the commit message.

```
pick 6h49f6s Impl. French language support
s 9n28e0a Impl. Spanish lanuage support
```

*Squash* the commit into its parent, improving the readability of the commit history. Subsequently opens an editor to modify their commit messages, which have also been combined.

*Be diligent when rebasing content that has already been pushed, as it creates a new commit and hash, obscuring any issue or comment that referenced the old hash.*

### Properly setup a computer
	
- Keep git profiles synchronized across your dists

    ```
    $ git config --global user.name "First Last"
    $ git config --global user.name "public-email@gmail.com"
    ```
    
    When done correctly, `blame` and `shortlog` will be attributed to everyone correctly.
    
- Turn on 2-Factor authentication for your emails, including your GitHub account

	GitHub's hosted information is very accessible. In the past, many accounts have been compromised through [brute force](https://github.com/blog/1698-weak-passwords-brute-forced) and even [reused passwords](https://github.com/blog/2190-github-security-update-reused-password-attack) that were stolen from other websites *last month*. 2-Factor is an easy way to sleep soundly at night.

- Never, ever move around your authentication keys
	
	They are *disposable*. Leaving them up to human error can open an opportunity for an attacker to do monumental damage. Keep them close. Recreate them if you have any doubt. 

### Write good commit messages

>As a golden rule, the commit message must contain all the information required to fully understand & review the patch for correctness. Less is not more. More is more.
>– *Git Commit Good Practice*, OpenStack

#### Template of a good commit message

```
[Imperative] [Noun] [Context]

[Expanded context and explanation]
```

This encompasses the best worded commits. They are straight and to the point, and give the reader a good idea of what's going on, and what the commit is doing about it.

#### Fixing bugs and closing issues

```
[Subject line]

Originally, [what was happening]

[Back-end explanation of bug]

[Solution]

Closes #XX
```

Here's a great example:

```
Fix infinite recursion bug in nested @Configuration

Prior to this commit, an infinite recursion would occur if a
@Configuration class were nested within its superclass, e.g.

  abstract class Parent {
      @Configuration
      static class Child extends Parent { ... }
  }

This is because the processing of the nested class automatically
checks the superclass hierarchy for certain reasons, and each
superclass is in turn checked for nested @Configuration classes.

The ConfigurationClassParser implementation now prevents this by
keeping track of known superclasses, i.e. once a superclass has been
processed, it is never again checked for nested classes, etc.

Issue: SPR-8955
```
Issues can be closed directly using a [keyword](https://help.github.com/articles/closing-issues-via-commit-messages/).

#### Keep the subject line under 50 chars, and the body under 80
This keeps any logs (shortlog, --oneline) within one line. For reference, 66 columns is considered the most legible for reading. 72 and 80 are popular column widths for programming.

For vimmers, you should know that line numbers and status symbols eat up column space, so it's a good idea to use `set cc=80` to colorize the 80th column in the document, and `set columns` a bit wider. I use 86. To change `cc` color, use `highlight ColorColumn guibg=color`. I use grey19.

#### Line-break the commit body

Add a line break at your column limit. This improves legibility of the commit message, as it standardizes the rendering of the commit throughout GitHub and the command line. Just like spaces over tabs. \**darts out door*\*

#### Use a present tense imperative in the subject line

Commits are commands to change and revise code; they are not historical artifacts. While you *did* do something, the commit *does* something.

Good:

- `Implement homepage link in top left corner of all sites`

Bad:

- `Implemented homepage link in corner`
- `Websites now link to homepage in top left corners`
- `Improve UX`

#### Don't blend syntax with English

While useful at conveying information in a shorter amount of characters, this causes the reader to switch gears too often, thinking *'Was that regex? Or was that python regex...?'*

Good: `Propagate cc to vim configuration files`

Bad: `Propagate cc to [_.]vimrc`

## Styleguide

As a rule of thumb, prioritize the readability of using code conventions. It is ironic attempting to condense code that will be read by programmers.
