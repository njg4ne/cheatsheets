
## Setup
Make a scratch repo for testing.
```bash
mkdir -p ~/test-repo-for-ssh-git-signing
cd ~/test-repo-for-ssh-git-signing
echo test > test.txt
git init .
git branch -M main
git add .
git commit -m "first unsigned commit"
git log
```
Use or create an ssh key.
```bash
ssh-keygen -f ~/.ssh/this-key-must-exist
```
Copy from here the **public** key to your GitHub account **twice** (once for auth & once for signing).
```bash
cat ~/.ssh/this-key-must-exist.pub
```
Optionally, undo previous signing settings.
```bash
git config --global --unset gpg.format
```
## Configure Git
Tell Git to use ssh for signing.
```bash
git config --global gpg.format ssh
```
Point git at your **specific** key.
```bash
git config --global user.signingkey ~/.ssh/this-key-must-exist
```
Tell git to always sign commits.
```bash
git config --global commit.gpgsign true
```
Set the global email and name
```bash
git config --global user.email "specific-email@example.com"
git config --global user.name "A Name"
```
Point git to an allowedsigners ssh file. 
```bash
git config --global gpg.ssh.allowedSignersFile ~/.ssh/allowed_signers
```
## Configure SSH
Make or replace that file.
```bash
echo "$(git config --get user.email) namespaces=\"git\" $(cat ~/.ssh/this-key-must-exist.pub)" > ~/.ssh/allowed_signers
```
## Testing
Test signing with a dry run (in a real git repo).
```bash
git commit --allow-empty -S -m "test signing"
```
Check it worked.
```bash
git log --show-signature
```
Undo and destroy the test commit while leaving any uncommitted changes.
```bash
git reset --soft HEAD~1
```
Optionally repeat [Testing](#Testing) without -S (force signing) to make sure git automatically does so.
Now cleanup the test repo.
```bash
cd ~
rm -rf ~/test-repo-for-ssh-git-signing
```