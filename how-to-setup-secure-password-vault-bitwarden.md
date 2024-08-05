---
tags:
  - Wizardry
  - How-To
date: 2024-07-23T13:54
---
<!-- 2024-07-23 (July 23, 2024 1:54 PM Tuesday) -->

# Guide for Getting Started on the Right Foot in Bitwarden (Version 3.0)

1. Decide whether you want your Bitwarden account hosted on the cloud server `bitwarden.com` or on `bitwarden.eu`; if you're unsure, choose `bitwarden.com` (until recently, this was the _only_ available server option). Also decide which email address you will use as your Bitwarden username &mdash; it is recommended to use a unique email address (e.g., a "plus" address, like `myname+randomstring@domain.com`, which many email service providers will deliver to your regular mailbox at `myname@domain.com`).

2. Get a piece of paper and write "Emergency Sheet" at the top.  Then write down the Bitwarden cloud server that you plan to use (`bitwarden.com` or `bitwarden.eu`), as well as the email address that you will use for your Bitwarden login.  If you're paranoid or like to play secret agent, make sure that you write with the paper placed on a hard surface (not a notepad or magazine), and that you are alone in a closed room with all curtains drawn.

3. Click [this link](https://passhelp.github.io/generator/#phrase:4) _once_, and copy down the displayed phrase on your piece of paper.  This will be your master password.  Unless you have a medical condition, you will be able to memorize it with some practice (you were able to memorize your mailing address, telephone number, names of friends and relatives, and similar information; memorizing your master password is not much harder &mdash; but accept that it _will_ take a bit of practice).  If you're concerned about the security of online password generators, then start by saving the linked passphrase generator webpage as a local `.html` file, disconnect your device from the internet, and thereafter open the locally saved `.html` file for generating your passphrase.

4. Register your Bitwarden account either on the [bitwarden.com server](https://vault.bitwarden.com/#/register) or on the [bitwarden.eu server](https://vault.bitwarden.eu/#/register).  Use a fake name if you wish, and leave the _Password Hint_ blank for now.

5. When you first log in upon account registration, there is an option to [Verify Email](https://community.bitwarden.com/uploads/default/original/1X/b55ede60e0419a333067ec4a093341df758ab2a3.png), which you should use.

6. Optionally, upgrade your subscription to Premium if you wish to use [Premium features](https://bitwarden.com/help/password-manager-plans/#compare-personal-plans).

7. In the Web Vault app, go to the "Two-Step Login" section of Security settings, enable a 2FA method for your Bitwarden account.  I recommend purchasing one or more [Yubikey Security Keys](https://www.yubico.com/product/security-key-series/security-key-nfc-by-yubico-black/) for the purpose of securing your Bitwarden account.  To set this up in Bitwarden, click "Manage" for the [Passkey](https://bitwarden.com/help/setup-two-step-login-fido/) provider, and register your Yubikeys there (_not_ under "Yubico OTP Security Key"). Personally, I have 3 security keys; I keep one on my person, one at home, and one at work.

8. IMPORTANT: Before leaving the "Two-Step Login" section, [get your 2FA Recovery Code](https://bitwarden.com/help/two-step-recovery-code/#get-your-recovery-code).  Accurately transcribe this code onto your "Emergency Sheet" paper.

9. In the "Keys" section of Security settings, [change your KDF algorithm](https://bitwarden.com/help/kdf-algorithms/#changing-kdf-algorithm) to Argon2id.  Keep the default settings unless you use iOS devices, in which case you should decrease the "memory" setting to 48 MB and increase "iterations" to 4.

10. Populate your vault by [importing passwords](https://bitwarden.com/help/import-data/) that had been stored elsewhere, or by creating new vault items from scratch.

11. [Download](https://bitwarden.com/download/) and install the Bitwarden client apps that you wish to use, and configure the settings in each.  It is recommended to set the vault Timeout Action to "Lock" instead of "Log out", and to use a relatively short Timeout Period.  Also enable to option that clears the system clipboard after a short delay.

12.  Make your first backup, by creating a vault export from one of the _non-mobile_ Bitwarden apps (i.e., Web Vault app, Desktop app, or browser extension), being sure to select the [encrypted `.json`  file format with the "Password Protected" option for the export type](https://bitwarden.com/help/encrypted-export/#create-an-encrypted-export).  Use the same method as before to create a strong password for your backup file, but this time, make it a [6-word passphrase](https://passhelp.github.io/generator/#phrase:6); write down the backup file password on your "Emergency Sheet" paper.  In addition, create an entry in your Bitwarden vault to save the backup file password (which will make it easier to use the password when you create future backups).

13. Use your Emergency Sheet as a "cheat sheet" for typing in your master password when logging in or unlocking your vault, until you have acquired to muscle memory to type it by heart (approximately one week, give or take).

14. Seal your Emergency Sheet in a security envelope (which you can purchase or [make yourself](https://passwordbits.com/emergency-sheet-envelope/)), and store it in a secure location.  Optionally, make one or more redundant copies of the Emergency Sheet, to store in different locations.

15. Optionally, update your Password Hint to contain a clue about where your Emergency Sheet is hidden.  To change your Password Hint, log in to the Web Vault and use the password change form, but type in your _existing_ master password into the new password field (so that the master password is _not_ changed), and do _not_ check the option for rotating your account encryption key.

16. If you use a Bitwarden browser extension (which is recommended), then [pin the extension icon](https://bitwarden.com/help/getting-started-browserext/#pin-the-extension) to the top of your browser window and disable the browser's built-in password manager (also disable any 3rd-party password managers that you may have installed prior to switching to Bitwarden). Optionally, make the following changes to the browser extension settings: 

* &nbsp;&nbsp;&nbsp;(a) Enable the Account Security option "Unlock with PIN" (but do _not_ disable "Lock with master password on browser restart"), defining the PIN to be a short passphrase or password that is easier to type than your master password.

* &nbsp;&nbsp;&nbsp;(b) Turn off the Auto-Fill option to "Show auto-fill menu on form fields" (there are 5 other ways to auto-fill, the best of which is the `Ctrl+Shift+L` keyboard shortcut &mdash; or `Cmd+Shift+L` on macOS).

* &nbsp;&nbsp;&nbsp;(c) Disable the Notification options "Ask to add login" and "Ask to update existing login" (it is better to add logins by first creating the account credentials directly in the browser extension, and the using auto-fill to transfer the credentials into the account registration form).

* &nbsp;&nbsp;&nbsp;(d) Disable the Notification option "Ask to save and use passkeys" unless you are sure that you want to store passkeys in your Bitwarden vault (passkeys are a "bleeding-edge" technology that may need some additional time to mature before the user experience is optimized).

* &nbsp;&nbsp;&nbsp;(e) If [privacy concerns](https://bitwarden.com/help/website-icons/#privacy-considerations) are important to you and don't mind the slightly degraded UI visuals, disable the Appearance option "Show website icons".

There are myriad additional options and advanced functions in Bitwarden, but the above covers all of the basics!  Update your backup export on a regular basis using the method from Step 12.  Don't use your master password or backup password anywhere else, and do not let anyone know what these passwords are.  Keep your devices secure, and malware free, and you should be good to go.


# References
- https://www.reddit.com/r/Bitwarden/s/iUNW89JMBS 
