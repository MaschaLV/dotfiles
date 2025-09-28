# Setting up ssh-agent with KWallet for VS Code on Arch Linux

## Problem

On Arch Linux, Visual Studio Code asks for the SSH key password every
time you push or pull, even though KWallet and ksshaskpass are
installed. The reason is that VS Code doesn’t directly talk to KWallet;
it expects to communicate with ssh-agent via the SSH_AUTH_SOCK
environment variable. By default, Arch does not ship a systemd service
for ssh-agent, so no socket is exported.

## Solution Overview

1.  Create a user systemd service for ssh-agent.
2.  Enable and start it.
3.  Export the SSH_AUTH_SOCK variable so that GUI apps (like VS Code)
    can use it.
4.  Add your key to the agent once per login (KWallet will handle the
    password prompt).
5.  Optionally configure KWallet to auto-unlock and add keys.

## Step 1: Create the service unit
Run:

    mkdir -p ~/.config/systemd/user

Create the file ~/.config/systemd/user/ssh-agent.service with the
following content:

    [Unit]
    Description=SSH key agent

    [Service]
    Type=simple
    Environment=SSH_AUTH_SOCK=%t/ssh-agent.socket
    ExecStart=/usr/bin/ssh-agent -D -a $SSH_AUTH_SOCK

    [Install]
    WantedBy=default.target

## Step 2: Enable and start the service

Reload and enable the service:

    systemctl --user daemon-reload
    systemctl --user enable --now ssh-agent

## Step 3: Export SSH_AUTH_SOCK globally

Create the file ~/.config/environment.d/10-ssh-agent.conf with the following content:

    SSH_AUTH_SOCK=%t/ssh-agent.socket

Then reload the environment or log out and back in:

    systemctl --user import-environment SSH_AUTH_SOCK

## Step 4: Verify the setup

Check that the socket is set:

    echo $SSH_AUTH_SOCK

Add your key (KWallet will prompt):

    ssh-add ~/.ssh/id_ed25519

List keys:

    ssh-add -l

## Step 5: Test in VS Code

Open a new terminal in VS Code and run:

    ssh-add -l

You should see your key listed. Now Git push/pull will work without
asking for the password.

## Add startup entry (KWallet)
Create a startup entry with the following command (on first login, KWallet will catch the ssh-add and use ssh-askpass to store the password, and load the key the following logins)

    ssh-add ~/.ssh/id_ed25519
