# Minecraft Server Setup & Operations Guide

## Requirements

- Java 21+ installed ([download here](https://adoptium.net/))
- Ngrok account ([ngrok.com](https://ngrok.com)) — **free tier requires adding a card for TCP tunnels**
- Windows machine (these instructions are Windows-specific)

---

## 1. Starting the Minecraft Server

Open a terminal (Command Prompt or PowerShell) in this folder and run:

```bat
java -Xmx4G -Xms2G -jar server.jar nogui
```

- `-Xmx4G` — maximum RAM allocated (4 GB). Increase if you have more RAM to spare.
- `-Xms2G` — initial RAM allocated (2 GB).
- `nogui` — runs without the graphical window (faster, lower memory).

The server is ready when you see:
```
[Server thread/INFO]: Done (Xs)! For help, type "help"
```

To stop the server, type `stop` in the terminal and press Enter.

---

## 2. Port Forwarding with Ngrok

Ngrok creates a public tunnel to your local server so friends can connect without needing to configure your router.

### 2a. Install Ngrok

1. Go to [ngrok.com/download](https://ngrok.com/download) and download the Windows ZIP.
2. Extract `ngrok.exe` to a convenient folder (e.g., `C:\ngrok\`).
3. Add that folder to your system PATH, **or** run `ngrok.exe` directly from that folder.

### 2b. Authenticate Ngrok

After creating a free account at [ngrok.com](https://ngrok.com):

1. Go to your [Ngrok Dashboard → Your Authtoken](https://dashboard.ngrok.com/get-started/your-authtoken).
2. Copy your authtoken and run:

```bat
ngrok config add-authtoken YOUR_AUTH_TOKEN_HERE
```

### 2c. Add a Payment Card (Required for TCP tunnels)

Ngrok's free plan requires a verified account with a card on file to use TCP tunnels (Minecraft uses TCP).

1. Log in at [ngrok.com](https://ngrok.com).
2. Go to **Billing** → **Add Payment Method**.
3. Enter your card details. You will **not be charged** unless you upgrade to a paid plan.
4. Once the card is saved, TCP tunnels are unlocked on your free account.

### 2d. Start the Tunnel

With the Minecraft server already running, open a **second terminal** and run:

```bat
ngrok tcp 25565
```

You will see output like:

```
Forwarding  tcp://0.tcp.ngrok.io:XXXXX -> localhost:25565
```

Share the address `0.tcp.ngrok.io:XXXXX` with your friends. They enter it exactly as shown in Minecraft → **Multiplayer → Add Server → Server Address**.

> **Note:** The port number changes every time you restart Ngrok unless you have a paid plan with a reserved TCP address.

---

## 3. Connecting to the Server

| Scenario | Address to use |
|---|---|
| Same local network (home WiFi) | `localhost:25565` or your local IP (e.g. `192.168.x.x:25565`) |
| Over the internet via Ngrok | `0.tcp.ngrok.io:XXXXX` (from Ngrok output) |

---

## 4. Editing Server Settings

All server configuration is in [server.properties](server.properties). Edit it with any text editor (Notepad, VS Code, etc.) **while the server is stopped**.

### Common settings to change

| Property | Default | What it does |
|---|---|---|
| `motd` | `A Minecraft Server` | Message shown in the server list |
| `max-players` | `20` | Maximum simultaneous players |
| `gamemode` | `survival` | Default gamemode (`survival`, `creative`, `adventure`, `spectator`) |
| `difficulty` | `easy` | World difficulty (`peaceful`, `easy`, `normal`, `hard`) |
| `hardcore` | `false` | Set `true` for hardcore mode (permadeath) |
| `white-list` | `false` | Set `true` to only allow whitelisted players |
| `enforce-whitelist` | `false` | Kicks non-whitelisted players if whitelist is enabled mid-session |
| `spawn-protection` | `16` | Radius (in blocks) around spawn that only ops can build in. Set `0` to disable. |
| `view-distance` | `10` | Chunk render distance. Lower this to reduce server load. |
| `online-mode` | `true` | Set `false` to allow cracked (non-premium) clients. **Not recommended.** |
| `level-seed` | _(current seed)_ | Seed for world generation. Only used when creating a new world. |
| `op-permission-level` | `4` | Permission level for ops (1–4). Level 4 = full access. |

### Applying changes

1. Stop the server (`stop` in the server terminal).
2. Edit `server.properties`.
3. Save the file.
4. Start the server again.

### Managing ops (admins)

In the server console, run:

```
op PlayerName
```

To remove op:

```
deop PlayerName
```

### Managing the whitelist

Enable whitelist in `server.properties` by setting `white-list=true`, then in the console:

```
whitelist add PlayerName
whitelist remove PlayerName
whitelist list
```

---

## 5. Quick Reference

```bat
# Start server
java -Xmx4G -Xms2G -jar server.jar nogui

# Start Ngrok tunnel (in a separate terminal)
ngrok tcp 25565

# Stop server (in server terminal)
stop
```
