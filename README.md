# Sandbox

A repository to hold examples and bug-reproducers for the Mu2e/Offline repo.

This repo was originally a subdirectory in Offline.  It was created from
Offline by the following.

1. Install git filter-repo
   https://docs.github.com/en/get-started/using-git/splitting-a-subfolder-out-into-a-new-repository
   I did this a Mac with git managed by homebrew.  So the command was:
   ```brew install git-filter-repo```
2. On Mu2e GitHub site, create a new repo named Sandbox
    - do not import anything
    - do not make README, License, .gitignore etc
3. At a terminal window

```
git clone --bare https://github.com/Mu2e/Offline
git filter-repo --path Sandbox
git tag | awk '{print "git tag -d " $0}' |sh
git push --import git@github.com/Mu2e/Sandbox
```

4. In another terminal window:
```git clone https://github.com/Mu2e/Offline```
  - remove Sandbox and make a PR
```git clone https://github.com/Mu2e/Sandbox```
  - ack -l Offline/Sandbox | xargs perl -wapi\~ -e 's&Offline/Sandbox&Sandbox/Sandbox&g'
  - sed -i  's&mu2e_Sandbox&Sandbox_Sandbox&' Sandbox/src/SConscript
  - touch .muse
  - cp from Offline, Li

