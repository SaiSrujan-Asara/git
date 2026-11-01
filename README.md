"""Git commits are complete snapshots of your project's tracked state, but Git optimizes storage by reusing unchanged content from previous commits"""

git init (initialise the local repo)
git config --global user.name "rich"
git config --global user.email "rich@rich.com"
git config --global core.editor "code"
git config --list

==============================
Unstaged (Red): Tracked files you've modified, but haven't run git add on yet

Staged (Green): Tracked files you've modified and run git add on, making them ready to commit.

Untracked (Red): Brand-new files that Git has never seen before (which, as we just talked about, git diff ignores until you track them).
============================
GIT ADD & COMMIT
===========================
1) git add rich.txt
2) git add . (recommended to make use git instead of handover process to shell)
3) git add * (its not recommended since it hand over process to shell it may ignore .gitignore)
4) git commit -m "commit with message"
5) git comiit -am "commit all the files which under staged it can modified but tracked and deleted files"
git commit -am will automatically stage and commit all tracked files that have been modified or deleted, but it will ignore brand new, untracked files.
==============================
GIT REMOVE & RESTOR
==============================

a) --> git rm --cached rich.txt  
it will remove file from git tracking (it will make file to workingdir) and no change in content of files of that file
----
b) --- git rm rich.txt
it will remove file from git traking and also form working dir (deleted from system)
---
b) --> git restore --staged rich.txt 
it will go back to workdir from staging area but it will be under tracking and no content of file will changes
====================================
-------------------
GIT RESTORE
-------------------
====================================
c) --> git restore rich.txt 
it will remove unstaged content from file i.e if you add content to file but it was not added to stge/index now if you restore that file all the content is removed back to previous state.

When you would use it:
If you made a bunch of messy changes in your working directory but it was not staged and want to instantly wipe them all out and reset your files back to your latest commit, git restore . it's safer and clearer.
=================================
=================================
----------------
GIT RESET
----------------
=================================
=================================
Main intention of git reset is to remove the commit from the Local git repo History and it had Types (soft, mixed and hard) and also we can remove collection of commits at single point (for example if we use HEAD~1 it remove latest commit if we use HEAD~3 it will remove latest 3 commits)

--------
[ Commit #1 ] <--- HEAD moves like this if used HEAD~1 during reset
    |
    v
[ Commit #2 ] (The one you are undoing)
    ├── --soft  ==> Changes stay in Staging Area (Green)
    ├── --mixed ==> Changes move to Working Directory (Red)
    └── --hard  ==> Changes are DELETED completely
============================================================================================
git reset --hard cec3499d2b32569a1bc8e2fc01d7e1ee55d3a73c
HEAD is now at cec3499 ----------------------

so if we use git reset it will remove all commit before above commit id

----------------------------------------------

d) --> git reset --soft HEAD~1
it will remove latest commit locally and stages the file without modifiying the conetent of file (mainly used to modify or rewrite the commit message without modify content)

---->  What happens with git reset --soft HEAD~3?
History goes back 3 commits: Those last 3 commits are wiped from your project history.

All changes go straight to the Staging Area (Green): Instead of dumping your code into the Working Directory as red files, Git takes all the code changes from all 3 commits and automatically stages them.

If you run git status right after git reset --soft HEAD~3, all your modified files will be green, sitting happily in the loading dock ready to be committed again.

Why is --soft with HEAD~3 so popular? (Squashing Commits)

This is the ultimate tool for "squashing" commits.

Imagine you spent the afternoon making 3 messy commits:

Commit 1: "typo fix"

Commit 2: "another typo fix"

Commit 3: "actually fixing the bug"
=============================================================================================
e) ---> git reset --mixed HEAD~1

it will remove latest commit locally and unstages file without modify file contents (that is file be in moved backto working dir where it will move out of tracking without file content modification)

---> HEAD~3 (Time Travel): Step-by-Step Breakdown:

Git looks at your current position and jumps backward 3 commits in history. It essentially deletes the last 3 commits from your project's recent timeline.

mixed (Where the code goes):
Since it's a mixed reset, Git doesn't destroy your code. Instead, it scoops up all the code changes from those 3 deleted commits and dumps them right back into your Working Directory (as red, uncommitted changes).

What does this look like in practice?
Imagine you made 3 separate commits earlier today:

Commit 3: "Fix login bug"

Commit 2: "Add navbar"

Commit 1: "Update homepage"

If you run git reset --mixed HEAD~3:

All 3 of those commits vanish from your git log.

If you type git status, you will see every single file modified in those last 3 commits sitting there in red in your Working Directory.

Why would you ever do this?

This is what developers use when they realize: "Man, I made a bunch of messy commits today, and I want to undo them all, keep my code, but re-organize and re-commit everything cleanly as one big commit (or a different set of commits)."

You keep all your work, but you erase the history of those last 3 saves.
=========================================================================================
e) ---> git reset --hard HEAD~1

it will remove latest commit locaaly from local (i.e) if any file had content that content will be removed and if any new files created in this commit those will be deleted complexly from the system. (Basically it will move back to old commit - 1 )

---> git reset --hard HEAD~3
it will wipe out every thing including file content and if any new files created till latest 3 commits from local git history. 

===============================
SUMMARY
==============================
Quick Recap of all 3 for HEAD~3:
git reset --soft HEAD~3  History goes back 3 commits. Code goes to Staging Area (Green).
git reset --mixed HEAD~3 (or just git reset HEAD~3) History goes back 3 commits. Code goes to Working Directory (Red).
git reset --hard HEAD~3  History goes back 3 commits. Code is DELETED forever.

=============================
HEAD & (DETACH HEAD)
============================
HEAD is simply a pointer (or a bookmark) that tells Git: "This is where you currently are in your project history."

Think of your commit history like a stack of books or a timeline:

Every time you make a commit, HEAD automatically moves forward to sit right on top of your newest commit.

HEAD always represents "right now"—the exact snapshot of the code your working directory is currently based on.

What does HEAD~1 mean?

HEAD~1 means the parent of your current commit — one commit before HEAD.

For example:


text
A → B → C (HEAD)
HEAD = Commit C

HEAD~1 = Commit B

HEAD~2 = Commit A

--------------------
Detached HEAD

When you run git checkout HEAD~3 (or check out any past commit), Git puts you into a special state called Detached HEAD.

What it means: You are now looking at history from a past point in time. Your branch pointer (master or main) is still sitting safely in the present, but your timeline cursor (HEAD) has unhooked itself and is floating in the past.

The danger: If you write code and make commits while in a "Detached HEAD" state, those commits won't belong to any branch. If you switch back to master, those new commits can get lost!

====================
GIT LOG
====================
a) ----> git log: list all the history of commits who, when and commit id and id

git log 
commit 566753e4f340de5c667910727a0442548d379734 (HEAD -> master)
Author: rich <rich@rich.com>
Date:   Thu Oct 29 17:29:09 2026 +0530
added f3.txt 3

--------------------
b) ----> git log -n 3             Print latest 3 commits from the entire logs
c) ----> git log --oneline        print all commits in oneline which include commit id and commit text
d) ----> git log --author="rich"  print all commits for specific user
e) ----> git log -p rich.txt      print all commits of specific file instead of complete history and it contains the what changes it had done
f) ----> git log --stat           print all commits with file changes
j) ----> git blame rich.txt       print each line of file content with commit 

git blame rich.txt
^dc3634e (rich 2026-10-29 11:22:56 +0530 1) you will get 45lpa
a9549709 (rich 2026-10-29 11:35:19 +0530 2) 45lpa
a9549709 (rich 2026-10-29 11:35:19 +0530 3) 45lpa



====================
GIT SHOW
====================
a) if we want to know what chages are done under git commit we can see using GIT SHOW

git show <commit-id>

git show dc3634efdf7910e1a0cbf2cfc8a0f8a5346169ef
commit dc3634efdf7910e1a0cbf2cfc8a0f8a5346169ef
Author: rich <rich@rich.com>
Date:   Thu Oct 29 11:22:56 2026 +0530

    added my fist file to repo

diff --git a/rich.txt b/rich.txt
new file mode 100644
index 0000000..3efbfb5
--- /dev/null
+++ b/rich.txt


========================
git show is specific to commit and git log -p file is specific to file commits
=======================

=============================
GIT DIFF
============================
a) git diff (dif b/w all tracked files within workdir and staging) (workdir ----> staging):-

git diff only works on tracked files.

it will get all the difference of all tracked files from workdir comapare with staging (Unstaged edits (Working Dir vs Stage))

git diff 

diff --git a/f3.tt b/f3.tt
index 4bbfc48..d914678 100644
--- a/f3.tt
+++ b/f3.tt
@@ -1,25 +1,6 @@
 45
-3
-3
-1
-6
+become rich
 you will get 45lpa
-you will earn 2 lpm
-1 
+you will earn 2 lpm 

---------

2. git diff --staged (Staging Area --> Last Commit)What it compares: 

Your Staging Area (green changes) against your Last Commit (HEAD).
When to use it: You ran git add on some files, and you want to double-check everything that is bundled up and ready to be committed before you type 

git diff --staged

---------

3. 3. git diff HEAD (Working Directory ---> Last Commit)

Your Working Directory (everything you've changed, both staged and unstaged(modified tracked file)) directly against your Last Commit.
When to use it: You want a full overview of all changes you've made since your last commit, regardless of whether they are staged or unstaged(modified tracked files)

--------

4. Compare two specific commits:             git diff <commit-id-1> <commit-id-2>
5. Compare two different branches:           git diff main feature-branch
6. See changes in a single file only:        git diff r1.txt


==================================
GIT REMOTE
=================================

1) git remote 
origin

git remote command will give the nick name to the remote server git url 

A). Convenience (Typing is hard, nicknames are fast)
Imagine if instead of typing: git push origin main

Every single time you wanted to push code, you had to type this: git push https://github.com/my-super-long-company-name/our-massive-enterprise-repository-project.git main

B). Multi-Remote Flexibility (Working with multiple servers)
In professional software development, a single local project often connects to multiple remote servers at the same time.

If Git didn't use nicknames, it wouldn't know which server you wanted to talk to. With nicknames, you can give each server its own distinct label:

origin: Points to your personal fork or your team's main repository on GitHub.

upstream: Points to the original open-source project repository managed by another company or foundation.

backup-server: Points to a private internal server your company uses for extra data redundancy.

-----------------

1) git remote -v

origin  https://github.com/SaiSrujan-Asara/git.git (fetch)
origin  https://github.com/SaiSrujan-Asara/git.git (push)

git remote -v is the command you use to check where your local project is connected on the internet.

The -v stands for verbose (which just means "show me the details").

----------------
2) git remote rename origin remote

git remote -v
remote  https://github.com/SaiSrujan-Asara/git.git (fetch)
remote  https://github.com/SaiSrujan-Asara/git.git (push)

git remote rename is the command you use when you want to change the nickname of a remote repository on your local machine.

While origin is the universal, default nickname for your main remote repo, there are times when you might want to change it (or when a team standard requires something different).

Why Would You Use This? (Real-World Scenarios)

Working with Open Source Forks (The origin vs upstream pattern):

When you "fork" someone else's repo on GitHub, origin usually points to your personal copy of the repo.

If you want to pull future updates from the original creator's repo, you add it as a remote and often name it upstream to keep them clearly separated (origin for your fork, upstream for the original).

Cleaning up messy nicknames:

If someone previously set up a remote with a weird or confusing name (like github-server-backup-2024), you can quickly rename it to something clean like origin

--------------
3) git remote remove origin :- Used remove the remote connection
----------
4) git remote add (Creating a new connection)
What it does: Creates a brand-new nickname and links it to a URL.

When to use it: When your project has no remote connection yet, or when you want to add a second remote (like adding an upstream repo alongside your origin).

Example:  git remote add origin https://github.com/my-username/my-repo.git

What happens if you use it wrong: If you try to run git remote add origin <url> when a remote named origin already exists, Git will throw an error:

fatal: remote origin already exists.

----------

5) git remote set-url (Updating an existing connection)

What it does: Modifies the URL of an already existing remote nickname.

When to use it: When the remote name (like origin) already exists, but you need to change its web address (e.g., you renamed your GitHub repository, changed your username, or switched from HTTPS to SSH keys like git@github.com:...).

Example: git remote set-url origin https://github.com/my-username/new-repo-name.git

The Problem: Local Branch vs. Remote Branch Connection
When you create a local branch called main, it exists entirely on your computer. When you run git push origin main for the very first time, Git asks:

"Okay, you want me to push to origin, but should this local main branch link up with origin/main, or origin/production, or some other branch on the server?"

Git refuses to guess. It forces you to explicitly establish a permanent partnership between your local branch and the remote-tracking branch.

What --set-upstream (or -u) Does
When you run:

git push --set-upstream origin main

(or its short version: git push -u origin main), you are doing two things at once:

Pushes your code: Uploads your commits to origin on GitHub.

Links the branches: Tells Git, "From now on, permanently bind my local main branch to origin/main."

Why you only have to do it ONCE
Once you run that command the first time, the partnership is locked in.

The First Time: git push -u origin main (Establishing the link)

Every Time After: Just git push (Because Git now remembers that main connects to origin/main, so you don't have to spell it out anymore!)

=================================
=================================
GIT BRANCH
=================================
=================================
A Git branch is a separate line of development in a Git repository. It lets you work on a new feature or fix a bug without affecting the main code.

Think of it like making a separate copy of your work where you can experiment safely.

Example
Imagine you're developing a website.

main — contains the stable code.

feature-login — where you build a login page.

bugfix-header — where you fix a header issue

---
1) git branch :- list local branch
2) git branch -r :- list remote branch
3) git branch -a :- list local and remote branch
4) git branch newbr :- create new branch locally
5) git branch -d newbr :- delete branch locally (soft delete)
6) git branch -D newbr :- delete branch forcefully in Local (Hard delete)
6) git branch -m b1 b10 :- change branch name from b1 to b10
7) git checkout -b b20 :- create new branch b20 and switch to new branch
8) git push -u origin b10 :- push local branch to remote 
Total 0 (delta 0), reused 0 (delta 0), pack-reused 0
remote: 
remote: Create a pull request for 'b10' on GitHub by visiting:
remote:      https://github.com/SaiSrujan-Asara/git/pull/new/b10
remote: 
To https://github.com/SaiSrujan-Asara/git.git
 * [new branch]      b10 -> b10
branch 'b10' set up to track 'origin/b10' 

9) git push origin -d b30  :- delete remote branch (this works only for remote)
To https://github.com/SaiSrujan-Asara/git.git
 - [deleted]         b30

10) git branch <new-branch-name> <commit-hash> : create branch out of commit id
11) git branch branchname v1.0.0 :- create branch out of tag
12) git checkout -b branchname commitid :- create branch using commit id and checkout into it
13) git checkout -b branchname v1.0.0 :- create branch using tag and checkout into it
==============================================
==============================================
Localy we have 2 branches b1 and b2. If the content from b1 need to get to b2 then we cant pull it instead we need to merge b1 to b2 by swithing to b2 branch and we can perform merger or rebase

If we need content from remote branch to local  then we can use git pull and the branch of remote branch (so basicaaly here it will merge the remote branch to local branch if we use pull)

==============================================
=============================================
GIT REMOTE TRACKING BRANCHES
============================================
============================================
A remote-tracking branch is a local reference that tells you the state of a branch on a remote repository the last time you communicated with that remote.

For example, imagine you have a GitHub repository:

GitHub (Remote repository)

main

development

Your computer (Local repository)

main

Local branch
origin/main

Remote-tracking branch

Notice these two:

main → Your local branch.

origin/main → Your local record of the remote repository's main branch.

Important: origin/main is not the actual branch on GitHub. It is a local reference representing the last-known position of that remote branch.

git fetch:- Updates your knowledge of the remote repository without merging changes into your local branch.

Remote-tracking branches are generally created and updated automatically by Git

+----------------------+-------------------------------------------------------+
| Purpose              | How it helps                                          |
+----------------------+-------------------------------------------------------+
| Track remote changes | Know about new commits on GitHub                      |
| Detect incoming      | Know what you haven't downloaded into your branch     |
| Detect outgoing      | Know what you haven't pushed                          |
| Compare branches     | Identify differences between local and remote         |
| Synchronization      | Help determine whether branches are up to date        |
+----------------------+-------------------------------------------------------+

=============================================
GIT MERGE VS GIT REBASE
=============================================
GIT MERGE

1) if we want to integrate the changes of one branch to another branch we can use git Merge/rebase

In merge method if we have branch main and feat1. If we want to merge main branches changes to feature branch then we need to checkout  feat1 branch then we can 

git merge main

with this command we will get all the commits of main to feature and it will ask for comment. This is for Merge Commit. Once comment is provided it will merge with merge commit.

now if we check the commits all the commits from the main branch can be visible and also one extra commit(Merge Commit) will be there

--------------GIT MERGE----------------------------
i have branch b1 and b2 both have same commits up to date

now in b1 i have commit a and commit b

now in b2 i have commit c

*********  if i merge b1 to b2 it look like b1 commits that are a and b will included on top of it i have b2 commit on top of it merge commit 

but that not the case we need to check "git log --graph --oneline --all --decorate"
-----------------------------------

---------------GIT REBASE--------------------------
git rebase will rewrite the commits without chaging content of file If we perform on shared branch this will lead to unnecessary problem (because of this commit id got changed but had same content it will prevent merge and it will say branch history got divergenteven though content is not changed)

Rebase changes commit identity, not necessarily code content.

Old commit:
Hash: abc123
Code: Add login functionality

New commit:
Hash: xyz789
Code: Add login functionality

Same code change, different commit identity.

This is why rebasing shared branches can cause synchronization problems.

Why?

Because their local commits D and E are not the same commits as D' and E'.

Git doesn't automatically know that D and D' represent the same code changes.

They may need to synchronize using:

git fetch origin
git reset --hard origin/b20

Important
 reset --hard discards local uncommitted changes and local commits not present in the remote branch. They should back up any work they need before running it.

---------------------------------------

Easy way to remember
Rebase A onto B: Checkout A, then rebase onto B.

Merge A into B: Checkout B, then merge A.

GIT BRANCH DIRECTION REFERENCE
================================================================

OPERATION                      SOURCE      DESTINATION   CHECKOUT
----------------------------------------------------------------
Rebase feature onto main       main        feature       feature
Merge main into feature        main        feature       feature
Merge feature into main        feature     main          main
----------------------------------------------------------------

REMEMBER:
- Rebase: Checkout the branch being rebased.
- Merge : Checkout the destination branch.

================================================================

=========================================
GIT CONFLICT
========================================

Here iam taking branches (main and b20) and Iam modified the same file called M3 which has no changes from the 2 branches

in main i have M3 file i will add 9 8 7 as new lines and commit it

in b20 branch in M3 file i will add 1 2 3 as new lines and commit it

now same file in both branch in same lines we have different values 

Now if we try to mege or rebase it will say conflict as below

git branch
  b10
* b20
  main
  test1

git checkout b20
Switched to branch 'b20'

git merge main
Auto-merging m3
CONFLICT (content): Merge conflict in m3
Automatic merge failed; fix conflicts and then commit the result.

cat m3 
<<<<<<< HEAD
1
2
3
=======
9
8
7
>>>>>>> main

after edit the file we need to add and commit the file which had conflicts (for testing even this file can be added but not encouraged to do it)

===============================================================
INTEGRATION MERGE STRATEGIES (--ff, --no-ff(merge), --squash)
===============================================================
1) FAST FORWARD MERGE
===============================================================
What is a "Fast-Forward" Merge?

A fast-forward merge happens when Git joins two branches, but it doesn't actually need to combine any code or create a new merge commit.

It occurs under a very specific condition: The branch you are merging into has not moved forward since you branched off from it.

How to Visualize It
Imagine a race track:

You create a new branch called feature from main. At this point, both point to the exact same commit.

You stay on feature and make 2 new commits (A and B).

Meanwhile, nobody has touched main. It is sitting right where you left it.

You switch back to main and run:

git merge feature

Because main hasn't changed at all since you left it, Git doesn't need to do any complex math or create a "Merge commit." It simply moves the pointer of main forward (fast-forwards it) so that it points directly to commit B alongside feature.

Key Characteristics of a Fast-Forward Merge

No Merge Commit: Git will not create a "Merge branch 'feature' into main" commit.

Linear History: The commit history remains a completely straight line.

Effortless: It happens instantly because there are zero conflicts to resolve.

===================
FAST FORWARD MERGE
====================

In a fast-forward merge, no merge commit is created.

Instead of creating a new commit to tie the two branches together, Git simply slides the branch pointer forward to catch up to the latest commit. It treats the history as if the work was done directly on that main branch all along.

=========================================================
2) NO FAST FORWARD (Standard / Three-Way Merge (--no-ff))
=========================================================

Used when: Both branches have diverged and you want an explicit record that a merge happened.

Result: Creates a distinct merge commit with two parent pointers. Preserves the exact shape of when and where branches split and rejoined.

Sometimes, even if a fast-forward is possible, you might want Git to create a permanent merge commit anyway (to clearly show in history that a feature branch was merged). You can force Git to do that with the --no-ff flag:

git merge --no-ff feature

This forces Git to create a merge commit even if a fast-forward was technically possible!

=======================
3) GIT SQUASH MERGE
=======================

The important difference is that git merge --squash automatically stages the changes, but does not automatically create the commit.

You do NOT need to manually add all the files again.

Normal merge vs squash merge
---
1. Normal merge
---

git checkout main
git merge b20

Git:
Automatically brings the changes.

Automatically creates a merge commit (when needed).

Moves HEAD to the new merge commit.

---
2. Squash merge
---

git checkout main
git merge --squash b20

Git:
Automatically brings all changes from b20.

Automatically stages the combined changes.

Does NOT create a commit.

Does NOT move HEAD.

You just need to run:

git commit -m "Squash merge b20"

Important: Do you need git add?
Normally, NO.

You can verify using:

git status
You should see the changes under:

Changes to be committed:
That means they are already staged.

If you see files under Changes not staged for commit, then you need to stage them.

Remember this simple rule:

git merge = Merge changes + create merge commit automatically.

git merge --squash = Merge changes + stage them; you create the commit manually.

==========================================================
BACKEND MERGE STRATEGIES
==========================================================

When you run git merge, Git uses a specific algorithm under the hood to calculate differences.

---------
ort (Ostensibly Recursive's Twin) — The Default
--------

What it is: Git’s modern default 3-way merge engine. It looks back for a common ancestor commit, compares it to both branches, and figures out how to mesh code or detect file renames safely.

--------
recursive (Legacy)
---------

What it is: The old default engine used in older versions of Git. It has since been replaced by ort (in fact, recursive is now just a synonym for ort).

-------
resolve
-------

What it is: An older 3-way merge algorithm that only works on two branches. It is stricter and looks for "criss-cross" merge ambiguities, but it cannot automatically detect file renames.

-------
octopus
-------

What it is: The default strategy when you try to merge more than two branches at the same time. It is designed to bundle multiple independent topic branches together safely without complex manual conflict resolution.

-------------
ours (Strategy, not option)
------------

What it is: This strategy forces the merge to completely ignore any code changes from the incoming branch. It keeps 100% of your current branch's code and acts as if the other branch was merged, while silently discarding everything the other branch did. (Note: This is different from the -Xours conflict resolution option).

--------
subtree
--------

What it is: A modified version of the ort strategy. It is used when you are merging a separate project repository into a sub-folder of your main repository. It automatically shifts file paths to match the target folder structure.


=========================
GIT STASH
=========================

git stash is a Git command that temporarily saves your uncommitted changes without committing them.


1) git stash -m "stashing in my branch" :- it will create stash in that branch with message, so that we switch it to other branch

2) git stash list :- It will list all branch stash with message

git stash list
stash@{0}: On main: m1
stash@{1}: On b20: s1

remember in stash list new stash will be on top which will point to {0}

Yes! By default, Git always works with stash@{0}, which is the latest stash.

Suppose your stash list is:


bash
git stash list

text
stash@{0}: Changes from Branch B
stash@{1}: Changes from Branch A

+-------------------+------------+--------------------------------------------+
| Command           | Which stash?| What happens?                              |
+-------------------+------------+--------------------------------------------+
| git stash apply   | stash@{0}  | Applies latest stash, keeps it             |
| git stash pop     | stash@{0}  | Applies latest stash, removes it if safe   |
| git stash drop    | stash@{0}  | Deletes latest stash without applying      |
| git stash clear   | All        | Deletes every single stash                 |
+-------------------+------------+--------------------------------------------+

What if you want to work with {1} instead?

You can explicitly specify the stash:


bash
git stash apply stash@{1}

bash
git stash pop stash@{1}

bash
git stash drop stash@{1}
Important concept
Suppose you drop {0}:

Before:


text
stash@{0}: Changes from B
stash@{1}: Changes from A
After:


text
stash@{0}: Changes from A
Git automatically renumbers the remaining stashes.

Remember: {0} always means the latest stash, not a permanent stash ID.

3) git stash drop:- Deletes the latest stash from the list without apply
4) git stash pop:- it will apply latest and delete stash from list
5) git stash apply:- it apply stash and keep it in list
6) git stash clear:- it will clear all stash from list

+-----------------------------+-------------------------------------------------+
| Command                     | Purpose                                         |
+-----------------------------+-------------------------------------------------+
| git stash list              | List all stashes                                |
| git stash show              | Summary of latest stash                         |
| git stash show -p           | Actual code changes in latest stash             |
| git stash show stash@{1}    | Summary of a specific stash                     |
| git stash show -p stash@{1} | Actual code changes in a specific stash         |
+-----------------------------+-------------------------------------------------+

7) git stash branch:- git stash branch is a Git command that creates a new branch from the commit where the stash was originally created and applies the stashed changes to that branch.


==================================
==================================
GIT TAG
=================================
===================================

A Git tag is a named reference (label) attached to a specific commit.

Think of it as putting a bookmark on an important commit so you can easily find it later.

For example, suppose your Git history looks like this:


text
C1 ─── C2 ─── C3 ─── C4 ─── C5
              ↑           ↑
           v1.0.0       v2.0.0

1. Why do we use Git tags?

Tags are commonly used to mark:

Software releases (v1.0, v2.0).

Production deployments.

Stable versions of code.

Important milestones.

For example, when you release your application to production, you might tag that commit as v1.0.0.

Even if you make 100 more commits afterward, the tag still points to the original release commit.

+-----------------------------+-------------------------------------------------+
| Command                     | Purpose                                         |
+-----------------------------+-------------------------------------------------+
| git tag                     | List all existing tags in the repository        |
| git tag -a v1.0 -m "msg"    | Create an annotated tag at your current HEAD    |
| git push origin v1.0        | Push a specific tag up to your remote server    |
| git push origin --tags      | Push ALL local tags up to the remote server     |
| git checkout v1.0           | Switch your workspace to view code at that tag  |
+-----------------------------+-------------------------------------------------+

1) git fetch origin tag <tag-name> :- Fetch a Single Specific Tag
2) git fetch --tags :- fetch all tags
3) git fetch --all :- fetch all branches and tags



===========================================
===========================================
GIT fetch vs GIT PULL
===========================================
===========================================

git fetch --prune origin  :- remove all branches from list which are deleted in remote.

1. What is git fetch?
git fetch downloads new commits, objects, and remote branch information from a remote repository into your local repository.

But it does NOT automatically merge those changes into your local working branch.

There are four important operations:

1) Git contacts the remote repository.

2) Git discovers new commits and objects.

3) Git downloads objects that your local repository doesn't already have.

4) Git updates the corresponding remote-tracking references, such as origin/main.

It does not automatically:

1) Merge changes into your local branch.

2) Change your current branch.

3) Modify your tracked working files.

+------------------------+-------------------------------------------------+
| Command                | Purpose                                         |
+------------------------+-------------------------------------------------+
| git fetch              | Fetch from the configured upstream remote       |
| git fetch origin       | Fetch from origin                               |
| git fetch --all        | Fetch from all configured remotes               |
| git fetch --prune      | Fetch and remove stale remote-tracking refs     |
| git fetch origin main  | Fetch the specified remote branch               |
| git fetch --tags       | Fetch tags from the remote                      |
+------------------------+-------------------------------------------------+

+-----------------------+-------------------------------------------------+-------------------------------------------------+
| Feature               | git fetch                                       | git pull                                        |
+-----------------------+-------------------------------------------------+-------------------------------------------------+
| Direction             | Downstream (Remote -> Local remote-tracking)    | Downstream (Remote -> Local active branch)      |
| What it does          | Downloads new commits/branches without merging  | Downloads new commits AND merges them right away|
| Safety                | Safe (does not touch your current working code) | Can cause merge conflicts if local work clashes |
| Underlying commands   | Just fetches objects and refs                   | Runs `git fetch` followed by `git merge`        |
+-----------------------+-------------------------------------------------+-------------------------------------------------+

==================================
==================================
GIT DEPTH
===================================
==================================
What is depth in Git?
In Git, depth refers to how many generations of commit history you want to download or keep.

Normally, when you clone a repository, Git downloads the entire history from the very first commit ever made. For large projects with years of history, this can mean gigabytes of data and a very slow download.

Depth lets you slice off the old history and only download the most recent commits.

To download the complete remaining history: git fetch --unshallow origin

+------------------------------------+---------------------------------------------------+
| Command                            | Purpose                                           |
+------------------------------------+---------------------------------------------------+
| git clone --depth 1 <url>          | Clone only the latest commit (shallow clone)      |
| git clone --depth 50 <url>         | Clone the latest 50 commits of history            |
| git fetch --depth=1                | Truncate/update local history to just 1 commit    |
| git fetch --unshallow              | Convert a shallow clone into a full repository    |
+------------------------------------+---------------------------------------------------+

====================================
GIT REVERT
====================================

git revert is used to undo the changes introduced by an existing commit without deleting or rewriting Git history.

This is particularly useful when you have already pushed a commit to GitHub and want to undo it safely.

git revert = Undo changes by creating a new commit.

git revert HEAD:- Revert latest commit
git revert abc123 :- Revert a specific commit

Scenario: You added a new file A
Initially:


text
GitHub repository:
├── B
├── C
└── D
You create file A and push it:

git add A
git commit -m "Add file A"
git push origin main
Now GitHub contains:


text
├── A
├── B
├── C
└── D
Now you execute git revert

git revert HEAD
git push origin main
Git creates a new commit that reverses the addition of A.

---
Suppose your history is:

C1 ─── C2 ─── C3
              ↑
          HEAD (main)

Reverting C1 will NOT remove C2 and C3.

if we execute:

git revert <C1-commit-hash>
Git creates a NEW commit C4 that reverses the changes introduced by C1.

Your history becomes:


text
C1 ─── C2 ─── C3 ─── C4
                      ↑
                     HEAD

What happens to the files?

Suppose:

Commit

Action

C1: Add file A

C2: Add file B

C3: Add file C

C4: Revert C1

After reverting C1:


text
Project/
├── B
└── C
File A is removed.

File B remains.

File C remains.

C1, C2 and C3 remain in Git history.

C4 is the new revert commit.

Important exception
If C2 or C3 depends on something introduced by C1, reverting C1 might cause conflicts or leave later functionality broken.

============================
============================
GIT REFLOG
===========================
============================
Git reflog records where your HEAD and branch references have been pointing over time.

That is: It helps you find previous positions of your branch, even after operations like git reset, git commit, git checkout, or git rebase.

It can help recover commits after accidental resets or rebases, provided the commit objects still exist.

Reflog is maintained locally, not shared with GitHub.

Reflog entries expire eventually.

Reflog is not a permanent backup.


1. Why do we need git reflog?
Imagine your Git history looks like this:


text
C1 ─── C2 ─── C3 ─── C4
                      ↑
                    HEAD
You accidentally execute:


bash
git reset --hard HEAD~2
Now your branch looks like this:


text
C1 ─── C2
        ↑
       HEAD
What happened?

C3 is no longer part of your current branch history.

C4 is no longer part of your current branch history.

Your branch now points to C2.

You might think C3 and C4 are permanently lost.

This is where git reflog helps!

2. How to use git reflog?
Execute:


bash
git reflog
Example output:


text
abc123 HEAD@{0}: reset: moving to HEAD~2
def456 HEAD@{1}: commit: Added file D
ghi789 HEAD@{2}: commit: Added file C
jkl012 HEAD@{3}: commit: Added file B
Notice:

HEAD@{0} = Most recent recorded HEAD position.

HEAD@{1} = Previous recorded HEAD position.

HEAD@{2} = Position before that.

You can see that your previous commit positions are still recorded in the reflog.

3. How to recover accidentally lost commits?
Suppose you want to recover C4.

From the reflog, you identify its commit hash:


text
def456 HEAD@{1}: commit: Added file D
You can recover it using:


bash
git reset --hard def456
Your branch is now back at C4.


text
C1 ─── C2 ─── C3 ─── C4
                      ↑
                     HEAD

+--------------------------+-------------------------------------------------+
| Command                  | Purpose                                         |
+--------------------------+-------------------------------------------------+
| git reflog               | Show HEAD's recent history                      |
| git reflog --all         | Show reflogs for all available references       |
| git reflog show main     | Show reflog for main                            |
| git reflog -n 10         | Show the latest 10 entries                      |
| git show HEAD@{2}        | Inspect a previous HEAD position                |
+--------------------------+-------------------------------------------------+



