# Using VS Code with Remote HPC Systems (Rocket/Comet) via SSHFS

This guide explains how to use VS Code locally while working with files on remote HPC systems like Rocket or Comet using SSHFS mounting.

## Problem

VS Code and other IDEs are often blocked or restricted on HPC login nodes because they:
- Consume excessive resources on the login node
- Extensions can use significant storage space
- May impact system performance for other users

## Solution

Use **SSHFS** to mount the remote filesystem to your local system, allowing you to use VS Code locally while working with remote files seamlessly.

## Benefits

- ✅ Full VS Code functionality with all extensions
- ✅ GitHub integration (push, pull, view changes, merge)
- ✅ AI-enhanced coding features
- ✅ User-friendly interface
- ✅ No resource usage on remote login nodes
- ✅ Works as if files are local

## Prerequisites

- macOS system
- SSH access to the remote HPC system
- Homebrew package manager

## Installation Steps

### 1. Install SSHFS

```bash
brew install macfuse
brew install --cask macfuse
brew install gromgit/fuse/sshfs-mac
```

### 2. Create Local Mount Point

```bash
mkdir -p ~/RemoteServer
```

### 3. Set Up SSH Key Authentication

Generate an SSH key pair for the server:
```bash
ssh-keygen -t ed25519 -C "nlt112@rocket.hpc.ncl.ac.uk"
```

Copy your public key to the server:
```bash
ssh-copy-id nlt112@rocket.hpc.ncl.ac.uk
```

Test passwordless login:
```bash
ssh nlt112@rocket.hpc.ncl.ac.uk
```

### 4. Mount Remote Filesystem

```bash
sshfs nlt112@rocket.hpc.ncl.ac.uk:/mnt/storage/nobackup/proj/spnmmd ~/RemoteServer 
```

### 5. Open in VS Code

```bash
code ~/RemoteServer
```

## Usage Tips

- **Unmounting**: To unmount the filesystem when done:
  ```bash
  umount ~/RemoteServer
  ```

- **Automatic Mounting**: Consider adding the mount command to a shell script or alias for easy access

- **Multiple Servers**: Create separate mount points for different remote systems

## Troubleshooting

### Connection Issues
- Verify SSH key authentication works without SSHFS first
- Check network connectivity to the remote server
- Ensure the remote path exists and you have access permissions

### Permission Problems
- Make sure your SSH key has proper permissions (600)
- Verify you have read/write access to the remote directory

### Performance
- Large file operations may be slower than working locally
- Consider working with smaller files when possible for better responsiveness

## Alternative Approaches

If SSHFS doesn't work for your setup, consider:
- Terminal-based editors like vim/emacs on the remote system
- File synchronization tools like rsync

## Security Notes

- Keep your SSH private key secure
- Use strong passphrases for SSH keys
- Follow your institution's security policies for remote access

---

**Note**: Replace `nlt112@rocket.hpc.ncl.ac.uk` and `/mnt/storage/nobackup/proj/spnmmd` with your actual username, server address, and remote path.
