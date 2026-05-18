# Collaborative Git Workflows: SSH Key Generation for GitHub

An SSH key pair consists of a private key and a public key. The private key
should be kept secure in your local machine, while the public key will be
added to your GitHub account to allow for secure authentication.

Summary of key points:

* SSH replaces passwords
* GitHub trusts your machine, not your authentication session
* You can have multiple SSH keys for different purposes (e.g.,
one for authentication and another for signing commits). In this setup, we
use one SSH key pair for both authentication and signing commits to keep
it simple.
* You can have multiple SSH key pairs for different machines, e.g., one
for your personal laptop and another for your work laptop. This way, if
one of your machines gets compromised, you can easily revoke the
corresponding SSH key from your GitHub account without affecting the other
machine that has not been compromised.

***Note:** Ensure you are using the terminal for all Git operations in this
lab, not a graphical Git client like the built-in Visual Studio Code Git
support or GitHub Desktop.*

## Install OpenSSH Client (if not already installed)

### Check if already installed

`ssh -V`

If `ssh -V` returns a version number, you already have OpenSSH installed. If it returns an error, you will need to install it.

<img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/windows11/windows11-original.svg" width="40" />

Installing OpenSSH in Windows:

### Install the client (not the server)

```bash
Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0
```

<img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/linux/linux-original.svg" width="40" /> <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/apple/apple-original.svg" width="40"/>

Installing OpenSSH Client in Linux/Mac is usually unnecessary as it is typically pre-installed. However, if you need to install it, you can use the following commands:

* Execute:

```bash
sudo apt update
sudo apt install openssh-client
```

Verify installation:

```bash
ssh -V
```

## Step 1: Open Terminal (Git Bash on Windows or Default Terminal on Linux/Mac)

Execute the following command to generate a new SSH key pair.

```bash
ssh-keygen -t ed25519 -C "<place your comment here>" -f ~/.ssh/id_ed25519_auth_and_sign
````

* `-t ed25519` specifies the type of key to create, which is  [https://en.wikipedia.org/wiki/EdDSA](https://en.wikipedia.org/wiki/EdDSA). This is a modern and secure choice for SSH keys.

* The comment can contain the name of the machine. That will help you identify
which key was used and where it was used from, e.g., a key for your personal
laptop and another key for your work laptop in future.

* `-f` specifies the file name of the private key as well as the folder where it
will be saved. The public key will be saved with the same name but with a
`.pub` extension. For example, in this case, the private key will be saved as `~/.ssh/id_ed25519_auth_and_sign` and the public key will be saved as `~/.ssh/id_ed25519_auth_and_sign.pub`.

* `~` represents the home directory of the current user. On Windows, this translates to `C:\Users\YourUsername`.

After running the command, you will be prompted to enter a passphrase. You can
leave it empty for no passphrase or enter a secure passphrase for added
security. If you enter a passphrase, you will need to remember it to use the
key. In this case, we leave it empty for simplicity, however,
in a professional setting, it is recommended to use a passphrase for added
security.

## Step 2: Add the PUBLIC (.pub) Key to Your GitHub Account

Log in to your GitHub account, navigate to "Settings" > "SSH and GPG keys" >
"New SSH key". Paste **all the contents** of your public key file
(`~/.ssh/id_ed25519_auth_and_sign.pub`) into the "Key" field and select
"Authentication Key" as the type.

Make sure that you are copy-pasting your public key, and **NOT** the private key.
Your private key (`~/.ssh/id_ed25519_auth_and_sign`) should never be shared with anyone.
The public key is safe to share and is used to authenticate your identity when
connecting to GitHub.

Give it a descriptive title, e.g., `Git authentication for GitHub from
<your_laptop_name>`. That way, if your laptop gets compromised, you can
easily identify which private key was being used for authentication and
revoke it from your GitHub account.

Click "Add SSH key" to save it.

Register the same public key again, but classify it as a signing key. This
 way, you can use the same SSH key pair for both authentication and
 signing commits. You can also choose to use different SSH key pairs for
 authentication and signing if you prefer, but using the same key pair
 simplifies the setup.

## Step 3: Add the SSH Key to the Local SSH Agent

First confirm that the SSH agent is running:

```bash
eval "$(ssh-agent -s)"
```

On Windows, if this command fails, ensure that the OpenSSH Authentication Agent service is running via Services.

```bash
git config --global gpg.format ssh
git config --global user.signingkey ~/.ssh/id_ed25519_auth_and_sign.pub
git config --global commit.gpgsign true
```

Then add the private key to the SSH agent:

```bash
ssh-add ~/.ssh/id_ed25519_auth_and_sign
```

## Step 4: Test the SSH Connection to GitHub

To verify that your SSH key is correctly set up and can authenticate with GitHub, run the following command:

```bash
ssh -T git@github.com
```

You should see a message like this:

```text
Hi <your_github_username>! You've successfully authenticated, but GitHub does not provide shell access.
```

This indicates that your SSH key is correctly configured and you can now use it for Git operations with GitHub.

---

However, if you see a message like this:

```text
Permission denied (publickey).
```

Or any other error message, you can troubleshoot the issue by running:

```bash
ssh -vT git@github.com
```

---

You should now be using SSH for both Git authentication when cloning repositories and for Git signing when committing changes.

![Use SSH not HTTPS](./assets/images/UseSSH_notHTTPS.png)

The cloning command in this case would then be as follows to clone the repository using SSH and store the code in a folder named `Lab-1-Git`:

```bash
git clone git@github.com:course-files/Git.git Lab-1-Git
```

The syntax is:

```bash
git@github.com:<username>/<repository>.git <name-of-repository-in-your-local-machine>
```

This setup enhances the security of your interactions with GitHub while also providing a convenient way to manage your Git operations.
