# 🔐 SSH Key Authentication Setup (Ubuntu to Ubuntu)

This guide sets up passwordless SSH login from one Ubuntu server (`ansible-server`) to another (`target-server`) using `ed25519` key pairs.

---

## 📍 1. Generate SSH Key on Source Server (Ansible Server)

On the **ansible-server**, generate an SSH key pair:

```bash
ssh-keygen -t ed25519 -C "ansible-server"
```

* When prompted for file path, press `Enter` to accept default: `/home/ubuntu/.ssh/id_ed25519`
* Choose a **passphrase** or leave it empty for no passphrase

```plaintext
Your identification has been saved in /home/ubuntu/.ssh/id_ed25519
Your public key has been saved in /home/ubuntu/.ssh/id_ed25519.pub
```

---

## 📍 2. Copy Public Key to Target Server

Use `ssh-copy-id` or manually append the public key to the target server:

### Option A: Using `ssh-copy-id` (if available)

```bash
ssh-copy-id -i ~/.ssh/id_ed25519.pub ubuntu@<target-server-ip>
```

### Option B: Manual Copy

From `ansible-server`, run:

```bash
cat ~/.ssh/id_ed25519.pub
```

Copy the key output and paste it into the `~/.ssh/authorized_keys` file of the `ubuntu` user on the **target-server**:

```bash
ssh ubuntu@<target-server-ip>
vim ~/.ssh/authorized_keys
# Paste the public key at the end
```

---

## 📍 3. Set Correct Permissions on Target Server

Ensure the `.ssh` directory and `authorized_keys` have correct permissions:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

---

## 📍 4. Test SSH Login

From **ansible-server**, SSH into **target-server**:

```bash
ssh ubuntu@<target-server-ip>
```

You should now connect without entering a password.

---

## 🧹 Notes

* You can create multiple key pairs for different purposes or rotate old keys.
* Backup your private key (`id_ed25519`) securely.
* Avoid overwriting existing keys unless necessary.

---

✅ **Successfully enabled key-based authentication between Ubuntu servers!**
