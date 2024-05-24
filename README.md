# Token-Contract-with-Freeze-Functionality
Here's a comprehensive `README.md` file for your GitHub repository that describes your Token Contract with Freeze Functionality project:

```markdown
# Token Contract with Freeze Functionality

## Project Name
Token Contract with Freeze Functionality

## Developer
[Your Name]

## Description
This project implements a token contract on the Stellar blockchain with added functionalities to freeze and unfreeze accounts. Frozen accounts cannot transfer tokens until they are unfrozen. This feature is useful for scenarios where token transactions need to be temporarily halted for specific accounts, providing an extra layer of security and control.

## Vision
The goal is to provide a robust and secure token management system that offers greater control over token transfers, enhancing security and flexibility for token holders.

## Contract Address
`<Your_Stellar_Testnet_Contract_Address>`

## Deployment Details
The contract is deployed on the Stellar testnet. The following steps outline how to interact with the contract using the Stellar SDK and Soroban CLI.

### Features
- **Freeze Account**: Allows freezing of a specific account's tokens.
- **Unfreeze Account**: Allows unfreezing of a specific account's tokens.
- **Transfer**: Enhanced transfer function that checks if accounts are frozen before processing the transaction.

### Usage
1. **Initialize the Contract**:
   ```sh
   soroban invoke --id <contract-id> --fn initialize --source <your-stellar-account>
   ```

2. **Freeze an Account**:
   ```sh
   soroban invoke --id <contract-id> --fn freeze_account --args '<account_address>' --source <your-stellar-account>
   ```

3. **Unfreeze an Account**:
   ```sh
   soroban invoke --id <contract-id> --fn unfreeze_account --args '<account_address>' --source <your-stellar-account>
   ```

4. **Transfer Tokens**:
   ```sh
   soroban invoke --id <contract-id> --fn transfer --args '<from_account>' '<to_account>' <amount> --source <your-stellar-account>
   ```

## Code Structure
The code is structured to ensure readability and maintainability:
- Functions are well-commented to explain their purpose and logic.
- Naming conventions follow Rust's style guidelines: snake_case for variables and functions, PascalCase for types and traits, and SCREAMING_SNAKE_CASE for constants.
- Error handling uses `match` statements and the `?` operator to handle errors gracefully.

### contract.rs
The main contract file that includes:
- **initialize**: Initializes the contract with the necessary data structures.
- **freeze_account**: Freezes the tokens of a specified account.
- **unfreeze_account**: Unfreezes the tokens of a specified account.
- **transfer**: Handles token transfers, with checks to ensure neither the sender's nor the recipient's tokens are frozen.

### Example Test
A basic test to verify the freeze and transfer functionalities:
```rust
#[cfg(test)]
mod tests {
    use super::*;
    use soroban_sdk::{testutils::EnvVal, Address, Env};

    #[test]
    fn test_freeze_and_transfer() {
        let env = Env::default();
        let contract_id = env.register_contract(None, TokenContract);
        let client = TokenContractClient::new(&env, &contract_id);

        let account1 = Address::random(&env);
        let account2 = Address::random(&env);

        client.initialize(&env);

        client.freeze_account(&env, &account1);
        assert!(env.storage().get::<Map<Address, bool>>(FROZEN_ACCOUNTS).unwrap().get(&account1).unwrap());

        client.unfreeze_account(&env, &account1);
        assert!(!env.storage().get::<Map<Address, bool>>(FROZEN_ACCOUNTS).unwrap().get(&account1).unwrap());

        client.transfer(&env, &account1, &account2, 100); // This should work if accounts are not frozen
    }
}
```

## Contribution
Contributions are welcome! Please fork the repository and submit a pull request with your changes. Ensure your code follows the project's coding standards and includes relevant tests.

## License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Contact
For any questions or inquiries, please contact [Your Name] at [your.email@example.com].
```

Replace the placeholders such as `[Your Name]`, `<Your_Stellar_Testnet_Contract_Address>`, and `<your-stellar-account>` with your actual information. This `README.md` file provides a comprehensive overview of your project, its features, and how to use it.
