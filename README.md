## GitLab SSH Key Setup

#### https://gitlab.com/dashboard --->Click: Edit profile --->Click: Access --->Click: SSH keys --->Click: Add new key
![](https://imgur.com/lEa755A.png)

#### powershell e command koro
```bash
mkdir gitlab
cd gitlab
```
```bash
ssh-keygen -t ed25519 -C "omar.labib.softwareengineer@gmail.com"
```
#### Generating public/private ed25519 key pair.
#### Enter file in which to save the key (C:\Users\User/.ssh/id_ed25519):
```bash
C:\Users\User\.ssh\
```
#### Enter passphrase (empty for no passphrase): Enter press koro.
#### Enter same passphrase again: Enter press koro.
---

#### kothay ssh key saved hoiche tar path dekhabe nicher moto.
#### Your identification has been saved in C:\Users\User\.ssh\gitlab_ed25519
#### Your public key has been saved in C:\Users\User\.ssh\gitlab_ed25519.pub
![](https://imgur.com/V7AhFsI)
#### note pad e open kore copy kore SSH Key te paste koro. (public key paste korte hobe)---> click koro: Add key
![](https://imgur.com/FnvwFHI.png)

---

#### test kore dekhte visit koro: https://gitlab.com/wasuit-group/my-first-pipeline --->click: Code --->Clone with SSH: copy koro.
#### powershell open koro
#### PS C:\Users\User\Desktop\gitlab> folder e command koro.
```bash
git clone git@gitlab.com:wasuit-group/my-first-pipeline.git
```

#### finalyy project clone hoye jabe ssh diye
