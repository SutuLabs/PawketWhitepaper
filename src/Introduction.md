
# Introduction

Pawket: A secure handy wallet, developing in the agile process.

When the Pawket team embarked on the ambitious journey of creating the best possible blockchain wallet designed for Chia, we started by defining a few design principles. Here are some of the guiding principles that helped us shape Pawket in its current form:

- **No mandatory internet access**.
  It should not include any scripts hosted on a third-party domain or require you to create a user account on the server. While some functionalities such as retrieving balances, starting transactions, and third-party integrations will always depend on internet access, the main function of signing transactions must remain usable in a closed network with no internet access.
- **No secret key derivatives server-side**.
  Even an encrypted or derived version of the user mnemonics or private key should never be sent or stored server side.
- **Interoperable cryptography**.
  A user must be able to take away any account with the tools of their choice, not just the one provided by us.
- **Free and open source software**.
  Both the client and the server should be fully available in an open source license. The software stack required to run the server should also be open source. The software should not include any mandatory proprietary component of any kind.
- **Privacy by design. No tracking**.
  The software should not store unnecessary personal information. The software should not report back user behavior or analytics to any third-party website unless the administrator or the user explicitly opt-in.
- **Security by default**.
  The solution should propose sane security settings by default. A power user can however still adjust the settings to match their security requirements and risk appetite.

If these principles are also important to you, there are good chances that Pawket will be the right fit. The rest of the document will provide you with information about the implementation details and associated risks so that you can make that call.
