# RuneScape: Dragonwilds

### Authors / Contributors

<!-- prettier-ignore-start -->
<!-- markdownlint-disable -->
<table>
    <tr>
        <td align="center">
            <a href="https://github.com/DavidV98">
                <img src="https://github.com/DavidV98.png" width="50px;" alt=""/><br /><sub><b>DavidV98</b></sub>
            </a>
            <br />
            <a href="https://github.com/pterodactyl/game-eggs/commits?author=DavidV98" title="Codes">💻</a>
        </td>
    </tr>
</table>
<!-- markdownlint-enable -->
<!-- prettier-ignore-end -->

___

### Game Description

[RuneScape: Dragonwilds](https://dragonwilds.runescape.com/) is Jagex's co-op survival crafting game, set on the continent of Ashenfall in Gielinor. Gather, craft, build, use magic, and fight dragons with up to 5 friends. Unreal Engine 5. Left early access on 15 September 2026.

The dedicated server is a free Steam app (App ID `4019830`) with a native Linux build. You don't need to own the game to run it.

___

### Server Ports

One UDP port. Discovery runs through Epic Online Services, so there is no query port.

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game | 7777    | UDP      | Yes      |

Keep the internal and external port the same. Port translation isn't documented as supported. For multiple servers, give each one its own port.

___

### Requirements

- 64-bit only, no ARM.
- RAM: 2 GB, plus 1 GB per player. About 8 GB for a full server.
- Player cap is 6 and set by Jagex. `MAX_PLAYERS` can only lower it.

___

### Setup

Set `OWNER_ID` and `ADMIN_PASSWORD` before the first start. The server won't boot without an Owner ID. You'll find yours at the bottom of the in-game Settings menu, and it's the only role that can unban players.

The server is ready when the console prints:

```log
START SESSION - Success
```

Players join from **Worlds > Public** by searching the **world name**, not the server name. The search is case sensitive. `MyWorld` won't match `myworld`.

`WORLD_NAME` only names the world created on the first boot. Changing it later doesn't rename an existing world, the server keeps loading the existing save. Check the log to see which world loaded.

Server and client builds have to match. If the server is behind, it just doesn't appear in the list. Restart with `AUTO_UPDATE` on to pull the current build.

___

### Configuration

The egg writes these before every start, so edit the panel variables and restart. Editing the files directly won't stick.

`RSDragonwilds/Saved/Config/LinuxServer/DedicatedServer.ini`

```ini
[/Script/Dominion.DedicatedServerSettings]
OwnerId          = <OWNER_ID>
ServerName       = <SERVER_NAME>
DefaultWorldName = <WORLD_NAME>
AdminPassword    = <ADMIN_PASSWORD>
WorldPassword    = <WORLD_PASSWORD>
```

`RSDragonwilds/Saved/Config/LinuxServer/Game.ini`

```ini
[/Script/Engine.GameSession]
MaxPlayers = <MAX_PLAYERS>
```

Keys the server manages itself, like `ServerGuid`, are left alone.

`#`, `;`, `"`, `` ` `` and `\` are blocked by the variable validation rules. The panel's ini writer escapes them in a way Unreal reads back literally, which would corrupt the value.

___

### Save Files

Worlds are `.sav` files in:

```md
/home/container/RSDragonwilds/Saved/Savegames
```

The server loads the newest `.sav` on start, so keep one world in there. Logs are in `RSDragonwilds/Saved/Logs/RSDragonwilds.log`, with the build version at the top.

To move an existing world over: stop the server, back up and empty `Savegames` (keep the folder), upload your `.sav`, start. **Don't rename the file.** The searchable world name comes from the filename, so renaming it loses the world.

Back up `Savegames` before a game update. Use Stop, not Kill, so the world is written to disk.

___

### Known Issues

Upstream game issues, not egg bugs:

- Steam invites don't connect. Players join through the world browser.
- Editing `DedicatedServer.ini` while the server runs loses the changes.
- Jagex doesn't publish older build branches, so you can't roll a server back.
- A version mismatch hides the server from the list instead of showing an error.
