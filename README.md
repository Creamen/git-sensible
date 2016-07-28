# guide 
A central reference to streamline and homoginize a team's workflow. 

There are many, many ways teams can fail with developing source code,
from the management of the team to the typing on the keys. Hopefully
this guide will help you understand how to use Git

* [Useguide] (#useguide)
* [Styleguide] (#styleguide)

## Useguide

It's important to have a similar workflow in creating source code, so
here's some important tips when working with Git.

### Use --rebase when updating your repo

``` git
git pull --rebase
```

When working in a group, your repo will have to pull down changes made
by other devs. `git pull` is made just for this, but by default **causes
an extra merge commit to be made just for updating your local repo.** This 
isn't pretty, and it gets even nastier with dozens of devs. So, simply 
use `git pull --rebase` to ensure you can cleanly get up to date with 
what's going on.

### Add new computers

1. Sync up your profile (locally)

    ```git
    git config --global user.name "Aug"
    git config --global user.name "dont-doxx-me-bro@gmail.com"
    ```
    
    This is pretty important, because the `blame` and `shortlog` will be 
    attributed to everyone correctly.

2. Generate an encryption key

    Github [explains it best](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/).
    
    After the key is generated and you've added it to your personal account,
    GitHub will accept your authentication from that computer, and you can
    work on your groups as normal.
    
    If at any time your keys are compromised (commonly done in someone's
    /dotfiles repo), just delete them from your account.

You're all set to clone repos and get typing.
