# SVN to GIT notes

### terminology
1. client/local -> your computer, machine you are working on
2. server -> where our code is hosted, centralized point of reference
3. working copy -> your local code
4. repository -> where all records including branches, tags, logs etc are defined

### repository
- svn
  - central repository, local working copy
  - changes committed -> central server -> immediately available for everyone
  - no concept of local commits
  - svn commit -> commits changes to server
  - branches/tags are literal copies created of it's source
  - svn update -> takes your local copy to latest revision of central server (unless revision id is specified)
  - svn log -> shows you logs from server* . logs are stored as svn information on server
    - (though clients like tortoies svn can store them in cache which can be retrieved if ever central repo get's lost)
  - every folder has .svn sub folder
- git
  - distributed vcs
  - there is a local repository that has all your logs, branches, tags etc
  - local repository is basically a copy of remote/server repo
  - you can perform actions like commit, branching, tagging, merging, reverting on that local repository
  - this local repository is basically stored in .git folder in root directory

### some comparisons and differences
- if your central repo is somehow get's wiped
  - svn: you are only left with your local files
    - your revision info's, commit history, branches and tags that are not checked out are lost forever
  - git: you can just push your local copy because git keeps entire repository on every clone.
    - you get your logs, commit history everything.
    - only data lost is the commits made to central server after your last update.
    - but in that case, whoever pushed last commit to server will have all the contents so nothing to worry about in terms of git data
    - (NOTE: github/bitbucket is git hosting platform and forks, issues, pull requests are feature features provided by those platform and not by git.
    - so that data will not be recoverable from local repository)
- in svn, when you create branch or tag, your entire source is copied
- in git, your commits are deltas between previous commit and new
  - and commits themselves refer to other commit(s) and your tags and branches just point to some commits,
  - so no new copies of data are created, saving that disk space
- in svn, when you commit, your changes are directly reflected on central repo/server.
- in git, you commit your changes to local repository and then push them to central server whenever needed.
- this gives us freedom of developing feature locally with changes split in commits without breaking code for other people.
- ![refer image for difference diagram](https://www.git-tower.com/learn/media/pages/git/ebook/en/command-line/appendix/from-subversion-to-git/d8bce57f3a-1697824976/centralized-vs-distributed.png)

### GIT commands
- ```git init``` -> initializes an empty repository in current folder
  - one time command for new repos
  - devs will use rarely/never.
  - we create new repo on hosting platform and clone/checkout it

- ```git add <file,folder,path,pattern>```
  - tells vcs to to track the files provided as argument
  - similar but not same as ```svn commit```
  - ```svn add``` adds file for tracking permanantly
  - ```git add``` adds current state/snapshot of file to staging (a pre commit stage if you may)
  - if you make change in added file, you need to add that file again for that change to reflect

- ```git status```
  - similar to tortoise SVN ```show modifications``` command
  - lists out the files/changes that are in staging and in untracked stage
  - untracked means you did some change locally but did not ```git add``` it to staging
  - when you commit in git, only files in staging area get's commited
  - in contrast to svn, where if you once do git add on a file, changes in that file will always get commited
    - unless you specify files (or uncheck files in tortoise svn)

- ```git diff```
  - shows the diff like svn diff (or svn patchfile that we create)
  - can be used to check diff between two commits or from any commit to head
  - can be used to check diff between HEAD and staged

- ```git commit```
  - commits files to local repository on (current branch of working copy)
  - changes are not avilable on remote/server repository unless we perform ```git push```

- git remote
  - git remote is an alias assigned to the remote server URL
  - by default, when you clone/checkout git repo, that URL is added as default remote named origin
  - if you create local repo and want to push it to remote, you add a remote and set it default upstream remote
  - ```git remote add origin <url>``` will create new remote named origin that points to URL
  - ```git remote set-url origin <url>``` will edit existing remote with new URL

- ```git push```
  - git push pushes your local commits to remote (bitbucket/github/etc)
  - for new local repos, after creating new remote, we need to run bellow command
    - ```git push -u origin main``` where origin is remote and main is branch
  - by default, git push pushes to origin remote with current active branchname

## difference points, again
- svn commit instantly reflected on central repo
- git commits needs to be pushed on central repo
- svn commit assigns it the latest revision integer + 1 integer
- git commit is hash of that comit
- hash value is unique and although long, you can specify first 3 to 4 alpha-numerals to specify the revision cause at most times, they are unique too

## git branching
we will check sandboxing [here](https://learngitbranching.js.org)
