Use or create an ssh key.
```bash
ssh-keygen -f ~/.ssh/this-key-must-exist
```
Optionally, undo previous signing setting.
```bash
git config --global --unset gpg.format
```
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
Make or replace that file.
```bash
echo "$(git config --get user.email) namespaces=\"git\" $(cat ~/.ssh/this-key-must-exist.pub)" > ~/.ssh/allowed_signers
```
Test signing with a dry run (in a real git repo).
```bash
git commit --allow-empty -S -m "test signing"
```
Check it worked.
```bash
git log --show-signature
```
Undo and destroy the test commit while leavinng any staged changes.
```bash
git reset --soft HEAD~1
```