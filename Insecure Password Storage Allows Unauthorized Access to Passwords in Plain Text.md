# First Flight #1: PasswordStore - Findings Report 
### Project: First Flight #1

### Dates: Oct 18th, 2023 - Oct 25th, 2023
  
[See more details here](https://www.codehawks.com/contests/clnuo221v0001l50aomgo4nyn)

## Insecure Password Storage Allows Unauthorized Access to Passwords in Plain Text

### Relevant GitHub Links
	
https://github.com/Cyfrin/2023-10-PasswordStore/blob/856ed94bfcf1031bf9d13514cb21b591d88ed323/src/PasswordStore.sol#L26-L29

## Summary
- This report outlines a significant security vulnerability discovered in the "PasswordStore" smart contract, which allows unauthorized access to the stored password. The vulnerability permits an attacker to access and reveal the password stored in the contract's storage without proper access controls.

## Vulnerability Details
- Description: The "PasswordStore" contract is found to be storing passwords in plain text without any form of hashing or encryption. This lack of security measure makes it vulnerable to unauthorized access and exposes user passwords.
- Affected Contract: PasswordStore
- Function Affected: setPassword(string memory newPassword)

# Proof of Concept:
1. Deploy the contract on the Anvil chain using the make deploy command. The password set using the default script is 'myPassword'.
2. Access the storage layout of the contract using the `forge inspect PasswordStore storage-layout --pretty` command. The password is stored in slot 1.
3. Access the storage slot 1 using `cast storage 0x5fbdb2315678afecb367f032d93f642f64180aa3 1`. The output obtained is `0x6d7950617373776f726400000000000000000000000000000000000000000014`.
4. Decode this using `cast to-ascii 0x6d7950617373776f726400000000000000000000000000000000000000000014`. The password 'myPassword' is clearly visible.

## Impact
- The impact of this vulnerability is significant, as it allows unauthorized parties to retrieve the stored password, which may compromise the security of the data protected by the password. 
- If the contract is used to store sensitive information, this vulnerability could lead to unauthorized access and potential data breaches.

## Tools Used
- No specific tool used. Manual code review was used to identify the vulnerability.

## Recommendations
Due to this, the overall architecture of the protocol needs to be rethought. One could encrypt the password off-chain, and then store the encrypted password on-chain. This would require the user to remember another password off-chain to decrypt the password. However, you'd also likely want to remove the view function as you wouldn't want the user to accidently send a transaction with the password that decrypts the password.
