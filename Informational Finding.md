# First Flight #1: PasswordStore - Findings Report 
### Project: First Flight #1

### Dates: Oct 18th, 2023 - Oct 25th, 2023
  
[See more details here](https://www.codehawks.com/contests/clnuo221v0001l50aomgo4nyn)

## The `PasswordStore::getPassword` natspec indicates a parameter that doesn't exist, causing the natspec to be incorrect

**Description:** 

```javascript
    /*
     * @notice This allows only the owner to retrieve the password.
@>   * @param newPassword The new password to set.
     */
    function getPassword() external view returns (string memory) {
```

The natspec for the function `PasswordStore::getPassword` indicates it should have a parameter with the signature `getPassword(string)`. However, the actual function signature is `getPassword()`.

**Impact:** The natspec is incorrect.

**Recommended Mitigation:** Remove the incorrect natspec line.

```diff
-     * @param newPassword The new password to set.
```
