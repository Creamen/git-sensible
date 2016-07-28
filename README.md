# guide 
A central reference to streamline and homoginize a team's workflow. 

There are many, many ways teams can fail with developing source code, from the 
management of the team to the typing of the keys. Hopefully this guide will 
help you understand how to better use Git in a team environment.

This isn't exhaustive. Be proactive in researching and asking others about
proper workflow, even if it's laughing at some poor developer from a post in
/r/programmerhumor.

* [Useguide] (#useguide)
* [Styleguide] (#styleguide)

## Useguide

It's important to have a similar workflow in creating source code, so here's 
some important tips when working with Git.

### Use --rebase when updating your repo

``` git
git pull --rebase
```

When working in a group, your repo will have to pull down changes made by 
other devs. `git pull` is made just for this, but by default **causes an extra 
merge commit to be made just for updating your local repo.** This isn't pretty, 
and it gets even nastier with dozens of devs. So, simply use `git pull --rebase` 
to ensure you can cleanly get up to date with what's going on.

### Add new computers

1. Sync your profile

    ```git
    git config --global user.name "First Last"
    git config --global user.name "fastestwaytoreachme@gmail.com"
    ```
    
    When done correctly, the `blame` and `shortlog` will be attributed to 
    everyone correctly.

2. Generate an encryption key

    Github [explains it best](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/).
    
    After the key is generated and you've added it to your personal account,
    GitHub will accept your authentication from that computer, and you can
    work on your groups as normal.
    
    If at any time your keys are compromised (commonly done in someone's
    /dotfiles repo), just delete them from your account.

You're all set to clone repos and get typing.

### Visualize your branch flow

Many teams don't unanimously use a git GUI (although I heard they're pretty
cool), so it's important that each dev has a visual of the branch flow.

```
git config --global alias.lol log --graph --decorate --oneline -20
git config --global alias.lola log --graph --decorate --oneline --all -20
```

Alternatively in your `~/.gitconfig` or `%userprofile%/.gitconfig`
```git
[alias]
	lol = log --graph --decorate --oneline -20
	lola = log --graph --decorate --oneline --all -20
```

### Commit with ease

### Make atomic commits



### Write better commit messages 

As a template: \[Imperative verb] \[noun] \[context]

Such as,
* Avoid 'type mismatch' errors in ExtendedBeanInfo
* Fix infinite recursion bug in nested @Configuration
* Compensate for Eclipse vs Sun compiler discrepancy

Along with many other authors, Chris Beams has a [monstrous](http://chris.beams.io/posts/git-commit/)
blog post on everything you need to know, but here's some important pointers:

* Use the present tense imperative in the subject line

    It's somewhat difficult to understand why one wouldn't use "Fixed a bug"
    as a commit message here's a oneliner you can recite to yourself:

    While you *did* type code and are likely *doing* something, each commit
    *does something*, so it's illogical to place that commit in the past tense.

    Ultimately, you will always be manipulating commits through rebasing, so it
    becomes very confusing when a developer has to pretend everything was 'done'
    in the past.

* Use the body to give a greater context and in-depth explanation
