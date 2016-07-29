# guide 
A central reference to streamline and homoginize a team's workflow. 

This isn't exhaustive. Be proactive in researching and asking others about proper workflow.

Also, don't expect to know these conventions off the bat. Git is very much an *inductive* language, which means, rather informally, *learn by observing specific scenarios*. Doing this [effectively](https://github.com/airbnb/javascript#objects--no-new) can convey concepts much quicker than teaching it [deductively](https://en.wikipedia.org/wiki/Deductive_reasoning).

To best harden these concepts within your mind, exercise them [prodecurally](https://en.wikipedia.org/wiki/Procedural_knowledge). Hopefully these topic sentences are relevant enough that they become commonplace within your workflow.

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
git pull --rebase
```

When working in a group, local repos become outdated often, and have to pull down changes made by other devs. `git pull` is made just for this, but by default **causes an extra merge commit just to update the local repo.** This isn't pretty, and gets even nastier with dozens of devs. Use `git pull --rebase` to cleanly update the repo with what's going on.

### Understand git branching models

Using a branching model will amplify the usefullness of branching, allowing each dev to be more creative with creating code and designing changes without stress.

However, there are a variety of models any team can use, and it's important to choose the right one for the group. Here are a few.

*Note – each model defines terms (master, release, etc) differently, so know the context.* With that said, these are constant throughout:

* *Production* is the currently deployed source code being used by the end-user. 
* *Feature* is a branch that works on a single implementation of a new product feature, such as `Mobile Support` or `Color Themes`.

#### Gitflow

While not having an official name, 'gitflow' is the most popular branching model amongst Github developers, while also being the most complicated.

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

* *Production* is *master*
* *Release* is a branch off *develop*, which is given final QA, no new features, and is tagged with versioning. 
* *Develop* is a branch off *master*, and is where developers work to make changes and improvements to code. QA would most often be done here and in *release*.
* *Feature* is a branch off *develop*. There are many features operating simultaneously – some by solo developers, and some by teams. They are merged into *develop* once finished.

#### Github-flow





### Visualize branch flow

Many teams don't unanimously use a git GUI (although I heard they're pretty cool), so it's important that each dev has a visual of the branch flow.

```
git config --global alias.lol log --graph --decorate --oneline -20
git config --global alias.lola log --graph --decorate --oneline --all -20
```

To modify local aliases, head over to `~/.gitconfig` 

### Rebase commits for readability

Rebasing is a fantastic tool enabling devs to make repo histories more readable and streamlined. If you haven't already, set up git to point to your editor.

`git rebase -i HEAD~1` will open an editor to *interactively* work with the `HEAD` and its parent.

```
pick f7f3f6d changed my name a bit
r 310154e updated README formatting and added blame
```

Opens an editor to *reword* the commit message. Useful when accidentally commiting with typos or lackluster language.

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

    ```git
    git config --global user.name "First Last"
    git config --global user.name "public-email@gmail.com"
    ```
    
    When done correctly, `blame` and `shortlog` will be attributed to everyone correctly.
    
- Turn on 2-Factor authentication for your emails, including your GitHub account

	GitHub's hosted information is very accessible. In the past, many accounts have been compromised through [brute force](https://github.com/blog/1698-weak-passwords-brute-forced) and even [reused passwords](https://github.com/blog/2190-github-security-update-reused-password-attack) that were stolen from other websites *last month*. 2-Factor is an easy way to sleep soundly at night.

- Never, ever move around your authentication keys
	
	They are *disposable*. Leaving them up to human error can open an opportunity for an attacker to do monumental damage. Keep them close. Recreate them if you have any doubt. 

### Write good commit messages

>As a golden rule, the commit message must contain all the information required to fully understand & review the patch for correctness. Less is not more. More is more.  
>– *Git Commit Good Practice*, OpenStack

Here is a template that I've conformed to over a few months of commiting:

```
[Imperative] [Noun] [Context]

[Expanded context and explanation]
```

See an example below.

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
