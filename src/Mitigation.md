# Risks Mitigation Strategies

## Offline mode

The most important risk mitigation strategy is offline mode. Pawket can fully work in offline mode with degraded function. Only signing is allowed in this mode. The communication between offline devices and blockchain is through the QR code.

## Fingerprint

Mnemonics and Account fingerprints are fully compatible with Official Chia Wallet.

## Malicious Spend Bundle Discovery

When interacting with unfamous dApp, no reputation can be used to prove they are trustful to generate a safe spend bundle. With the coin analysis tool, Pawket can quickly find malicious coins or corrupted spend-bundle and give users a significant warning message.

## Coin/Offer Analysis

Power users are also allowed to analyze coin/offer thoroughly.

TODO: add analysis UI image

## Phishing

In this attack scenario, an attacker would create a page looking like a regular login page, or inject an additional javascript on a legitimate Pawket page.

Pawket mitigates this type of attack by using a "reserved security token". After the user is unlocked with the right password, the user will be welcomed with this "reserved security token". To prevent an attacker from placing a transparent input dialog on top of the input box, for example, to capture the passphrase, a field focus event displays an additional interaction in place with color changes.

## Cross Site Scripting (XSS)

High on the list of web application vulnerabilities, an XSS vulnerability would allow an attacker to run arbitrary content on the page.

### Persistent XSS

In this scenario, an attacker would submit malicious data to the server that would then be executed on the page when accessed by another user.

While Pawket server API would provide textual information and the Chialisp program. We assume it is the responsibility of the clients to treat the server information as hostile and take care of the sanitization.

In practice, in most cases, this means only rendering the information as text and using escaping facilities provided by the templating libraries used by Pawket (Vue) and template literals.

One common threat is the execution of the Chialisp program. First, the Chialisp program is designed to be a purely functional programming language, meaning no side effects exist so that no environment would be affected. Second, the Chialisp program would be executed in an iframe sandbox to avoid any possible affection.

Additionally, Pawket is preventing running inline javascript or including javascript files from third-party domains.

### Reflected XSS

In this scenario, an attacker would craft a link (for example in an email) to run arbitrary code when the user navigates on the Pawket domain.

In certain cases, Pawket uses parameters provided in URLs. URL parameters are used, for example, to directly launch a send transaction (which allows 3rd party dApp to launch Pawket for sending) or accept an offer.

The attack surface for reflected XSS is reduced by running validation on the requested content, for example, the address of send target would be decoded by bech32m first, and the Chialisp program executes in the iframe sandbox.

### Unsafe methods

The execution of javascript `eval` statements is not allowed.


### SQL Injection

The server-side application seldom uses direct SQL queries but instead accesses the data through the framework ORM. By default, these database abstraction layers prevent most SQL injection issues. User input used by the ORM is always carefully validated. The remaining risks are mitigated using code reviews.


### File upload

File upload is another sensitive area for web applications. Pawket at the moment only supports offer upload. The offer file size limit can be controlled by the administrator using the webserver environment variables.

### CSRF

Cross-Site Request Forgery is an attack scenario where a user's action on a malicious third-party site would trigger a modification of data on a website, such as editing a resource or deleting it, crafting a malicious image URL, or having the user submit a form from another domain.

The Pawket server is stateless and authenticate-less, so no one can launch such an attack on the Pawket server.

### Recovery risks

To recover an account a user must import their mnemonics and optional the accounts to regain access to their accounts. This process makes an attack scenario quite complicated but reduces the usability for the end user by introducing chances of losing access to the account permanently if the mnemonics are lost.

### Source Control

Pawket utilizes git for source control, and Azure DevOps for fully automated CI/CD. Both codes in client and server have an adequate level of coverage. Tests are forced during the continuous integration process.

For every release, digital signatures are published to help the end-user or enterprise administrator ensure the integrity of each release.

Only a small amount of people are responsible and allowed to publish code. Before being pushed on a sensitive branch, each pull request is therefore reviewed and validated by different maintainers.

### Bug bounty

When we have a sponsor, we will run a bug bounty program to further increase the security level.

<!-- ### 3rd Party Audits -->
