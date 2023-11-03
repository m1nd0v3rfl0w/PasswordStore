# First Flight #1: PasswordStore - Findings Report 
### Project: First Flight #1

### Dates: Oct 18th, 2023 - Oct 25th, 2023
  
[See more details here](https://www.codehawks.com/contests/clnuo221v0001l50aomgo4nyn)

## Missing Access Control in setPassword Function            

### Relevant GitHub Links
	
https://github.com/Cyfrin/2023-10-PasswordStore/blob/856ed94bfcf1031bf9d13514cb21b591d88ed323/src/PasswordStore.sol#L26-L29

## Summary
This report identifies a vulnerability in the setPassword function of the "PasswordStore" smart contract. The issue arises from the absence of access control checks, allowing anyone to set the password without proper authorization.

## Vulnerability Details
- Contract Name: PasswordStore
- Function Affected: setPassword(string memory newPassword)
- Description: The setPassword function does not include a require statement or access control check, enabling unauthorized users to change the stored password. This oversight could potentially compromise the integrity of the password management system.
``` 
function setPassword(string memory newPassword) external --> No access control implemented 
{ 
   s_password = newPassword; 
   emit SetNetPassword();
} 
```

## Impact
- Unauthorized users can change the stored password, potentially compromising the security of the data.

## Tools Used
- No specific tool used. Vulnerability identified using manual code review.

## Recommendations
- Add an access control modifier (e.g., "onlyOwner") to the setPassword function, ensuring that only the contract owner (the address that deployed the contract) can change the password.
```
function setPassword(string memory newPassword) external { 
        if (msg.sender != s_owner) {
            revert PasswordStore__NotOwner();
        }
        s_password = newPassword; 
        emit SetNetPassword();
    }
```
