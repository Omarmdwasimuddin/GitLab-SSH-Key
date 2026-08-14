# GitLab SSH Key Setup
SSH key generate করে GitLab account-এ add করা, তারপর SSH দিয়ে repository clone করা — পুরো process ধাপে ধাপে এখানে দেওয়া আছে।

---

## Step 1: GitLab-এ SSH Key Add করার জায়গা খুঁজে বের করা
GitLab dashboard-এ গিয়ে SSH key add করার option বের করো:

**https://gitlab.com/dashboard** → Click: **Edit profile** → Click: **Access** → Click: **SSH keys** → Click: **Add new key**

![GitLab SSH Keys Page](https://imgur.com/lEa755A.png)

---

## Step 2: PowerShell-এ Folder Create করা
PowerShell open করে এই command গুলো run করো — একটা `gitlab` folder create হবে এবং সেই folder-এ ঢুকবে:

```bash
mkdir gitlab
cd gitlab
```

---

## Step 3: SSH Key Generate করা
এই command দিয়ে SSH key pair generate করো (email তোমার GitLab account-এর email দিয়ে replace করতে পারো):

```bash
ssh-keygen -t ed25519 -C "omar.labib.softwareengineer@gmail.com"
```

Command run করার পর কিছু prompt আসবে, সেগুলো এভাবে handle করতে হবে:

**Prompt 1:**
```
Generating public/private ed25519 key pair.
Enter file in which to save the key (C:\Users\User/.ssh/id_ed25519):
```
এখানে file path দিয়ে দাও:
```bash
C:\Users\User\.ssh\
```

**Prompt 2:**
```
Enter passphrase (empty for no passphrase):
```
Enter press করো (খালি রাখো)।

**Prompt 3:**
```
Enter same passphrase again:
```
আবার Enter press করো।

---

## Step 4: Key কোথায় Save হয়েছে দেখা
উপরের command গুলো ঠিকমতো complete হলে, terminal-এ এই রকম output আসবে — এটা দিয়ে বোঝা যাবে key কোথায় save হয়েছে:

```
Your identification has been saved in C:\Users\User\.ssh\gitlab_ed25519
Your public key has been saved in C:\Users\User\.ssh\gitlab_ed25519.pub
```

![SSH Key Generated Output](https://imgur.com/V7AhFsI.png)

---

## Step 5: SSH Config File Setup

Default filename (`id_ed25519`) ব্যবহার না করে এখানে custom নাম (`gitlab_ed25519`) দেওয়া হয়েছে, তাই SSH client নিজে থেকে এই key খুঁজে পাবে না — clone/push/pull করার সময় "Permission denied (publickey)" error আসতে পারে। এটা এড়াতে, আর ভবিষ্যতে অন্য কোনো account/service-এর জন্য আলাদা key add করলে conflict না হওয়ার জন্য, একটা `config` file বানানো দরকার।

`C:\Users\User\.ssh\config` file create/edit করো (extension ছাড়া, শুধু নাম `config`):

```bash
notepad C:\Users\User\.ssh\config
```

এই content paste করো:

```
Host gitlab.com
    HostName gitlab.com
    User git
    IdentityFile C:/Users/User/.ssh/gitlab_ed25519
    IdentitiesOnly yes
```

**কী কী করে এই config:**
- `Host` / `HostName` → `gitlab.com`-এ connect করার সময় এই rule apply হবে
- `IdentityFile` → এই key (`gitlab_ed25519`) ব্যবহার হবে, default key খোঁজার চেষ্টা করবে না
- `IdentitiesOnly yes` → শুধু এই specified key try করবে, `.ssh` folder-এ অন্য key (যেমন GitHub-এর জন্য আলাদা key) থাকলেও সেটা try করে confuse হবে না

> 💡 পরে যদি GitHub বা অন্য কোনো Git service-এর জন্যও আলাদা SSH key বানাও, তাহলে এই একই file-এ নতুন `Host` block যোগ করে দিলেই হবে — একটা file দিয়ে সব key manage করা যাবে।

---

## Step 6: Public Key Copy করে GitLab-এ Paste করা
1. `gitlab_ed25519.pub` file-টা Notepad দিয়ে open করো
2. Full content copy করো (এটা হচ্ছে তোমার **public key**, private key না)
3. GitLab-এর "SSH Key" input box-এ paste করো
4. Click করো: **Add key**

![Add SSH Key to GitLab](https://imgur.com/FnvwFHI.png)

> ⚠️ **মনে রাখবে:** শুধু `.pub` extension-ওয়ালা file-এর content paste করবে। Private key (`gitlab_ed25519`, extension ছাড়া file) কখনো কাউকে share করবে না।

---

## Step 7: Test করা — Repository Clone করা
এখন test করে দেখা যাবে SSH key ঠিকমতো কাজ করছে কিনা।

1. Visit করো: **https://gitlab.com/wasuit-group/my-first-pipeline**
2. Click: **Code** → **Clone with SSH** → copy করো clone URL-টা
3. PowerShell-এ project folder-এ গিয়ে (যেমন: `PS C:\Users\User\Desktop\gitlab>`) এই command run করো:

```bash
git clone git@gitlab.com:wasuit-group/my-first-pipeline.git
```

✅ Command successful হলে, project SSH দিয়ে clone হয়ে যাবে — মানে SSH key setup ঠিকমতো কাজ করছে।

---

## Quick Summary

| Step | কাজ |
|------|-----|
| 1 | GitLab-এ SSH Keys page-এ যাওয়া |
| 2 | Local-এ `gitlab` folder create করা |
| 3 | `ssh-keygen` দিয়ে key pair generate করা |
| 4 | Key file path check করা |
| 5 | SSH config file বানিয়ে custom key filename এর জন্য host mapping set করা |
| 6 | Public key (`.pub`) GitLab-এ paste করে add করা |
| 7 | SSH দিয়ে repo clone করে test করা |
