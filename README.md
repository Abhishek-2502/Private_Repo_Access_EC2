# Granting EC2 Access to a Private GitHub Repository

This guide explains how to allow an AWS EC2 instance to access a private GitHub repository using SSH deploy keys.

---

## Step 1: Generate an SSH Key on EC2

Refer to **Step 2** of the guide: [Node_Todo_App_Docker_Jenkins_FreeStyle](https://github.com/Abhishek-2502/Node_Todo_App_Docker_Jenkins_FreeStyle)

## Step 2: Add the Public Key to GitHub Deploy Keys

1. Go to **GitHub** and open the **private repository**.
2. Navigate to **Settings → Deploy keys**.
3. Click **Add deploy key**.
4. Paste the copied public key into the field.
5. Check **Allow write access** (only if needed).
6. Click **Add key**.

---

## Step 3: Ensure Correct Permissions on EC2

Check ownership of the key file:
```bash
ls -l ~/.ssh/github-deploy
```

If the owner is `root`, change it to the current user:
```bash
sudo chown ubuntu:ubuntu ~/.ssh/github-deploy
```

Set correct file permissions:
```bash
chmod 600 ~/.ssh/github-deploy
```

---

## Step 4: Add the Key to SSH Agent & Clone Repository

1. Start the SSH agent:
   ```bash
   eval "$(ssh-agent -s)"
   ```

2. Add the SSH key:
   ```bash
   ssh-add ~/.ssh/github-deploy
   ```

3. Verify the connection to GitHub:
   ```bash
   ssh -T git@github.com
   ```
   Expected output:
   ```
   Hi username! You've successfully authenticated, but GitHub does not provide shell access.
   ```

4. Clone the private repository:
   ```bash
   git clone git@github.com:Abhishek-2502/EyeSeeU.git
   ```

---

## Troubleshooting

- **Permission Denied (publickey)**: Ensure the deploy key is added to GitHub.
- **Key not being used**: Run `ssh-add -l` to confirm the key is loaded.
- **SSH config issue**: Create a `~/.ssh/config` file with:
  ```
  Host github.com
      IdentityFile ~/.ssh/github-deploy
      User git
  ```
  Then restart the SSH agent and try again.

---

Now your EC2 instance can securely access the private GitHub repository! 🚀

## Author 

- Abhishek Rajput

