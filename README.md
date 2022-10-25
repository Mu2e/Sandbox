# Sandbox: examples and bug-reproducers for the Mu2e/Offline repo.

This repo was originally a subdirectory in Offline.  It was created from Offline by the following.

1. This work was informed by the instructions from:
  - https://docs.github.com/en/get-started/using-git/splitting-a-subfolder-out-into-a-new-repository/
  - https://docs.github.com/en/get-started/importing-your-projects-to-github/importing-source-code-to-github/importing-a-git-repository-using-the-command-line
3. In the Mu2e GitHub site, create a new repo named Sandbox
    - do not import anything
    - do not make README, License, .gitignore etc
4. In a clean working directory in a clean terminal window:
```
git clone --bare https://github.com/Mu2e/Offline         # Does not check out a working tree
git filter-repo --path Sandbox                           # Remove everything except Sandbox; see step 5.
git tag | awk '{print "git tag -d " $0}' |sh             # Remove all tags
git push --import git@github.com/Mu2e/Sandbox            # The current branch is main so that's what's pushed.
cd ..
rm -rf Sandbox
```
4. In another clean working directory in a clean terminal window:
  - ```git clone https://github.com/Mu2e/Offline```
  - Start a new branch, remove Sandbox, commit, push to my own fork and make a PR
  - ```git clone https://github.com/Mu2e/Sandbox```
  - Rename the Sandbox directory in anticipation of future peer directories; fix include directives and link lists:
  ```
  cd Sandbox
  git checkout -b firstRealVersion
  git mv Sandbox FromOffline
  ack -l Offline/Sandbox | xargs perl -wapi\~ -e 's&Offline/Sandbox&Sandbox/FromOffline&g'
  sed -i  's&mu2e_Sandbox&sandboxFromOffline&' Sandbox/src/SConscript                             # See step 6
  touch .muse
  cp ../Offline/LICENSE .
  # add README.md and .gitignore
  git commit -m "comment." -a
  ```
  - Push to my own fork and make a PR
5. filter-repo is not included in the git distribution on mu2egpvm*. I did this step on my laptop.  I use homebrew to manage git so I installed this with
   ```
   brew install git-filter-repo
   ```
6. There was an unexpected detail when changing the names of the libraries in the link lists in the SConscript file: muse decided to name
   the libraries that it created as ```libsandboxFromOffline_*```; I had expected ```libsandbox_FromOffline_*```; note the missing underscore.
 
