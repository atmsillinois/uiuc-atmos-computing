# Accessing a Visual Studio Code Tunnel on a keeling compute node

This guide covers setting up and using a VS Code Remote Tunnel from a Mac or PC to a compute node on Keeling, via the `seseml` Slurm partition.

> **Network requirement:** You must be on the UIUC campus network or connected to the [UIUC VPN](https://techservices.illinois.edu/vpn) to SSH into Keeling.

---

## One-Time Setup

These steps only need to be done once (or very rarely).

### 1. Install VS Code Locally (Mac or PC)

- Download from [https://code.visualstudio.com](https://code.visualstudio.com), or your local Software Center from SESE IT if available.
- Install as usual for your platform.

### 2. Install the Remote - Tunnels Extension in VS Code

1. Open VS Code on your local machine.
2. Open the Extensions panel (`Cmd+Shift+X` on Mac, `Ctrl+Shift+X` on PC).
3. Search for **"Remote - Tunnels"** (publisher: Microsoft).
4. Click **Install**.

> This extension lets VS Code connect to a running tunnel on a remote machine.

### 3. Install the VS Code CLI on Keeling (Linux)

SSH into the Keeling head node:

```bash
ssh <your-netid>@keeling.earth.illinois.edu
```

Download and install the VS Code CLI for Linux:

```bash
cd ~
curl -Lk 'https://code.visualstudio.com/sha/download?build=stable&os=cli-alpine-x64' -o vscode_cli.tar.gz
tar -xzf vscode_cli.tar.gz
```

This extracts a single binary called `code`. Move it somewhere on your `$PATH` (or just reference it from `~/`):

```bash
mkdir -p ~/bin
mv code ~/bin/
```

Make sure `~/bin` is on your `$PATH`. Add this to your `~/.bashrc` if it isn't already:

```bash
export PATH="$HOME/bin:$PATH"
```

Then reload:

```bash
source ~/.bashrc
```

Verify:

```bash
code --version
```

### 4. Authenticate the Tunnel (First Time Only)

When you first run `code tunnel`, it will ask you to authenticate with a GitHub or Microsoft account. You only need to do this once — the credentials are cached.

You'll do this during your first session (see below). It will print a URL and a code. Open the URL in your browser, paste the code, and authorize. After that, re-authentication is not needed unless the token expires (rare).

---

## Starting a Session (Do This Each Time)

### Step 1: SSH into Keeling

From your Mac or PC terminal:

```bash
ssh <your-netid>@keeling.earth.illinois.edu
```

### Step 2: Start a Persistent `screen` Session

Create a named screen session:

```bash
screen -S vscode
```

Inside the screen session, start an 8-hour auto-kill timer so the session cleans itself up to match your Slurm job lifetime:

```bash
(sleep 8h && screen -S vscode -X quit) &
```

> **Why screen?** If your SSH connection drops, the screen session (and everything running inside it) stays alive on Keeling. You can reconnect and reattach. The background timer ensures the session doesn't linger after your job ends.

### Step 3: Request a Compute Node via Slurm

Inside the screen session, request an interactive job:

```bash
srun --partition=seseml --time=08:00:00 --mem=100G --cpus-per-task=4 --pty bash
```

Breakdown of the flags:
- `--partition=seseml` — submit to the `seseml` partition
- `--time=08:00:00` — wall-clock time limit of 8 hours
- `--mem=100G` — request 100 GB of total RAM for the job
- `--cpus-per-task=4` — request 4 CPU cores
- `--pty bash` — allocate a pseudo-terminal and launch an interactive bash shell

Wait until you land on a compute node. Your prompt will change (e.g., from `keeling` to something like `keeling-d04` or `keeling-j03`).

### Step 4: Start the VS Code Tunnel

On the compute node, run:

```bash
code tunnel --name keeling
```

- `--name keeling` gives the tunnel a fixed, recognizable name. You can use any name you like.

**First time only:** You'll see output like:

```
*
* Visual Studio Code Server
*
* By using the software, you agree to
* the Visual Studio Code Server License Terms (https://aka.ms/vscode-server-license)...
*
* To grant access to the server, please log into https://github.com/login/device
* and use code XXXX-XXXX
```

1. Open the URL in your browser.
2. Enter the code shown.
3. Authorize with your UIUC M
icrosoft or GitHub account.

After authentication, you'll see:

```
✔ Tunnel is ready
Open this link in your browser https://vscode.dev/tunnel/keeling
```

The tunnel is now running.

### Step 5: Detach from Screen

Press `Ctrl+A`, then `D` to detach from the screen session. The tunnel keeps running in the background.

You can now close your SSH connection entirely if you want.

### Step 6: Connect from VS Code Locally

1. Open VS Code on your Mac or PC.
2. Open the Command Palette (`Cmd+Shift+P` on Mac, `Ctrl+Shift+P` on PC).
3. Type **"Remote-Tunnels: Connect to Tunnel..."** and select it.
4. Sign in with the same Microsoft/Github account you used to authenticate the tunnel.
5. You should see **`keeling`** (or whatever name you chose) listed. Select it.

VS Code will connect and open a remote session on the Keeling compute node. You now have full access to the remote filesystem, terminal, extensions (which will install on the remote side as needed), and all your data.

Alternatively, open `https://vscode.dev/tunnel/keeling` in any browser for a web-based editor (no local VS Code install needed).

---

## Reconnecting During the 8-Hour Window

If your SSH drops, your laptop sleeps, or you just closed VS Code:

### From VS Code (Easiest)

Just reopen VS Code and connect to the tunnel again:

1. Command Palette → **"Remote-Tunnels: Connect to Tunnel..."**
2. Select **`keeling`**.

You need to be on the UIUC network or VPN for this to work.

That's it. The tunnel is still running inside screen on the compute node.

### If You Need to Check on the Tunnel via SSH

```bash
ssh <your-netid>@keeling.earth.illinois.edu
screen -r vscode
```

This reattaches you to the screen session so you can see the tunnel output. Detach again with `Ctrl+A`, `D`.

---

## When the Session Expires

After 8 hours, the Slurm job ends, the compute node is released, and the tunnel dies. To start again, repeat the **"Starting a Session"** steps above (Steps 1–6). You will **not** need to re-authenticate with GitHub — that credential is cached.

---

## Quick Reference: Setup vs. Routine

| Task | When |
|---|---|
| Install VS Code locally | Once |
| Install Remote - Tunnels extension | Once |
| Install VS Code CLI on Keeling (`~/bin/code`) | Once (update occasionally) |
| Authenticate tunnel with GitHub | Once (token cached) |
| SSH → screen → srun → `code tunnel` | Every session |
| Connect from local VS Code | Every session (and on reconnect) |

---

## Troubleshooting

**Tunnel name already in use:** If a previous tunnel with the same name didn't shut down cleanly, run `code tunnel kill` on the head node before starting a new one, or use `code tunnel rename`.

**Can't find the tunnel in VS Code:** Make sure you're signed into the same GitHub/Microsoft account on both ends.

**Screen session gone:** If `screen -r vscode` says "no screen to be resumed," your head-node session may have been cleaned up. Just start fresh from Step 2.

**Job pending:** The `seseml` partition may be full. Check queue status with `squeue -p seseml` and wait, or try during off-peak hours. You can also check your pending job with `squeue -u $USER`.

**Updating the VS Code CLI:** Re-download and extract the tarball (Step 3 of One-Time Setup) to update. Do this if the tunnel complains about version mismatches with your local VS Code.