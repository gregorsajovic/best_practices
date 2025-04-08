# Solidity language best practices

A clean, consistent structure and naming convention in your Solidity smart contracts can make a huge difference—especially as your codebase grows or becomes collaborative.
When programming in solidity we should follow these guides:

## Table of content

- [Solidity language best practices](#solidity-language-best-practices)
  - [Table of content](#table-of-content)
    - [Layout of Contract:](#layout-of-contract)
    - [Layout of Functions:](#layout-of-functions)
    - [🧾 1. Naming Conventions (Solidity)](#-1-naming-conventions-solidity)
    - [📁 2. Folder \& File Structure (in contracts/)](#-2-folder--file-structure-in-contracts)
    - [🧠 3. Design Patterns (for Solidity Structure)](#-3-design-patterns-for-solidity-structure)
      - [Modular Logic (for composability)](#modular-logic-for-composability)
      - [Libraries](#libraries)
    - [🧩 4. Separation of Concerns in Solidity Contracts](#-4-separation-of-concerns-in-solidity-contracts)
      - [Benefits:](#benefits)
    - [🎨 5. Style Tips](#-5-style-tips)


### Layout of Contract:
- version
- imports
- errors
- interfaces, libraries, contracts
- Type declarations
- State variables
- Events
- Modifiers
- Functions

<p align="right">(<a href="#readme-top">back to top</a>)</p>

---

### Layout of Functions:
- constructor
- receive function (if exists)
- fallback function (if exists)
- external
- public
- internal
- private
- internal & private view & pure functions
- external & public view & pure functions


And [here](./ExampleContract.sol) is the example of solidity contract how it should be structured.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

---

### 🧾 1. Naming Conventions (Solidity)
- Contracts
    - PascalCase (MyContract)
    - Singular nouns
    - Factory contracts: MyContractFactory
    - Upgradeable contracts: MyContractV1

Examples:
```solidity
contract TokenVault {}
contract StakingPool {}
contract NFTMarketplace {}
```

- Files
  - Match contract name exactly: TokenVault.sol → contract TokenVault
  - Interfaces: ITokenVault.sol
  - Libraries: SafeTransferLib.sol, MathLib.sol

- Functions
  - camelCase
  - Keep them descriptive
  - For internal/private functions, prefix with _ (optional, but common)

```solidity
function transferTokens(address recipient, uint256 amount) public {}
function _calculateReward(address user) internal view returns (uint256) {}
```
- Variables
  - camelCase for most
  - Constants: ALL_CAPS_WITH_UNDERSCORES

```solidity
uint256 public rewardRate;
uint256 private totalStaked;
address internal owner;

uint256 public constant MAX_SUPPLY = 1_000_000 ether;
```

- Events
  - PascalCase

```solidity
event TokenMinted(address indexed to, uint256 amount);
event StakeWithdrawn(address indexed user, uint256 amount);
```

<p align="right">(<a href="#readme-top">back to top</a>)</p>

---

### 📁 2. Folder & File Structure (in contracts/)

```bash
contracts/
├── Token/
│   ├── Token.sol              # Main token contract
│   ├── IToken.sol             # Interface
│   └── TokenStorage.sol       # Optional: Storage separation
│
├── libraries/
│   ├── MathLib.sol
│   └── SafeTransferLib.sol
│
├── utils/
│   └── Ownable.sol            # Custom reusable modules
│
├── Vault/
│   ├── TokenVault.sol
│   └── ITokenVault.sol
```
Separate by domain logic, not just type (e.g., don’t put all interfaces/, contracts/, lib/ into top level—nest when needed).

<p align="right">(<a href="#readme-top">back to top</a>)</p>

---

### 🧠 3. Design Patterns (for Solidity Structure)
- Ownable / Access Control
  - Always secure admin actions with roles.

```solidity
modifier onlyOwner() {
    require(msg.sender == owner, "Not owner");
    _;
}
```
Or better yet, use OpenZeppelin's Ownable.sol or AccessControl.sol.

--- 

#### Modular Logic (for composability)

Split big contracts:
```solidity
contract Vault {
    using SafeTransferLib for ERC20;
    // main vault logic
}
```

#### Libraries

Put stateless logic in libraries:
```solidity
library MathLib {
    function average(uint256 a, uint256 b) internal pure returns (uint256) {
        return (a + b) / 2;
    }
}
```

<p align="right">(<a href="#readme-top">back to top</a>)</p>

---

### 🧩 4. Separation of Concerns in Solidity Contracts

When structuring your smart contracts, aim for modularity and clarity by separating responsibilities into distinct files or layers. Here's a simple breakdown:

| Layer            | Responsibility                  | Example                    |
|------------------|----------------------------------|----------------------------|
| **Core Logic**   | Handles main business logic      | `StakingPool.sol`          |
| **Storage**      | Manages storage layout (esp. for upgradeable contracts) | `StakingStorage.sol` |
| **Interfaces**   | Defines external-facing contract APIs | `IStakingPool.sol`     |
| **Libraries**    | Shared utility logic and math    | `StakingMathLib.sol`       |
| **Access Control** | Manages roles and permissions  | `Ownable.sol`, `AccessControl.sol` |

#### Benefits:
- Easier to test and audit
- Reusable components
- Cleaner contract upgrades
- Reduced gas cost by reusing libraries


<p align="right">(<a href="#readme-top">back to top</a>)</p>

---

### 🎨 5. Style Tips

- Use NatSpec comments for all public functions & contracts.
```solidity
/// @notice Mints new tokens to a specified address.
/// @param to The recipient address.
/// @param amount Amount of tokens to mint.
function mint(address to, uint256 amount) external onlyOwner;
```

- Use custom errors instead of string reverts:

```solidity
error NotOwner();

function doSomething() external {
    if (msg.sender != owner) revert NotOwner();
}
```

- Keep functions short & focused (≤ 20 lines ideally).

- Avoid deeply nested if/else. Use require or early returns.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

---
