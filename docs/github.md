# Create a SSH key from WLS

## 1. Check for Existing Keys
Before creating a new key, check if one already exists in your local WSL environment.
```bash
ls -al ~/.ssh

```
*If this returns "No such file or directory," you do not have an SSH key set up yet.*

## 2. Generate a New SSH Key

Generate a new ED25519 SSH key, replacing the email with your GitHub account email.

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"

```

*Press **Enter** to accept the default file location and press Enter twice for no passphrase (optional).*

## 3. Add the Key to the SSH Agent

Start the SSH agent in the background and add your newly created private key.

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```
## 4. Add the Public Key to GitHub

Output your public key to the terminal so you can copy it:

```bash
cat ~/.ssh/id_ed25519.pub

```

1. Log in to GitHub and go to **Settings** > **SSH and GPG keys**.
2. Click **New SSH key**, give it a title (e.g., "WSL Local Machine"), and select **Authentication Key**.
3. Paste the copied output and save.

## 5. Test the Connection

Verify that your WSL environment can successfully authenticate with GitHub.

```bash
ssh -T git@github.com

```

* If prompted that the authenticity of the host can't be established, type `yes` and press **Enter**.
* A successful connection will return: `Hi <username>! You've successfully authenticated, but GitHub does not provide shell access.`


## 6. Troubleshooting: Git Asks for a Password

If Git asks for a username and password when you try to push (e.g., `git push -u origin main`), it means your local repository was cloned using the **HTTPS** URL instead of the **SSH** URL.

To fix this, change the remote URL inside your project folder:

```bash
# Ensure you are in your project directory
cd your-project-folder

# Update the remote URL to use SSH
git remote set-url origin git@github.com:<your_username>/<your_repository_name>.git

# Verify the change
git remote -v

# Push your code securely
git push -u origin main

```
