# dungeon

### 👉 `ssh dungeon@tplewe.com`

This is a proof-of-concept for a [multi-user dungeon](https://en.wikipedia.org/wiki/Multi-user_dungeon) over SSH. (They are usually connected to over telnet.) Just an idea I had and started working on.

Enter the ssh command above in a terminal to try it out!

For best results, use an up-to-date terminal emulator that supports 24-bit true color (chances are yours does), and make your terminal window fullscreen. I've tested it in Windows Terminal, Warp, WezTerm, Alacritty and Git Bash and it works great in all of them. The integrated terminal inside both VS Code and Zed don't render the colors right for some reason.

---

## Structure
One `server` process must be running on the target Linux machine. An account is set up (which does not require a password) called `dungeon` which anyone can SSH into. The shell program configured for that account is the `client` program. The `server` process communicates with all the different `client` processes over a [Unix socket](https://en.wikipedia.org/wiki/Unix_domain_socket), using a custom binary protocol (see [`PROTOCOL.md`](PROTOCOL.md)).

## Building
### Requirements
- both client and server programs must run on a Unix-based machine
- `make` (mine is v4.3)
- [Go](https://go.dev) v1.24.5
### Steps
1. Run the following in the root `dungeon/` directory:
    ```sh
    make
    ```
1. if your long-running server service and dungeon account config are expecting the executables to be in certain places, copy both the `bin/dungeon-server` and `bin/dungeon-client` executables to those places (such as the "dungeon" account's root directory)
2. set up a long-running service to make sure the `bin/dungeon-server` is always running (e.g. `sudo systemctl restart dungeon-server`)
3. clients should now be able to log into the server via SSH
