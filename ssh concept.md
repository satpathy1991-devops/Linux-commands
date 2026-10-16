# SSH Key Recovery in Linux

## 1. What is SSH?

SSH (Secure Shell) is a protocol used to securely connect to remote Linux servers.

For example, we use MobaXterm to connect to an AWS EC2 Ubuntu instance using an SSH private key.

## 2. Understanding SSH Key Files

An SSH key pair contains two keys:

| File | Purpose |
|---|---|
| `id_ed25519` | Private key — kept secret by the user |
| `id_ed25519.pub` | Public key — can be shared with the administrator |
| `authorized_keys` | Contains public keys permitted to log in to a Linux user account |
| `known_hosts` | Stores host identity information for servers previously contacted |

Example directory:

```text
/home/achyut/.ssh/
├── authorized_keys
├── id_ed25519
├── id_ed25519.pub
└── known_hosts
```

Note: The files shown are examples. A user's `.ssh` directory does not necessarily contain all of them.

## 3. What Happens if the `.ssh` Directory Is Deleted?

Suppose the SSH directory for the Linux user `achyut` is deleted:

```bash
rm -rf /home/achyut/.ssh/
```

Possible consequences:

- The user's `authorized_keys` file is deleted.
- SSH public-key login may fail because the server no longer has the authorized public key.
- The user's private key on their personal computer is not deleted.
- Other users' SSH directories are not affected.

Deleting the directory does not automatically stop the EC2 instance or its running applications.

**Important:** Never delete an SSH directory on a production server without confirming the recovery method.

## 4. Scenario: The Private Key Is Still Available

Suppose Achyut has the original private key on their computer, but the server's `.ssh` directory has been deleted.

The original private key can still be used. There is no need to create a new key pair just because the server's public key record was deleted.

### Step 1: Derive the public key from the existing private key

On Achyut's computer, run:

```bash
ssh-keygen -y -f id_ed25519 > id_ed25519.pub
```

Explanation:

- `ssh-keygen` — manages SSH keys.
- `-y` — derives the public key from the private key.
- `-f id_ed25519` — specifies the private key file.
- `>` — redirects the output into a file.
- `id_ed25519.pub` — the resulting public key.

This command does not change the original private key.

### Step 2: Re-create the SSH directory on the server

An administrator with appropriate privileges can run:

```bash
mkdir -p /home/achyut/.ssh
chown achyut:achyut /home/achyut/.ssh
chmod 700 /home/achyut/.ssh
```

Explanation:

- `mkdir -p` — creates the directory if it does not exist.
- `chown` — sets ownership to the `achyut` user and group.
- `chmod 700` — allows only the owner to access the directory.

### Step 3: Restore the public key

Create the authorization file:

```bash
touch /home/achyut/.ssh/authorized_keys
```

Set its ownership and permissions:

```bash
chown achyut:achyut /home/achyut/.ssh/authorized_keys
chmod 600 /home/achyut/.ssh/authorized_keys
```

Append the public key to `authorized_keys` using a secure transfer method:

```bash
cat id_ed25519.pub >> /home/achyut/.ssh/authorized_keys
```

Run the `cat` command from a location where the public key file is available, or transfer the public key to the server first.

The public key must be installed in the correct user's `authorized_keys` file.

### Step 4: Connect using MobaXterm

Achyut selects the original private key in MobaXterm and connects to the server.

If the public key matches, SSH configuration and permissions are correct, and network access is permitted, login should succeed.

## 5. What If the Private Key Is Lost?

If Achyut's original private key is also lost, the old key pair cannot be recovered from the public key alone.

The recovery process is:

1. Generate a new SSH key pair on Achyut's computer.
2. Keep the new private key secure.
3. Provide the new public key to the administrator.
4. Add the public key to `/home/achyut/.ssh/authorized_keys`.
5. Configure MobaXterm to use the new private key.

The administrator must have an alternative way to access the server to restore the public key.

## 6. What If the Private Key Is Compromised?

If a private key may have been exposed, do not continue using it.

1. Generate a replacement key pair.
2. Authorize the new public key.
3. Test access using the new private key.
4. Remove the compromised public key from `authorized_keys` and any other places where it is trusted.

## 7. Root User vs Normal User

Each Linux user has a separate home directory and SSH configuration.

| Linux user | Home directory | SSH directory |
|---|---|---|
| `root` | `/root` | `/root/.ssh/` |
| `ubuntu` | `/home/ubuntu` | `/home/ubuntu/.ssh/` |
| `achyut` | `/home/achyut` | `/home/achyut/.ssh/` |

Deleting `/root/.ssh/` does not automatically delete `/home/achyut/.ssh/`.

Logging in as `ubuntu` and running `sudo su -` switches to root using sudo privileges. This does not require `/root/.ssh/` to exist.

## 8. Important Security Rules

- Never share your private SSH key with other users.
- Each user should ideally have their own key pair.
- Share public keys when granting SSH access.
- Keep private keys secure and restrict their file permissions.
- Back up important private keys securely.
- Test access before removing an old key.
- Never remove the only working access method without preparing a recovery plan.

## 9. Quick Revision

| Situation | Action |
|---|---|
| `.ssh` directory deleted, original private key available | Derive or restore the matching public key and authorize it on the server |
| Private key lost | Generate a new key pair and authorize the new public key |
| Private key compromised | Replace the key and revoke trust in the compromised public key |
| Root's `.ssh` deleted | Root's SSH configuration may be affected; other users' SSH files remain separate |
| Public key deleted but private key available | Derive the public key again using `ssh-keygen -y` |

### Interview Question

**Q: If a Linux user's `.ssh` directory is accidentally deleted, does the user always need to generate a new SSH key pair?**

**Answer:** No. If the user still has the original private key, the corresponding public key can be derived using `ssh-keygen -y` and restored to the user's `authorized_keys` file. The user can continue using the original private key once server access is correctly restored.
