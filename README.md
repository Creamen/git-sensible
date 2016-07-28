# guide 
A central reference to streamline and homoginize a team's workflow. 

There are many, many ways teams can fail with developing source code, from the 
management of the team down to the typing of the keys. Hopefully this guide 
will help improve your programming experience.

This isn't exhaustive. Be proactive in researching and asking others about
proper workflow, even if it's laughing at some poor developer from a post in
/r/programmerhumor.

With all this said, don't expect to know these procedures off the bat. Git is very much an *inductive* language, which means, rather informally, *learn by 
doing*. You could say this is a compilation of everything we've had to Google, 
and has improved our workflow. Use what works for you.

* [Useguide] (#useguide)
* [Styleguide] (#styleguide)

## Useguide

It's important to have a similar workflow in creating source code, so here's 
some important tips when working with Git.

### Use --rebase when updating the repo

``` git
git pull --rebase
```

When working in a group, repos become outdated often, and have to pull down 
changes made by other devs. `git pull` is made just for this, but by default
**causes an extra merge commit to be made just for updating the local repo.**
This isn't pretty, and gets even nastier with dozens of devs. So, using 
`git pull --rebase` will ensure the repo will cleanly get up to date with 
what's going on.

### Add a new computer

1. Sync your profile

    ``` git
    git config --global user.name "First Last"
    git config --global user.name "fastestwaytoreachme@gmail.com"
    ```
    
    When done correctly, the `blame` and `shortlog` will be attributed to 
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

```
[Imperative verb] [noun] [context]

[Expanded context and explanation]

Closes #00

# Please enter the commit message for your changes. Lines starting              
# with '#' will be ignored, and an empty message aborts the commit.             
# On branch master...
```

It's more effective to learn commit messages *inductively*, so here's some
examples:

* Avoid 'type mismatch' errors in ExtendedBeanInfo
* Fix infinite recursion bug in nested @Configuration
* Compensate for Eclipse vs Sun compiler discrepancy

Along with many other authors, Chris Beams has a [monstrous](http://chris.beams.io/posts/git-commit/)
blog post on everything you need to know, but here's some important pointers:

* Use a present tense imperative in the subject line
* Keep the subject line under 50 chars, and the body under 80
    - Keep any logs (shortlog, --oneline) within one line
* Use the body to give a greater context and in-depth explanation
* For issues, you can close an issue directly using a [keyword](https://help.github.com/articles/closing-issues-via-commit-messages/)

### Commit with ease

### Make atomic commits
