# sensible 

Learning git isn't intuitive; neither is using it properly. Git-sensible provides a more streamlined git workflow, as well as important pointers on how to use it properly. In a team setting, this useguide can be a great central reference to streamline and homoginize a team's workflow. 

- [Useguide](#useguide)
	- [Properly setup](#properly-setup)
	- [Avoid straying from conventions](#avoid-straying-from-conventions)
	- [Understand git branching models](#understand-git-branching-models)
		- [Gitflow](#gitflow)
		- [Github-flow](#github-flow)
		- [Trunk](#trunk)
	- [Delete a branch](#delete-a-branch)
	- [Rebase commits for readability](#rebase-commits-for-readability)
	- [Write good commit messages](#write-good-commit-messages)
		- [Keep the subject line under 50 chars, and the body under 80](#keep-the-subject-line-under-50-chars-and-the-body-under-80)
		- [Line-break the commit body](#line-break-the-commit-body)
		- [Use a present tense imperative in the subject line](#use-a-present-tense-imperative-in-the-subject-line)
		- [Don't blend syntax with English](#dont-blend-syntax-with-english)
		- [Template of a good commit message](#template-of-a-good-commit-message)
		- [Fixing bugs and closing issues](#fixing-bugs-and-closing-issues)

## Sensible gitconfig

Much like vim-sensible's ~/.vimrc, paste this in your ~/.gitconfig to improve git's functionality.

```
[user]
    name = First Last
    email = public-email@gmail.com
[core]
    autocrlf = input
[alias]
	lol = log --graph --decorate --pretty=oneline --abbrev-commit -20
	lola = log --graph --decorate --pretty=oneline --abbrev-commit --all -20
[push]
	default = simple
[branch]
    autosetuprebase = always
```

`[user]`: Set and forget. `git blame` and `git shortlog` will be attributed correctly.
`[core]`: Linux and Windows systems handle line breaks (enter key) differently. Set to `input` on Mac, and `true` on Windows machines.
`[alias]`: Use `git lol` to quickly visuable the branch flow.
`[push]`: Simplify git pushing.
`[branch]`: If you have work in the index, automatically rebase over it when pulling.

## Useguide

Use these conventions to improve your workflow and dev experience.

### Properly set up
	
#### *Should I use my real name?*

The answer will almost always be yes.

If you're concerned about security and privacy, it is much more effective to keep track of all the *other* information about you online – hotels, webforums, and financial sites are most often hacked and contain dubious amounts of personal information. Close old accounts, mitigating the chance that your passwords and payment information may be leaked by a [breach](https://en.wikipedia.org/wiki/List_of_data_breaches). Vigilante.pw estimates 1.8 billion records have been [stolen](https://vigilante.pw/) from data breaches.
    
#### Turn on 2-Factor authentication for your emails, including your GitHub account

GitHub's hosted information is very accessible. In the past, many accounts have been compromised through [brute force](https://github.com/blog/1698-weak-passwords-brute-forced) and even [reused passwords](https://github.com/blog/2190-github-security-update-reused-password-attack) that were stolen from other websites *last month*. 2-Factor is an easy way to sleep soundly at night.

#### Never, ever move around private keys
	
They are *disposable*. Leaving them up to human error can open an opportunity for an attacker to do monumental damage. Keep them close. Recreate them if you have any doubt. 

### Avoid straying from conventions

Always consider how other developers and programmers will view your code and workflow – In one question, **what will others expect out of your source code and workflow?** If a developer uses strange filetrees, branching models, capitalization, or indentation, other developers will be forced to think harder about their code, rapidly increasing fatigue.

Conforming to conventions is self-fulfilling – as more programmers use conventions workflows become more streamlined and collaboration becomes easier.

With that said, conforming to conventions doesn't change what is most *effective* for programming. That is a different beast entirely, and is the mechanism the drives shifts in conventions. 

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

- `Master` hosts the **production** – the code currently being used by the end-user.
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

- `Master` hosts production
- Any `features`, `hotfixes`, and `releases` branch off `master`

The appeal of Github-flow is simple:

- Provide an easy to use branch model for **continuous integration (CI)**
- Have each dev work with the same parent branch of code, enabling easier branch collaboration
- Much, much less likelihood that any codebase will diverge and cause conflicts
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
    - Create `release` branches, which are sent to production
    - Cherry pick commits from `master`. Useful for bugs.
- **Multiple productions** can be simultaneously hosted and debugged.

Knowing these three models in mind, you can effectively use what works best for your project and team.

### Delete a branch
 
Delete unused or recently implemented branches to keep the repository clean. This must be done locally and remotely.

```
$ git checkout develop
$ git branch -D feature-mobile
$ git push origin --delete feature-mobile
$ git push
```

### Rebase commits for readability

Rebasing is a fantastic tool enabling devs to make repo histories more readable and streamlined. If you haven't already, set up git to point to your editor.

```
$ git rebase -i HEAD~2
``` 

Opens an editor to *interactively* work with the `HEAD` and its parent. Here are a few examples.

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

*Squash* the commit into its parent. Subsequently opens an editor to modify their commit messages, which have also been combined.

*Be diligent when rebasing content that has already been pushed, as it creates a new commit and hash, obscuring any issue or comment that referenced the old hash.*

### Write good commit messages

>As a golden rule, the commit message must contain all the information required to fully understand & review the patch for correctness. Less is not more. More is more.
>– *Git Commit Good Practice*, OpenStack

Let's cover the semantics first.

#### Keep the subject line under 50 chars, and the body under 80

This keeps any logs (shortlog, --oneline) within one line. For reference, 66 columns is considered the most legible for reading. 72 and 80 are popular column widths for programming.

For vimmers, you should know that line numbers and status symbols eat up column space, so it's a good idea to use `set cc=80` to colorize the 80th column in the document, and `set columns` a bit wider. I use 86. To change `cc` color, add `highlight ColorColumn guibg=blabla` in your vimrc. I use grey19.

#### Line-break the commit body

Add a line break at your column limit. This improves legibility of the commit message, as it standardizes the rendering of the commit throughout GitHub and the command line. Just like spaces over tabs. \**darts out door*\*

#### Use a present tense imperative in the subject line

Commits are commands to change and revise code; they are not historical artifacts. While you *did* something, the commit *does* something.

Good:

- `Implement homepage link in top left corner of all sites`

Bad:

- `Implemented homepage link in corner`
- `Websites now link to homepage in top left corners`
- `Improve UX`
- `haaaands`

#### Don't blend syntax with English

While useful at conveying information in a shorter amount of characters, this causes the reader to switch gears too often, thinking *'Was that regex? Or was that python regex...?'*

Good: `Propagate cc to vim configuration files`

Bad: `Propagate cc to [_.]vimrc`

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