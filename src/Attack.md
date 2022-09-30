# Impact on Potential Attack Scenarios

## Compromised Network

### Man in the middle

If an attacker can break the TLS connection between the client and the server they will have access to the unencrypted data, and as such be able to capture the raw spend bundle for MEV or user-interested addresses for identity linking.

To reduce these risks a Pawket administrator must ensure that the server SSL configuration is configured to modern standards for example by disabling support for weak algorithms.


## Compromised client

### Memory access

Pawket does not protect the end user in a scenario where the attacker would be able to read the content of the memory on the client, e.g. a scenario where an attacker is capable of breaking the browser sandbox. Regular organizations should be fine by making sure the end user browsers are up to date and without malicious extensions installed.

### Filesystem / keylogger access

Similarly, Pawket does not fully protect the end-user in a scenario where the attacker has read access to the local filesystem or has a keylogger installed. It would be possible in this scenario for example to gather the secret key from the local storage and the passphrase using a keylogger. Enforcing general end user endpoint security best practices (such as having an anti-virus in place, patched operating system, etc.) should be enough to mitigate the remaining risks to an acceptable level for most organizations.

### Clipboard access

Similarly, Pawket cannot prevent another application or web extension with clipboard access right to listen to clipboard changes and protect the password if the user chooses to use this functionality in a compromised environment. Reducing the number of installed applications and extensions to a minimum is a good risk mitigation measure.

### Input method

Very similar to keylogger, but this is much more common than that scenario. When importing mnemonics, one could commonly input mnemonics with an input method, but this could leak these sensitive mnemonics through the input method provider, especially when the input method would collect the inputs to do some analysis later. Using the system input method without collecting could save you.

### Weak RNG

Pawket utilize browser built-in RNG (random number generator) for mnemonics generation (`crypto.getRandomValues()`). It's critical to have safe mnemonics by the safe RNG. Pawket does not protect the end user in a scenario where the attacker has changed the behavior of browser RNG.

## Misconfigured client

### Weak master password

At the moment there are no rules to enforce that a password must be strong even though it is encouraged. For example, it is possible for a user to setup a password with one digit.

### Reserved Security Token

Our research shows that the majority of users do not understand the concept of phishing and therefore do not understand the concepts behind the reserved security token. Additional training and prompts may be required for this mechanism to be useful.

## Malicious Web Application

### Rogue vendor employee

An attacker with access to the Pawket server or CDN would be able to distribute a malicious web application. To mitigate this risk, every developer of the Pawket team must use a strong password, only with the approval of maintainers, the code can be published. Moreover, notifications are sent to the maintainers and committee after a publication.

## Malicious dApp

### Misleading Guide

When a transaction is raised by dApp, it may intend to give the user misleading guidance, and inject misleading text (e.g. "Official", "Pawket"). Pawket would give the user a visually distinguished area of dApp raised text.

### Malicious Chialisp Program

All code is executed in the sandbox, it's safe to get the execution result of the Chialisp program. However, the generated UTXO may have a flaw, e.g. the UTXO can be transferred by an unauthenticated malicious adversary. Pawket has a built-in mechanism to check some common issues of spend bundles, but not all issues can be found. Users are encouraged to use dApp with reputation and audit reports to avoid this type of attack.