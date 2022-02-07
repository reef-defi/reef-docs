---
title: "Ledger"
description: "Ledger Nano S and Nano X support for Reef chain"
lead: ""
date: 2020-10-06T08:48:57+00:00
lastmod: 2020-10-06T08:48:57+00:00
draft: false
images: []
menu:
  docs:
    parent: "users"
weight: 130
toc: true
---

{{< alert icon="⚠️" text="Ledger support is currently only available for advanced users. Reef app is currently pending Ledger store approval, after which it will be available for everyone." >}}

### Ledger device
A Ledger device is a hardware wallet that is considered one of the most secure ways to store your digital assets. Ledger uses an offline, or cold storage, method of generating private keys, making it a preferred method for many crypto users. This guide will help you to connect your Ledger device to the Reef extension. Using an app listed above, such as [Reef console](https://console.reefscan.com) you can then use Reef extension to safely send REEF, stake REEF and other various actions from your hardware wallet account.

#### Requirements
- a Ledger Nano S or a Ledger Nano X
- [set up](https://support.ledger.com/hc/en-us/articles/360000613793?docs=true) your Ledger device
- install [Ledger Live](https://support.ledger.com/hc/en-us/articles/4404389606417-Download-and-install-Ledger-Live?docs=true) on your device
- install [Reef extension](/docs/users/extension/) on your Chrome-like browser (Google Chrome, Chromium, Brave). At least 0.41.3 version is required.


#### Install Reef app
1. Open the _Manager_ in Ledger Live.
2. Connect and unlock your Ledger Device.
3. If asked, follow the onscreen instructions and _Allow Ledger Manager_.
4. Find _Reef_ app in the app catalog.
5. Click the Install button:
![install Reef app on Ledger device](https://docs.reef.finance/blog/reef-chain-v8-is-live-on-mainnet/ledger.jpeg)
6. Wait for the app to install and then close the Ledger Live.


### Using the Developer Release

{{< alert icon="💡" text="NOTE: These instructions are for development installation only. It is recommended to install the application from Ledger Live unless you know exactly what you're doing." >}}


Instructions for downloading the pre-release binary from the GitHub releases are written in the README for the Reef Ledger application [GitHub repository](https://github.com/reef-defi/ledger-reef).

On the [releases page](https://github.com/reef-defi/ledger-reef/releases) you can download the shell script `install_app.sh` and then make it executable in your shell by typing the command `chmod +x install_app.sh`.

Using `install_app.sh` help command will show you the available options:

```
$ ./install_app.sh --help
Zondax Installer [Reef-2.8.3] [Warning: use only for test/demo apps]
  load    - Load Reef app
  delete  - Delete Reef app
  version - Show Reef app version
```

Next, you must make sure your Ledger device is plugged in and unlocked and you're using the latest firmware. If everything is prepared, then type `./install_app.sh` load and accept the prompts on your Ledger device to install the application.

First it will prompt you to _Allow an unsafe manager_ - confirm this by switching the screen to the allow screen and pressing the corresponding buttons.

After some processing time, the screen of your device will update to say _Install app Reef_. Navigate all the way to the right, verify the Identifier hash matches the one that is printed in your terminal. Click both buttons on _Perform Installation_ to install the application. It will ask again for your PIN code.

At the end of the process you should have the newly installed Reef application on the device.


#### Using a Ledger device on [Reef console](https://console.reefscan.com)

{{< alert icon="💡" text="Note: Ledger Live should be off while using Ledger with Reef console as it can interfere with normal operation." >}}


##### Loading Your Account

Reef console already has an integration with the Ledger application so that your device will work with the browser interface after installation. The functionality is currently gated behind a feature setting that you will need to turn on.

In order to turn on the interoperability with the Reef Ledger application, go to the [_Settings_](https://console.reefscan.com/?rpc=wss%3A%2F%2Frpc.reefscan.com%2Fws#/settings) tab in Reef Console. Find the option for attaching Ledger devices and switch the option from the default _Do not attach Ledger devices_ to _Attach Ledger via WebHID_. Be aware: if you are not seeing this it is because there is [no Ledger support on Firefox](https://github.com/polkadot-js/apps/issues/3771).

Click `Save` to keep your settings. Refresh the Reef Console website.

Once you confirm your selection, depending on your browser and its security settings, you might need to confirm the USB connection through a popup when adding the Ledger device for the first time.

Now we can load our Ledger device account. Open Reef extension window and select _Attach ledger account_:
![Attaching ledger account](/docs/users/attach_ledger_account.png)

Select the network on which you want to use the ledger and enter a descriptive name. Make sure the Reef app is open and focused on the Ledger device (it should say _Reef Ready_). Select the _Account type_ and _Account index_ (select 0 for both if you are not sure).

Click _Import account_ and the account should be imported, designated by the USB icon.

Refresh the Reef console website to reload the Reef extension accounts. The Ledger account should not be visible in the Reef Console:

![Reef account](/docs/users/ledger_account_console.png)

You can now use this account to interact with Reef apps and it will prompt your ledger for confirmation when you initiate a transaction.

##### Checking the account balance
If you added the account to the extension as describe above, you can visit [Reef console](https://console.reefscan.com) to see the available balance. Otherwise you can see the balance on our [Reefscan explorer](https://reefscan.com/).


##### Sending a transfer
1. Go to [Reef console](https://console.reefscan.com).
2. Click _Send_ next to the injected Ledger device account.
3. Change _send to address_ to the desired account and provide the REEF amount.
4. Click _Make Transfer_.
5. Click _Sign and submit_.
6. A popup should appear. Make sure your Reef app is opened on the Ledger device (it should say _Reef Ready_).
7. Click on _Sign on Ledger_.
8. The transaction data should appear on the Ledger device. Review the data and then move to the right and confirm the transaction by clicking both Ledger device buttons.
9. Wait for the transaction to be included. At this point the transaction was successful.

##### Receiving a Transfer

In order to receive a transfer on the accounts stored on your Ledger device, you will need to provide the sender (i.e. the payer) with your address.

The easiest way to get your address is to click on the account name in Reef Console which will open a sidebar. Your address will be shown in this sidebar, along with some other information. Another method is just clicking on your account's avatar icon - this immediately copies your address to the clipboard. You can also copy it in the Reef extension by clicking _Copy_ icon next to the address.

##### Staking

Since Ledger does not support batch transactions, you must do two separate transactions when you want to stake using an account stored on a Ledger device.

 - Go to the [_Staking_](http://localhost:3000/?rpc=wss%3A%2F%2Frpc.reefscan.com%2Fws#/staking) tab found under the _Network_ dropdown in the top navigation menu.
 - Click the _Account Actions_ pane in the inner navigation.
 - Click _+ Stash_ instead of _+ Nominator_ or _+ Validator_ (selecting the latter two will not work).
- Input the amount of tokens to bond and confirm the transaction.
- Confirm the transaction on the Ledger device.
- When the transaction is included you will see the newly bonded account in the _Account Actions_ page.
- Select _Start Nominating_ or _Start Validating_ to start nominating or validating.
- Confirm the transaction on the Reef Console and on the Ledger device.


##### Support

If you need support visit one of the channels [here](/docs/help/faq/#where-can-i-get-support).
