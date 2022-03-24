# Git Notes

- Adding git-prompt

```
$ vim .bashrc
...

#
# Lines added for git-prompt
#
git_prompt_sh='/usr/share/git-core/contrib/completion/git-prompt.sh'
if [ -f ${git_prompt_sh} ]; then
  source ${git_prompt_sh}
  export GIT_PS1_SHOWDIRTYSTATE=true
  export GIT_PS1_SHOWUNTRACKEDFILES=true
  export PS1='[\u@\h \W$(declare -F __git_ps1 &>/dev/null && __git_ps1 " (%s)")]\$ '
fi

```

- Generate ssh-key

```
$ ssh-keygen -t ed25519 -C "my_github_email@mail.com"
Generating public/private ed25519 key pair.
Enter file in which to save the key (/home/user/.ssh/id_ed25519): /home/user/.ssh/id_github   
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/user/.ssh/id_github.
Your public key has been saved in /home/user/.ssh/id_github.pub.
The key fingerprint is:
SHA256:SBN2gCVTAlV5by2RfNNbdryQPLboyd27L7xyhjfzk/Q my_github_email@mail.com
The key's randomart image is:
+--[ED25519 256]--+
|  .o==Bo.. ..... |
|    .=.o. + o*. =|
|      o. . +o.++o|
|     . o  +..... |
|      . S.o.o .  |
|           + . o |
|             o. +|
|            o O+E|
|             =.B*|
+----[SHA256]-----+
$ cat /home/alex/.ssh/id_github.pub
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIL8ULrWCXI+Rh+QhD77R7RIcNvytjDwjEN3SFydM5nbc my_github_email@mail.com
```

- Evaluate ssh-agent

```
$ eval "$(ssh-agent -s)"
Agent pid 1887
```

- Add identity to ssh-agent

```
$ ssh-add ~/.ssh/id_github
Identity added: /home/user/.ssh/id_github (my_github_email@mail.com)
```

- Authenticate on GitHub

```
$ ssh -T git@github.com
```

**NOTE: To do this, you need to add the ssh-key to your github configuration, inside the profile settings.**

- Set GitHub user locally

```
$ git config --global user.name "User"
$ git config --global user.email my_github_email@mail.com
```

- Clone repository

```
$ git clone [https://...repository.git]
```

**NOTE: If authentication is required, enter the corresponding values:**
<pre>
Username for 'https://github.com': <i>username</i>
Password for 'https://<i>username</i>@github.com': <i>Personal access token</i>
</pre>

- Create a new branch

```
$ git branch user/new_branch
```

- Switch to a new branch

```
$ git checkout user/new_branch
```

**NOTE: Run both commands as:**
```
$ git checkout -b user/new_branch
```

- Check status

```
$ git status
```

- Review differences

```
$ git diff
```

- Adding new/modified file(s)

```
$ git add new_file
```

- Commit new/modified file(s)

```
$ git commit -m "Adding updated new_file file"
```

- Push new branch to upstream

```
$ git push -u origin user/new_branch
```
