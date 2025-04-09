<a id="readme-top"></a>
<p align="left"><a href="../README.md">Back to main readme</a></p>

# Solidity language best practices

A clean, consistent structure and naming convention in your Solidity smart contracts can make a huge difference—especially as your codebase grows or becomes collaborative.
When programming in solidity we should follow these guides:

## Table of content

- [Solidity language best practices](#solidity-language-best-practices)
  - [Table of content](#table-of-content)
    - [Layout of Contract:](#layout-of-contract)
    - [Layout of Functions:](#layout-of-functions)
    - [🧾 1. Naming Conventions (Solidity)](#-1-naming-conventions-solidity)
    - [📁 2. Folder \& File Structure](#-2-folder--file-structure)
    - [🧠 3. Design Patterns (for Solidity Structure)](#-3-design-patterns-for-solidity-structure)
      - [Modular Logic (for composability)](#modular-logic-for-composability)
      - [Libraries](#libraries)
    - [🧩 4. Separation of Concerns in Solidity Contracts](#-4-separation-of-concerns-in-solidity-contracts)
      - [Benefits:](#benefits)
    - [✅ 5. Test Naming Conventions and folder structure](#-5-test-naming-conventions-and-folder-structure)
      - [✅ Test File Naming Conventions](#-test-file-naming-conventions)
      - [✅ Test Folder structure](#-test-folder-structure)
    - [🎨 6. Style Tips](#-6-style-tips)
      - [General tips](#general-tips)


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

### 📁 2. Folder & File Structure

Basic folder structure:


```bash
my-hardhat-project/
│
├── contracts/           # ✅ Your Solidity contracts
│   └── MyToken.sol
│
├── test/                # ✅ Your JavaScript or TypeScript tests
│   └── MyToken.test.js  # Test filename should match contract
│
├── scripts/             # ✅ Scripts for deployment or other tasks
│   └── deploy.js
│
├── deployments/         # (Optional) Used with Hardhat Deploy plugin
│
├── hardhat.config.js    # Hardhat config file
├── package.json         # NPM dependencies
└── README.md

```

and here is detailed "contract" folder structure:

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

Folder structure for testing:

```bash
test/
├── tokens/
│   └── ERC20Token.test.js
├── governance/
│   └── Voting.test.js
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

### ✅ 5. Test Naming Conventions and folder structure

#### ✅ Test File Naming Conventions

| Purpose |	Convention | Example |
|---------|------------|---------|
|Test file name |	Match contract name (PascalCase) + .test.js or .test.ts |	MyToken.test.js |
|Grouped contracts (optional)	| Use folders to organize them |	test/tokens/ERC20Token.test.js |


📌 Why? Keeping test filenames aligned with contract names makes it easier to find and manage tests as your project grows.

- Match the contract name in the test file for clarity.

```bash
test/
└── Token.test.js      # Testing Token.sol
└── VotingSystem.test.js
```
- Group related tests using describe() blocks:

```js
describe("Token", function () {
    it("Should deploy correctly", async function () {
        // test logic here
    });
});
```

#### ✅ Test Folder structure

Bonus Tip: Subfolders for larger projects
If your project grows, you can break things down like this:

```bash
contracts/
├── tokens/
│   └── ERC20Token.sol
├── governance/
│   └── Voting.sol
```
And tests:

```bash
test/
├── tokens/
│   └── ERC20Token.test.js
├── governance/
│   └── Voting.test.js
```


### 🎨 6. Style Tips

#### General tips

- Use custom errors instead of string reverts:

```solidity
error NotOwner();

function doSomething() external {
    if (msg.sender != owner) revert NotOwner();
}
```

- Keep functions short & focused (≤ 20 lines ideally).
- Avoid deeply nested if/else. Use require or early returns.
- Use NatSpec format for comments so you can automatically generate documentation from comments, more details [here](NatSpec.md).

<p align="right">(<a href="#readme-top">back to top</a>)</p>

---
