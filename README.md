# Managing Multiple GitHub Accounts in VSCode

This guide explains how to manage multiple GitHub accounts (personal and company) in Visual Studio Code (VSCode) using SSH. This method allows you to switch between accounts easily without the risk of pushing to the wrong repository.

## Table of Contents
1. [Understanding SSH](#understanding-ssh)
2. [Setup SSH Directory](#setup-ssh-directory)
3. [Generate SSH Keys](#generate-ssh-keys)
4. [Add SSH Keys to GitHub](#add-ssh-keys-to-github)
5. [Start the SSH Agent](#start-the-ssh-agent)
6. [Create SSH Config File](#create-ssh-config-file)
7. [Setup Git Configuration Files](#setup-git-configuration-files)
8. [Conclusion](#conclusion)

## Understanding SSH

SSH (Secure Shell) is a cryptographic network protocol used for secure data communication. By generating SSH keys for your GitHub accounts, you can securely authenticate without needing to enter your username and password every time you push or pull from a repository.

## Setup SSH Directory

1. Open a terminal (command prompt, PowerShell, or bash).
2. Navigate to the SSH directory:
   ```bash
   cd ~/.ssh
   ```

## Generate SSH Keys

Generate separate SSH keys for your personal and company accounts using the following command:

1. For the company account:
   ```bash
   ssh-keygen -t rsa -b 4096 -C "your_company_email@example.com" -f ~/.ssh/company_github_username
   ```

2. For the personal account:
   ```bash
   ssh-keygen -t rsa -b 4096 -C "your_personal_email@example.com" -f ~/.ssh/personal_github_username
   ```

**Note:** Replace `your_company_email@example.com` and `your_personal_email@example.com` with your actual email addresses. Follow the prompts to complete the generation process.

## Add SSH Keys to GitHub

1. Open the generated SSH key files (e.g., `~/.ssh/company_github_username.pub` and `~/.ssh/personal_github_username.pub`) in a text editor.
2. Copy the contents of each file.
3. Go to your GitHub account settings:
   - For the company account: **Settings** -> **SSH and GPG keys** -> **New SSH key**.
   - For the personal account: **Settings** -> **SSH and GPG keys** -> **New SSH key**.
4. Paste the copied key and give it a descriptive title. Click **Add SSH key**.

## Start the SSH Agent

Start the SSH agent to manage your keys:

```bash
eval "$(ssh-agent -s)"
```

## Create SSH Config File

1. Open or create the SSH config file:
   ```bash
   nano ~/.ssh/config
   ```
2. Add the following configurations for your keys:
   ```plaintext
   Host github-company
       HostName github.com
       User git
       IdentityFile ~/.ssh/company_github_username

   Host github-personal
       HostName github.com
       User git
       IdentityFile ~/.ssh/personal_github_username
   ```

## Setup Git Configuration Files

Navigate to your home directory and create the following configuration files:

```bash
cd ~
touch .gitconfig
touch .gitconfig.company
touch .gitconfig.personal
```

### Inside `~/.gitconfig.company`

Add the following content:

```plaintext
[user]
    email = your_company_email@example.com
    name = Your Name

[github]
    user = "company_github_username" # should be inside double quotes.

[core]
    sshCommand = "ssh -i ~/.ssh/company_github_username" # should be inside double quotes.
```

### Inside `~/.gitconfig.personal`

Add the following content (update as needed):

```plaintext
[user]
    email = your_personal_email@example.com
    name = Your Name

[github]
    user = "personal_github_username" # should be inside double quotes.

[core]
    sshCommand = "ssh -i ~/.ssh/personal_github_username" # should be inside double quotes.
```

### Inside `~/.gitconfig`

Add the following content:

```plaintext
[includeIf "gitdir:C:/Users/<your_username>/Personal/"] # Include for all .git projects under Personal/
    path = ~/.gitconfig.personal

[includeIf "gitdir:C:/Users/<your_username>/Company/"] # Include for all .git projects under Company/
    path = ~/.gitconfig.company  

[core]
    excludesfile = ~/.gitignore      # Ignore .gitconfig files valid everywhere
```

Replace `<your_username>` with your actual Windows username.

## Conclusion

You can now access your company and personal GitHub accounts without worrying about accidental pushes. By following these steps, you will have a secure and efficient way to manage multiple GitHub accounts in VSCode.

If you have any questions or run into issues, feel free to ask for help!
