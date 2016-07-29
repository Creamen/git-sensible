# guide 
A central reference to streamline and homoginize a team's workflow. 

There are many, many ways teams can fail when developing source code, from the management of the team down to the typing of the keys. Hopefully this guide will help improve your programming experience, and make code more sustainable.

This isn't exhaustive. Be proactive in researching and asking others about proper workflow.

With all this said, don't expect to know these methods off the bat. Git is very much an *inductive* language, which means, rather informally, *learn by doing*. You could say this is a compilation of everything we've had to Google, and has improved our workflow. Use what works for you.

* [Useguide](#useguide)
* [Styleguide](#styleguide)

----------

## Useguide

### Rebase commits for readability

Rebasing is a fantastic tool enabling devs to make the history more readable and streamlined

`$ git rebase -i HEAD~1` will open an editor to *interactively* work with commits from the `HEAD` to its second parent.

```
pick f7f3f6d changed my name a bit
r 310154e updated README formatting and added blame
```

Opens an editor to *reword* 310154e's commit message. Useful when accidentally commiting with typos or lackluster language.

```
pick 4e64052 Revise animations to use CSS in homepage
f 4bdc7dc Fix IE compatibility with homepage animations
```

*Fixup* the commit, as if fixing a bug in the previous commit. This discards the commit message.

```
pick 6h49f6s Impl. French language support
s 9n28e0a Impl. Spanish lanuage support
```

Squashes the commit into its parent, improving the readbility of commit history. Subsequently opens an editor to modify their commit messages, which have also been combined.

*Be diligent when rebasing content that has already been pushed, as it creates a new commit and hash, obscuring any issue or comment that referenced that hash.*

### Use --rebase when getting up to date 

```git
git pull --rebase
```

When working in a group, local repos become outdated often, and have to pull down changes made by other devs. `git pull` is made just for this, but by default **causes an extra merge commit just to update the local repo.** This isn't pretty, and gets even nastier with dozens of devs. So, use `git pull --rebase` to cleanly update the repo with what's going on.

### Set up a new computer

1. Sync your profile

    ```git
    git config --global user.name "First Last"
    git config --global user.name "public-email@gmail.com"
    ```
    
    When done correctly, `blame` and `shortlog` will be attributed to everyone correctly. Turn on 2-Factor authentication for that email, as it will be publicly accessible.

2. Generate an encryption key

    Github best [explains how](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/).
    
    After the key is generated and added to your personal account, GitHub will accept the authentication from that computer, and you can work on your groups as normal.
    
    If at any time your keys are compromised (commonly done in someone's /dotfiles repo), just delete them from your account.

### Visualize branch flow

Many teams don't unanimously use a git GUI (although I heard they're pretty cool), so it's important that each dev has a visual of the branch flow.

```
git config --global alias.lol log --graph --decorate --oneline -20
git config --global alias.lola log --graph --decorate --oneline --all -20
```

To modify local aliases, head over to `~/.gitconfig` 

### Write good commit messages

This is the subject of hundreds of blog posts – this one is nothing special. *Tim Pope* even has his own rundown of commit messages.

>As a golden rule, the commit message must contain all the information required to fully understand & review the patch for correctness. Less is not more. More is more.  
>– *Git Commit Good Practice*, OpenStack

Here is a template that I've conformed to over a few months of commiting:

```
[Imperative] [Noun] [Context]

[Expanded context and explanation]
```

#### Fixing bugs and closing issues

```
[Title]

Previously, [what was happening]

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

#### Keep the subject line under 50 chars, and the body under 80
This keeps any logs (shortlog, --oneline) within one line. For reference, 66 columns is considered the most legible text wrap for reading. 72 and 80 are popular columns widths.

For vimmers, you should know that line numbers and status symbols eat up column space, so it's a good idea to use `set cc=80` to colorize the 80th column, and `set columns` a bit wider. I use 86. To change `cc` color, use `highlight ColorColumn guibg=color`. I use grey19.

#### Use a present tense imperative in the subject line
`Fix` – not `Fixes` or `Fixed`.  Commits are commands to change code, not changed code.

#### Line-break the commit body

Add a line break after your desired amount of columns. This improves legibility of the commit message, as it standardizes the rendering of the commit across GitHub and the command line. Just like spaces over tabs. **darts out door*\*

#### Don't blend syntax with English
Moron.

----------


# Styleguide

As a rule of thumb, prioritize the readability of using code conventions. It is ironic attempting to condense code that will be read by programmers. Condensers do it in a single click.
