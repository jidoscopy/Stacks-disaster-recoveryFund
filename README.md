# Stacks Disaster-Relief Donation Smart Contract

## Overview
This smart contract manages a disaster-relief donation system with recipient allocations and admin controls. It allows users to donate funds, administrators to manage recipients and contract settings, and recipients to withdraw allocated funds.

## Features
- **Donations**: Users can donate STX tokens within specified limits.
- **Recipient Management**: Admins can add, update, and remove recipients.
- **Withdrawals**: Recipients can withdraw allocated funds after a cooldown period.
- **Admin Controls**: Admins can set donation limits, pause the contract, change the admin, and shut down the contract in emergencies.
- **Refund Requests**: Donors can request a refund within 24 hours.
- **Audit Logs**: Basic logging of contract actions for transparency.

---

## Contract Variables

### Constants
- `ERR_UNAUTHORIZED`: Error code for unauthorized actions.
- `ERR_INVALID_AMOUNT`: Error code for invalid amounts.
- `ERR_INSUFFICIENT_FUNDS`: Error code for insufficient funds.
- `ERR_RECIPIENT_NOT_FOUND`: Error code for missing recipient.
- `ERR_RECIPIENT_EXISTS`: Error code for duplicate recipient.
- `ERR_NOT_CONFIRMED`: Error code for unconfirmed actions.
- `ERR_UNCHANGED_STATE`: Error code for unchanged state updates.
- `withdrawal-cooldown`: 24-hour cooldown period for withdrawals.

### Data Variables
- `total-funds`: Total STX funds held by the contract.
- `admin`: Address of the contract administrator.
- `min-donation`: Minimum donation amount.
- `max-donation`: Maximum donation amount.
- `paused`: Contract operational status.

### Data Maps
- `donations`: Tracks donations by user.
- `recipients`: Tracks allocated funds for recipients.
- `last-withdrawal`: Tracks last withdrawal timestamp for recipients.

---

## Public Functions

### Donation Functions
#### `donate(amount uint)`
- Allows users to donate STX tokens.
- Donations must be within the minimum and maximum limits.
- Fails if the contract is paused.

### Recipient Management
#### `add-recipient(recipient principal, allocation uint)`
- Adds a new recipient with an initial fund allocation.
- Only the admin can call this function.

#### `update-recipient-allocation(recipient principal, new-allocation uint)`
- Updates the fund allocation for a recipient.
- Only the admin can call this function.

#### `remove-recipient(recipient principal)`
- Removes a recipient from the system.
- Only the admin can call this function.

#### `confirm-remove-recipient(recipient principal, confirm bool)`
- Confirms the removal of a recipient.
- Ensures an explicit confirmation before deletion.
- Only the admin can call this function.

### Withdrawal Functions
#### `withdraw(amount uint)`
- Allows recipients to withdraw allocated funds.
- Withdrawals are subject to a cooldown period of 24 hours.
- Fails if the contract is paused.

### Admin Functions
#### `set-admin(new-admin principal)`
- Transfers admin rights to another user.
- Only the current admin can call this function.

#### `set-donation-limits(new-min uint, new-max uint)`
- Sets new minimum and maximum donation limits.
- Only the admin can call this function.

#### `set-paused(new-paused-state bool)`
- Pauses or unpauses the contract.
- Only the admin can call this function.

#### `emergency-shutdown()`
- Shuts down the contract in case of emergency.
- Only the admin can call this function.

### Refund Functions
#### `request-refund(amount uint)`
- Allows donors to request a refund within 24 hours of donation.
- Fails if the request is beyond the refund period.

### Audit Functions
#### `log-audit(action-type (string-ascii 32), actor principal)`
- Logs contract actions for transparency.

---

## Read-Only Functions

### `get-donation(user principal)`
- Returns the total donation amount of a user.

### `get-total-funds()`
- Returns the total funds held in the contract.

### `is-paused()`
- Returns the contract's paused status.

### `get-recipient-allocation(recipient principal)`
- Returns the allocated funds for a recipient.

### `get-admin()`
- Returns the current admin address.

### `get-user-history(user principal)`
- Returns the donation and withdrawal records of a user.

---

## Error Handling
- Errors are represented as error codes (`err uX`), where `X` corresponds to a specific failure condition.
- The contract checks for unauthorized access, invalid inputs, insufficient funds, and other conditions before processing any function.

---

## Deployment & Usage
### Steps to Deploy
1. Compile the contract using Clarity tools.
2. Deploy the contract on the Stacks blockchain.
3. Assign an admin account to manage operations.

### Usage Guide
- **Donors** can call `donate(amount)` to contribute funds.
- **Admins** can manage recipients, pause the contract, and configure settings.
- **Recipients** can call `withdraw(amount)` to claim allocated funds.
- **Users** can request refunds within 24 hours.

---

## Security Considerations
- **Admin Privileges**: The admin has powerful controls, including shutting down the contract. It is recommended to use a multisig wallet.
- **Cooldown Mechanism**: Withdrawals and refunds have a cooldown period to prevent abuse.
- **Paused Mode**: The contract can be paused in case of an emergency.
- **Audit Logging**: Actions are logged to improve transparency and tracking.

---

## License
This contract is open-source and licensed under the MIT License.

