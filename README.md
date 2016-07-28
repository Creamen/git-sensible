# guide 
A central reference to streamline and homoginize a team's workflow. 

There are many, many ways teams can fail when developing source code, from the 
management of the team down to the typing of the keys. Hopefully this guide 
will help improve your programming experience, and make code more sustainable.

This isn't exhaustive. Be proactive in researching and asking others about
proper workflow.

With all this said, don't expect to know these procedures off the bat. Git is 
very much an *inductive* language, which means, rather informally, *learn by 
doing*. You could say this is a compilation of everything we've had to Google, 
and has improved our workflow. Use what works for you.

* [Useguide] (#useguide)
* [Styleguide] (#styleguide)

## Useguide

### Rebase commits for readability

Rebasing is a fantastic tool enabling devs to make the history more readable 
and streamlined.

`$ git rebase -i HEAD~2` will open an editor to work with the commits from the
`HEAD` to its second parent.

```
r f7f3f6d changed my name a bit
r 310154e updated README formatting and added blame
```

Opens an editor to *reword* the commit messages. Useful when accidentally 
commiting with typos or lackluster language.

```
pick 4e64052 Revise animations to use CSS in homepage
f 4bdc7dc Fix IE compatibility with homepage animations
```

*Fixup* the commit, as if fixing a bug in the previous commit. This discards 
the commit message.

```
pick 6h49f6s Impl. French language support
s 9n28e0a Impl. Spanish lanuage support
```

Squashes the commit into its parent, improving the readbility of commit history. 
Subsequently opens an editor to modify their commit messages, which have also
been combined.

*Be diligent when rebasing content that has already been pushed, as it creates
a new commit and hash, obscuring any issue or comment that referenced that 
hash.*

*Additionally, rebasing commmits that have reached master will duplicate the 
amount of contributions made by the authors. Those commits are there, but have
been made unreachable. The only fix to this is to delete the repo. Be careful
when rebasing master commits numerous times (e.g., using the BFG).*

### Use --rebase when getting up to date 

``` 
git pull --rebase
```

When working in a group, local repos become outdated often, and have to pull 
down changes made by other devs. `git pull` is made just for this, but by 
default **causes an extra merge commit to be made just for updating the local 
repo.** This isn't pretty, and gets even nastier with dozens of devs. So, using 
`git pull --rebase` will ensure the repo will cleanly get up to date with 
what's going on.

### Set up a new computer

1. Sync your profile

    ``` git
    git config --global user.name "First Last"
    git config --global user.name "frequented-email@gmail.com"
    ```
    
    When done correctly, `blame` and `shortlog` will be attributed to 
    everyone correctly. Turn on 2-Factor authentication for that email, as
    it will be publicly accessible.

2. Generate an encryption key

    Github best [explains how](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/).
    
    After the key is generated and added to your personal account,
    GitHub will accept the authentication from that computer, and you can
    work on your groups as normal.
    
    If at any time your keys are compromised (commonly done in someone's
    /dotfiles repo), just delete them from your account.

### Visualize branch flow

Many teams don't unanimously use a git GUI (although I heard they're pretty
cool), so it's important that each dev has a visual of the branch flow.

```
git config --global alias.lol log --graph --decorate --oneline -20
git config --global alias.lola log --graph --decorate --oneline --all -20
```

To modify local aliases, head over to `~/.gitconfig` in Linux, and
`%userprofile%\.gitconfig` in Windows.

### Write better commit messages

This is the subject of hundreds of blog posts -- this explanation is nothing 
special. *Tim Pope* even has his own rundown of commit messages.

As a golden rule, the commit message must contain all the information 
required to fully understand & review the patch for correctness. Less is not 
more. More is more. (Thanks to [OpenStack](https://wiki.openstack.org/wiki/GitCommitMessages))

It's more effective to learn commit messages *inductively*, so here's a great
example:

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

Here is a template that I've conformed to over a few months of commiting:

```
[Imperative verb] [noun] [context]

[Expanded context and explanation]

Closes #000

# Please enter the commit message for your changes. Lines starting              
# with '#' will be ignored, and an empty message aborts the commit.             
# On branch master...
```

Along with many other authors, Chris Beams has a [monstrous](http://chris.beams.io/posts/git-commit/)
blog post on everything you need to know, but here's some important pointers:

* Use a present tense imperative in the subject line
* Keep the subject line under 50 chars, and the body under 80 (66-72-80 are 
popular)
    - This keeps any logs (shortlog, --oneline) within one line
* Use the body to give a greater context and in-depth explanation
* Use a [keyword](https://help.github.com/articles/closing-issues-via-commit-messages/)
 to close an issue.

# Styleguide

As a rule of thumb, prioritize the readability of code using code convensions.

The compression and speed of the code is second priority, as those problems can 
be solved with a compression tool.

