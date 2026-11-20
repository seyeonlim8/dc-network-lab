# P0 Environment Setup

Goal: a reproducible Linux lab host for containerlab + FRR on an Apple Silicon Mac.

## 1. Requirements

| Item | Tested version |
|---|---|
| Mac | Apple Silicon (M2), macOS 26.6.2 |
| OrbStack | 2.2.5 |
| VM OS | Ubuntu 24.04.5 LTS (noble), arm64 |

## 2. Create an isolated Ubuntu VM

[Mac]
```
orb create --isolated ubuntu:noble dclab
orb -m dclab
```

- Why `--isolated`: the VM has no access to Mac files (no `/mnt/mac`, no `/Users`), cannot run Mac commands, and does not receive the Mac SSH agent. A mistake inside the lab cannot change files on the Mac.
- Why `:noble`: without a version, OrbStack 2.2.5 creates Ubuntu 26.04 (the default). Pinning 24.04 LTS keeps the setup reproducible.

Verify [VM]:

| Command | Expected |
|---|---|
| `uname -m` | `aarch64` |
| `lsb_release -d` | `Ubuntu 24.04.5 LTS` |
| `ls /mnt/mac` | `No such file or directory` |
| `ls /Users` | `No such file or directory` |

## 3. Base packages

[VM]
```
sudo apt update
sudo apt install -y git curl
```

Verify: `git --version` -> `git version 2.43.0`, `curl --version` -> `curl 8.5.0 (aarch64-unknown-linux-gnu)`

## 4. GitHub access (repo-scoped deploy key)

[VM]
```
ssh-keygen -t ed25519 -C "dclab-vm deploy key (dc-network-lab)"
cat ~/.ssh/id_ed25519.pub
```

Add the public key on GitHub: repo Settings -> Deploy keys -> Add deploy key -> check "Allow write access".

```
ssh -T git@github.com
git clone git@github.com:seyeonlim8/dc-network-lab.git
```

- Why a deploy key: it grants access to this repository only. If the VM is compromised, other repositories and the GitHub account are not exposed. Revoke it by deleting the key in repo Settings.

## 5. Docker
TODO

## 6. containerlab
TODO

## 7. 2-node FRR eBGP lab
TODO

## Troubleshooting

| Symptom | Cause | Fix |
|---|---|---|
| On the Mac, `uname -m  # comment` prints `usage: uname`; `(text)` gives `invalid mode specification` | zsh does not treat `#` as a comment in interactive shells by default | `setopt interactivecomments` (add it to `~/.zshrc` to keep it) |
