# guide 
A central reference to streamline and homoginize a team's workflow. 

* [Useguide](#useguide)
* [Styleguide](#styleguide)

## Useguide

### Avoid straying from conventions

It is difficult to know when making a decision about programming is better or worse for yourself. For example, some might find it difficult going about styling their code or setting up their work environment for the project.

In order to make this easier for the user, the user should consider how other developers and programmers will view their code and workflow – In one question, **what will others expect out of your source code?** If it uses strange filetrees, capitalization, or indentation, other developers will be forced to think harder about your code, leading to more fatigue and poor performance.

Synchronizing and conforming to conventions is a self-fulfilling prophecy – the more programmers use conventions the more streamlined each project environment becomes.

With that said, it is up to your group and the community as a whole to use what is most *effective* for programming. That is a different beast entirely, and is the mechanism the drives shifts in conventions. 

Here are a few that should help you.

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

* *Master* hosts the **production** – the code currently being used by the end-user.
* *Release* branches off *develop*, which is given final QA, no new features, and is tagged with versioning.
* *Develop* branches off *master*, and is where developers work to make changes and improvements to code. QA would most often be done here and in *release*.
* *Feature* branches off *develop*. There are many features operating simultaneously – some by solo developers, and some by teams. They are merged into *develop* once finished.
* *Hotfix* branches off *master*, and can be merged into *master* and *develop*.

In comparison to [Github-flow](#Github-flow),  gitflow allows for stricter versioning and segregation of code.

#### Github-flow

> If your code is having only one version in production at all times (i.e. web sites, web services, etc) you may use github-flow. Main reason is that you don't need to complex things for the developer. Once developer finish a feature or finish a bugfix its immediately promoted to production version.
> – Gayan Pathirage via Stack Overflow

```
                 hotfix-sqli
                    -----
                   /     \
 Master  ----------------------------
             \         /             \
              ---------               --- release-2.0
             feat-mobile
```

* *Master* hosts production.
* Any *features*, *hotfixes*, and *releases* branch off *master*.

The appeal of Github-flow is simple:

- Provide an easy to use branch model for **continuous integration (CI)**.
- Each developer is working with the same parent branch of code, allowing for easy collaboration between groups of programmers.
- Much, much less likelihood that any codebase will diverge and cause conflicts.

#### Trunk

Trunk-based development is another model focused on **continuous integration (CI)** that has been around for many years. Facebook uses this model, pushing new, production-ready code every day.

```
             2.x   2.1.x          3.x
Release      ------x-----         -----
            /       \            /
Master   --------------------------------
```

It's very unique in its aspects –

* *Master* is **not** production, and instead is the universal development environment.
* Developers exclusively commit to *master*.
* Each commit is production-ready. Ergo, no commit ever breaks the build.
* The model utilizes **release engineers**, who have specific rights –
	* Branch off *release* branches, which are then sent to *production*
	* **Cherry pick** commits from *master*. Useful for bugs.
* **Multiple productions** can be simultaneously hosted, debugged, and developed on.

### Keep branches up to date

Realistically, there will likely be ongoing features and a release at work for any repository. Here is a snip of a **gitflow** tree.

```
Release                        ----
                              /
Develop      -----------------
                 \     /      \
Feature           -----        ---- feature-mobile
```

In the event that *develop* is updated, such as through a QA commit or merge from *hotfix*, its child branches can be updated with `rebase`.

```
$ git checkout feature-mobile
$ git rebase develop
```

### Delete a branch
 
Once finished with a branch (merged into parent), they can be deleted to keep the repository nice and clean. This must be done locally and remotely.

```
$ git checkout develop
$ git branch -D feature-mobile
$ git push origin --delete feature-mobile
$ git push
```


### Visualize branch flow

Many teams don't unanimously use a git GUI (although I heard they're pretty cool), so it's important that each dev has a visual of the branch flow.

```
$ git config --global alias.lol log --graph --decorate --oneline -20
$ git config --global alias.lola log --graph --decorate --oneline --all -20
```

To modify local aliases, head over to `~/.gitconfig` 

### Rebase commits for readability

Rebasing is a fantastic tool enabling devs to make repo histories more readable and streamlined. If you haven't already, set up git to point to your editor.

```
$ git rebase -i HEAD~1
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

### Properly setup
	
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

This encompasses the best commits messages I find. They are straight & to the point, and give the reader a good idea of what's going on, and what the commit is doing about it.

#### Fixing bugs and closing issues

```
[Subject line]

Originally, [what was happening]

[Back-end explanation of bug]

[Solution]

Closes #XX
```

Issues can be closed directly using a [keyword](https://help.github.com/articles/closing-issues-via-commit-messages/).

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

#### Keep the subject line under 50 chars, and the body under 80
This keeps any logs (shortlog, --oneline) within one line. For reference, 66 columns is considered the most legible for reading. 72 and 80 are popular column widths for programming.

For vimmers, you should know that line numbers and status symbols eat up column space, so it's a good idea to use `set cc=80` to colorize the 80th column in the document, and `set columns` a bit wider. I use 86. To change `cc` color, use `highlight ColorColumn guibg=color`. I use grey19.

#### Line-break the commit body

Add a line break after your desired amount of columns. This improves legibility of the commit message, as it standardizes the rendering of the commit throughout GitHub and the command line. Just like spaces over tabs. **darts out door*\*

#### Use a present tense imperative in the subject line

Good:

* `Implement homepage link in top left corner of all sites`

Bad:

* `Implemented homepage link in corner`
* `Websites now link to homepage in top left corners`
* `Improve UX`

#### Don't blend syntax with English
While useful at conveying information in a shorter amount of characters, this causes the reader to switch gears too often, thinking *Was that latex? Or was that python latex...?*

Good: `Propagate cc to vim configuration files`

Bad: `Propagate cc to [_.]vimrc`

## Styleguide

As a rule of thumb, prioritize the readability of using code conventions. It is ironic attempting to condense code that will be read by programmers. Condensers do it in a single click.
