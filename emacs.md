# emacs.md - A Space Odyssey


I create new repositories on a regular basis in my `~/git`
directory. While I do this I have a running Emacs instance. How do I
then refresh Projectile so that `C-c p p` will list my new git
repository? Simply run:

```
M-x projectile-discover-projects-in-directory
```

I added this file you are reading, `emacs.md` to a new repository
`readme`. I want to add and commit this file with magit. I then
still need to set the GitHub remote repository. I want to do this
also with magit.

Let's add the file first. Run `M-x magit-status`. Then move the
cursor to the untracked file `emacs.md` and stage the file with
`s`. Now we can commit: press `c`, read through the switches and
options. Right now we don't need any of them. We need one action:
commit. So press `c` again. Then enter a descriptive commit message.
Start with a verb. Do not write the verb in past tense. Always write
the verb in present tense. Complete the commit message with `C-c c`.
That's it.

I went to GitHub and created a repository:
git@github.com:cudos/readme.git. So I now need to add this repository
as "remote" to my local repository. I run `M-x magit-remote-add`. For
the remote name I type `origin` and I enter the remote URL. I can
inspect the result with `M-x magit-remote-popup`. Everything's
perfect. Now I can push my pending commits to the remote origin with
`M-x magit-push`.

I tried to find out if it makes sense to create new files with
projectile. No, `C-x C-f`, there's nothing simpler than that.

I know I will open this file, `emacs.md` quite often in the future. So
I set a bookmark to it with `C-x r m`. Then I can reopen `emacs.md`
quite easy through the list of all my bookmarks `C-x r l`. Pretty
cool, hm?

I have my org-mode file `refile.org` open and I want to refile a
`TODO` item. I do it with `C-c C-w`.

I have something that needs to be or I want to look upon somethimes in
the future, not now. I hit `C-c c`. Then I can choose between two
templates for todos (t) and notes (n).
