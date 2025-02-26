# Step Rewards Anti-Cheat System

A Stacks blockchain-based step rewards system with robust anti-cheat mechanisms to ensure fair distribution of rewards.

## Overview

This project implements a step-counting rewards system on the Stacks blockchain using Clarity smart contracts. The system allows users to record their daily steps and earn rewards while preventing fraudulent activity through sophisticated anti-cheat mechanisms.

## Anti-Cheat Mechanisms

### Time-based Cooldowns
- Each user is limited to 10,000 steps per day that count toward rewards
- Steps beyond this limit are recorded but do not generate additional rewards
- Daily limits reset at UTC midnight (based on block height)
- Prevents users from claiming excessive rewards in a short timeframe

### Oracle Cross-checks
- External oracles validate step data for authenticity
- Suspicious patterns are flagged automatically (e.g., 50,000 steps in 1 hour)
- Approved oracles can manually report suspicious activity
- Flagged accounts may be subject to review or temporary suspension
- Hourly step tracking to identify unusual patterns

## Repository Structure
```plaintext
## Smart Contracts

### step-rewards.clar

The main contract that handles step tracking and reward distribution:

- `record-steps`: Records user steps and calculates rewards
- `get-user-stats`: Retrieves total stats for the current user
- `get-daily-stats`: Retrieves daily stats for a specific day
- `flag-user`: Admin function to manually flag suspicious users

### anti-cheat.clar

Implements anti-cheat mechanisms:

- `check-suspicious`: Checks if step activity is suspicious
- `register-oracle`: Registers an approved oracle for cross-checking
- `oracle-report`: Allows oracles to report suspicious activity
- `check-user-flagged`: Checks if a user has been flagged
- `is-oracle-approved`: Verifies if an oracle is approved

## Technical Details

### Reward Calculation
- Each valid step earns 1 token (configurable)
- Daily limit of 10,000 steps that count toward rewards
- Steps are validated before rewards are calculated

### Suspicious Activity Detection
- Hourly step tracking to detect unusual patterns
- Automatic flagging for users recording more than 50,000 steps in an hour
- Oracle-based verification system for additional security

### Data Storage
- User daily steps are stored with user principal and day
- Total user statistics track lifetime steps and rewards
- Flagged users are tracked with reason codes

## Installation

### Prerequisites
- [Clarinet](https://github.com/hirosystems/clarinet) v1.0.0 or higher
- [Node.js](https://nodejs.org/) v14 or higher (for testing)

### Setup
1. Clone this repository:
   ```bash
   git clone https://github.com/yourusername/step-rewards-anti-cheat.git
   cd step-rewards-anti-cheat
```

2. Install dependencies:

```shellscript
npm install
```


3. Initialize Clarinet project (if starting from scratch):

```shellscript
clarinet new step-rewards-anti-cheat
```




## Usage

### Local Development

```shellscript
# Start Clarinet console
clarinet console

# Run tests
clarinet test
```

### Contract Interaction Examples

#### Recording Steps

```plaintext
(contract-call? .step-rewards record-steps u8000)
```

#### Checking User Stats

```plaintext
(contract-call? .step-rewards get-user-stats)
```

#### Registering an Oracle

```plaintext
(contract-call? .anti-cheat register-oracle 'ST1PQHQKV0RJXZFY1DGX8MNSNYVE3VGZJSRTPGZGM true)
```

#### Oracle Reporting Suspicious Activity

```plaintext
(contract-call? .anti-cheat oracle-report 'ST1PQHQKV0RJXZFY1DGX8MNSNYVE3VGZJSRTPGZGM true "Impossible step count detected")
```

## Deployment

### Testnet Deployment

1. Configure your Stacks wallet in Clarinet.toml
2. Deploy to testnet:

```shellscript
clarinet deploy --testnet
```




### Mainnet Deployment

1. Ensure contracts have been thoroughly tested
2. Deploy to mainnet:

```shellscript
clarinet deploy --mainnet
```




## Testing

The project includes comprehensive tests for both contracts:

```shellscript
# Run all tests
clarinet test

# Run specific test
clarinet test tests/step-rewards_test.ts
```

Test coverage includes:

- Normal step recording scenarios
- Edge cases (e.g., exceeding daily limits)
- Anti-cheat detection scenarios
- Oracle reporting functionality
- Administrative functions


## Security Considerations

- Contract owner has administrative privileges
- Oracle approval system prevents unauthorized reporting
- Time-based cooldowns prevent reward farming
- Suspicious activity detection is automated and oracle-verified


## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request


## Acknowledgments

- Stacks Foundation
- Clarity language documentation
- Community contributors
