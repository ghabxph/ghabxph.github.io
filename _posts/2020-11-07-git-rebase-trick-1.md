---
layout:     post
title:      "Git Rebase Tricks: Segregate a file from set of commits and squash them into one commit"
categories: git rebase
date:       2020-11-07 23:59:00 +0800
comments:   true
---

## What's the Goal?

I have a file at the root of my repo called README.md. For your case, it can be any other  file.  My
goal is to remove README.md on set of commits, and instead move it into one single  commit  and  the
rest of the recent commits shall be on top of it safely.

I want my `README.md` to be after `develop` branch. And  then  the  `new-history`  branch  which  is
derived from the latest commit should rebase to the committed `README.md` commit hash.

## Procedure Summary

1. Create a branch called `new-hist`
2. Copy `README.md` elsewhere in your file system
3. Checkout to `develop` and create `readme.md` branch.
4. Move `README.md` back to its original place to update it.
5. Do a git add and commit `README.md`.
6. Switch back to `new-history`
7. Rebase to `readme.md` branch. Expect merge conflict.
8. Clearing the merge conflict, do this until things are all done:
   `git checkout --ours README.md && git add README.md && git rebase --continue`
9. Think and reflect to what you have read and learn, and use it to become a better commitizen. lol.

## Details

To begin, let's start by checking my entire commit history.

```
 gabriel@manjaro   feature/gabriel-dummy  glad
* Sat Nov 7 20:30:09 2020 +0800 -- f157251 --> docs: added commit rules -- (HEAD -> feature/gabriel-dummy, origin/feature/gabriel-
dummy)
* Sat Nov 7 18:05:20 2020 +0800 -- 1a8fb0e --> chore(ssh-port-setup): moved ssh-setup into ssh/ssh-port-setup --
* Sat Nov 7 18:02:51 2020 +0800 -- 9b32b88 --> docs(ssh-port-setup): done --
* Sat Nov 7 15:03:41 2020 +0800 -- 7c2450c --> chore: created server-management folkder --
* Sat Nov 7 13:55:17 2020 +0800 -- 1e18833 --> docs: ansible folder --
* Fri Nov 6 23:37:59 2020 +0800 -- efe53b6 --> feat(ansible): ansible --
* Fri Nov 6 22:49:36 2020 +0800 -- 664c197 --> docs: note about folder structure and markdown writing rule --
* Fri Nov 6 22:30:01 2020 +0800 -- fb980c3 --> chore: initial readme, emacs project marker (.projectile), and changed to yarn -- (
origin/master, origin/feature/gabriel, origin/develop, origin/HEAD, master, feature/gabriel, develop)
* Wed Aug 26 19:56:23 2020 +0800 -- e9638ac --> feat: refactor --
* Mon Aug 24 20:23:14 2020 +0800 -- 97314e1 --> feat: go lang login grpc --
* Fri Aug 21 08:00:04 2020 +0800 -- a18f550 --> feat: initial commit --
 gabriel@manjaro   feature/gabriel-dummy  
```

And these commits shows the entire history of my README.md file

```
 gabriel@manjaro   feature/gabriel-dummy  git blame README.md | awk '{print "glad | grep " substr($1, 1, 7)}' | sort | uniq | sh
* Fri Nov 6 22:49:36 2020 +0800 -- 664c197 --> docs: note about folder structure and markdown writing rule --
* Sat Nov 7 15:03:41 2020 +0800 -- 7c2450c --> chore: created server-management folkder --
* Sat Nov 7 20:30:09 2020 +0800 -- f157251 --> docs: added commit rules -- (HEAD -> feature/gabriel-dummy, origin/feature/gabriel-dummy)
* Fri Nov 6 22:30:01 2020 +0800 -- fb980c3 --> chore: initial readme, emacs project marker (.projectile), and changed to yarn -- (origin/master, origin/feature/gabriel, origin/develop, origin/HEAD, master, feature/gabriel, develop)
 gabriel@manjaro   feature/gabriel-dummy 
```

`fb980c3` is `develop`, `master`, or `feature/gabriel` branch  for  emphasis.  I'll  just  name  it
`develop` branch. My goal is to create a commit  after  `develop`  branch.  The  commit  after  the
`develop` branch are the combination of three other commits that affect `README.md`.  For emphasis,
these are: `f157251`, `7c2450c`, and `664c197`.

We will begin by creating a branch called `new-hist`.  It  should  be  on  top of our latest commit
`f157251`.

```
 gabriel@manjaro   feature/gabriel-dummy  git checkout -b new-hist    
 gabriel@manjaro   feature/gabriel-dummy  git branch
  develop
  feature/gabriel
  feature/gabriel-dummy
  master
* new-hist
  readme.md
  gabriel@manjaro   feature/gabriel-dummy  glad -n1
* Sat Nov 7 20:30:09 2020 +0800 -- f157251 --> docs: added commit rules -- (HEAD -> feature/gabriel-dummy, origin/feature/gabriel-
dummy, new-hist)
```

Now that we have created `new-hist` branch, let's begin by copying `README.md` at the back of our
repo (../) and then switch to `develop` branch. Create `readme.md` branch so that it'll be on top
of develop branch. Next is move back `README.md` to the repo and then add and commit the changes.

I like signing all of my commits, so I consistently add -S flag. Do not add -S flag if you do not
know how to sign commits. I might create a special tutorial for this one. It's not really that
hard, but we'll save that for later.

```
 gabriel@manjaro   readme.md  cp README.md ../
 gabriel@manjaro   readme.md  git checkout develop
Switched to branch 'develop'
Your branch is up to date with 'origin/develop'.develop
 gabriel@manjaro   develop  git checkout -b readme.md
Switched to a new branch 'readme.md'
 gabriel@manjaro   develop  mv ../README.md .
 gabriel@manjaro   develop ±  git add README.md    
 gabriel@manjaro   develop ✚  git commit -S -m "qqqqqqqq"
[develop 5eb20cb] qqqqqqqq
 1 file changed, 126 insertions(+), 4 deletions(-)
 rewrite README.md (89%)
 gabriel@manjaro   develop  
```

Now what's next. I'd like to show you the state of my branches. As you'll see below, we created a
commit with message `qqqqqqqq`. I did not take the name very seriously as I will make another
article explaining how to revise this commit when this commit is not on top. That is made
possible through rebase. We'll get to that later!

So what happened is that, there's a commit of `qqqqqqqq` on top of develop branch.
```
 gabriel@manjaro   develop  glad
* Sun Nov 8 01:04:55 2020 +0800 -- 5eb20cb --> qqqqqqqq -- (HEAD -> readme.md)
| * Sat Nov 7 20:30:09 2020 +0800 -- f157251 --> docs: added commit rules -- (origin/feature/gabriel-dummy, new-hist, f
eature/gabriel-dummy)
| * Sat Nov 7 18:05:20 2020 +0800 -- 1a8fb0e --> chore(ssh-port-setup): moved ssh-setup into ssh/ssh-port-setup --
| * Sat Nov 7 18:02:51 2020 +0800 -- 9b32b88 --> docs(ssh-port-setup): done --
| * Sat Nov 7 15:03:41 2020 +0800 -- 7c2450c --> chore: created server-management folkder --
| * Sat Nov 7 13:55:17 2020 +0800 -- 1e18833 --> docs: ansible folder --
| * Fri Nov 6 23:37:59 2020 +0800 -- efe53b6 --> feat(ansible): ansible --
| * Fri Nov 6 22:49:36 2020 +0800 -- 664c197 --> docs: note about folder structure and markdown writing rule --
|/  
* Fri Nov 6 22:30:01 2020 +0800 -- fb980c3 --> chore: initial readme, emacs project marker (.projectile), and changed to yarn -- (
origin/master, origin/feature/gabriel, origin/develop, origin/HEAD, master, feature/gabriel)
* Wed Aug 26 19:56:23 2020 +0800 -- e9638ac --> feat: refactor --
* Mon Aug 24 20:23:14 2020 +0800 -- 97314e1 --> feat: go lang login grpc --
* Fri Aug 21 08:00:04 2020 +0800 -- a18f550 --> feat: initial commit --
 gabriel@manjaro   develop  
```

Now the rebase part!. Simply go back to `new-hist` branch and rebase to `readme.md`. That's just
simply. Expect merge conflict. What's the fix?

```
 gabriel@manjaro   readme.md  git checkout new-hist 
Switched to branch 'new-hist'
 gabriel@manjaro   new-hist  git rebase readme.md 
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
error: could not apply 664c197... docs: note about folder structure and markdown writing rule
Resolve all conflicts manually, mark them as resolved wgit checkout --ours README.md && git add README.md && git rebase --continueith
"git add/rm <conflicted_files>", then run "git rebase --continue".
You can instead skip this commit: run "git rebase --skip".
To abort and get back to the state before "git rebase", run "git rebase --abort".
Could not apply 664c197... docs: note about folder structure and markdown writing rule
 ✘ gabriel@manjaro  ➦ 5eb20cb ±✚ >R>  
```

And yup. As I expect. The fix is by doing `git checkout --ours README.md`. What it does is that the
HEAD which is the target commit where you do the rebase (`readme.md` branch). is the one that we
will follow instead the item that we are in. I made the command just one-liner to make things
easier for all.

```
 gabriel@manjaro  ➦ 5eb20cb ±✚ >R>  git checkout --ours README.md && git add README.md && git rebase --continue
Updated 1 path from the index
dropping f15725139a3b6604ee1fba9b46ca9268f32f949d docs: added commit rules -- patch contents already upstream
Successfully rebased and updated refs/heads/new-hist.
 gabriel@manjaro   new-hist  glad
 gabriel@manjaro   new-hist  glad | more
* Sat Nov 7 18:05:20 2020 +0800 -- 625b6ca --> chore(ssh-port-setup): moved ssh-setup into ssh/ssh-port-setup -- (HEAD -> new-hist
)
* Sat Nov 7 18:02:51 2020 +0800 -- 82d2e24 --> docs(ssh-port-setup): done --
* Sat Nov 7 15:03:41 2020 +0800 -- 853164a --> chore: created server-management folkder --
* Sat Nov 7 13:55:17 2020 +0800 -- 719630f --> docs: ansible folder --
* Fri Nov 6 23:37:59 2020 +0800 -- 1636c71 --> feat(ansible): ansible --
* Sun Nov 8 01:04:55 2020 +0800 -- 5eb20cb --> qqqqqqqq -- (readme.md, develop)
| * Sat Nov 7 20:30:09 2020 +0800 -- f157251 --> docs: added commit rules -- (origin/feature/gabriel-dummy, feature/gabriel-dummy)
| * Sat Nov 7 18:05:20 2020 +0800 -- 1a8fb0e --> chore(ssh-port-setup): moved ssh-setup into ssh/ssh-port-setup --
| * Sat Nov 7 18:02:51 2020 +0800 -- 9b32b88 --> docs(ssh-port-setup): done --
| * Sat Nov 7 15:03:41 2020 +0800 -- 7c2450c --> chore: created server-management folkder --
| * Sat Nov 7 13:55:17 2020 +0800 -- 1e18833 --> docs: ansible folder --
| * Fri Nov 6 23:37:59 2020 +0800 -- efe53b6 --> feat(ansible): ansible --
| * Fri Nov 6 22:49:36 2020 +0800 -- 664c197 --> docs: note about folder structure and markdown writing rule --
|/  
* Fri Nov 6 22:30:01 2020 +0800 -- fb980c3 --> chore: initial readme, emacs project marker (.projectile), and changed to yarn -- (
origin/master, origin/feature/gabriel, origin/develop, origin/HEAD, master, feature/gabriel)
* Wed Aug 26 19:56:23 2020 +0800 -- e9638ac --> feat: refactor --
* Mon Aug 24 20:23:14 2020 +0800 -- 97314e1 --> feat: go lang login grpc --
* Fri Aug 21 08:00:04 2020 +0800 -- a18f550 --> feat: initial commit --
 gabriel@manjaro   new-hist  
```

**Actually, that's all.** You successfully segregated one single file to a set of commits. This  is
very significant for writing a much more meaningful commit message while sacrificing  the  logs  of
work that you have done. In my opinion, log  work  should  not  be  monitored  through  git  as  it
encourages consistently dirty commits. In my opinion, commit messages should be clear and in point.

What's next below is to do some comparison against `feature/gabriel-dummy` and `new-hist` to  check
whether it worked the way we want it.

```
 gabriel@manjaro   new-hist  git branch | more
  develop
  feature/gabriel
  feature/gabriel-dummy
  master
* new-hist
  readme.md
 gabriel@manjaro   new-hist  git diff 5eb20cb README.md | more
 gabriel@manjaro   new-hist  git checkout feature/gabriel-dummy 
Switched to branch 'feature/gabriel-dummy'
Your branch is up to date with 'origin/feature/gabriel-dummy'.
 gabriel@manjaro   feature/gabriel-dummy  git diff 664c197 README.md | more
diff --git a/README.md b/README.md
index 19f9653..6f2a3de 100644
--- a/README.md
+++ b/README.md
@@ -1,13 +1,106 @@
+
+## Commit Policy
+
+The purpose of this policy  is to segregate source code commits from documentation changes
+and other non-source changes.

+##CONFIDENTIAL##
 gabriel@manjaro   feature/gabriel-dummy  
```

**Nice, isn't it?** We were able to take full control of history and remove README.md on  the  rest
of commits that we don't want it to belong. My goal in this guide is to just put the  README.md  on
one single commit from the past, and other recent commits shall be on top of it.

I tell you. git is so powerful. It is even more powerful, if you explore further on how to use it.

Thank you very much for reading.
