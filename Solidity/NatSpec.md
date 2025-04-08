
<a id="readme-top"></a>
<p align="left"><a href="./README.md">Back to main readme</a></p>

## NatSpec comments

Solidity smart contracts and functions in a human- and machine-readable format. Think of it like Javadoc or Doxygen, but for Solidity. It’s especially useful for:
- Developers reading your code
- Tools like Etherscan or Remix
- Users interacting with smart contracts through a UI

---

### Basic format

NatSpec comments go above the item you're documenting using /// for single-line or /** */ for multi-line comments.
```solidity
/// @title A simple token contract
/// @author Alice
/// @notice This contract is just for demonstration
/// @dev All function calls are currently implemented without side effects
contract MyToken {
    /// @notice Transfers tokens to a given address
    /// @dev This function uses a simple balance mapping
    /// @param to The address to transfer to
    /// @param amount The number of tokens to send
    /// @return success Whether the transfer succeeded
    function transfer(address to, uint256 amount) public returns (bool success) {
        // implementation
    }
}
```
---

### 🏷️ Common NatSpec Tags

| Tag           | Used In             | Description                                           |
|---------------|---------------------|-------------------------------------------------------|
| `@title`      | Contract/interface  | A brief title for the contract or interface           |
| `@autho`r     | Contract/interface  | Author of the code                                    |
| `@notice`     | Contract/function   | A comment meant for end users                         |
| `@dev`        | Contract/function   | Developer-focused comment                             |
| `@param`      | Function            | Description of a function parameter                   |
| `@return`     | Function            | Description of a return variable                      |
| `@inheritdoc` | Contract/function   | Inherit NatSpec comments from parent                  |
| `@custom:`    | Contract/function   | Create your own tags (e.g. `@custom:security`)        |


<p align="right">(<a href="#readme-top">back to top</a>)</p>

---

###  Example in Context

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

/// @title Voting System
/// @author You
/// @notice You can use this contract to vote on proposals
/// @dev Use with care — no access control in this demo
contract Ballot {
    /// @notice Maps addresses to vote counts
    mapping(address => uint256) public votes;

    /// @notice Cast a vote
    /// @dev Only increments, does not store proposal
    /// @param amount Number of votes to cast
    function vote(uint256 amount) public {
        votes[msg.sender] += amount;
    }

    /// @notice Returns the votes for an address
    /// @param voter The address to query
    /// @return count Number of votes
    function getVotes(address voter) public view returns (uint256 count) {
        return votes[voter];
    }
}
```

<p align="right">(<a href="#readme-top">back to top</a>)</p>

---
### 🧪 Bonus: Tools That Use NatSpec

- Remix IDE: Shows @notice and @param in its UI.
- Etherscan: Displays function documentation if verified.
- DApps: Can use NatSpec to auto-generate UI or tooltips for end users.


Use NatSpec comments for all public functions & contracts.

You can find more information about NatSpec Format [here](https://docs.soliditylang.org/en/latest/natspec-format.html).

<p align="right">(<a href="#readme-top">back to top</a>)</p>

---
### Documentation generated from NatSpec

Generating documentation from NatSpec comments can seriously level up the clarity of your contracts—especially for team projects or when building public-facing dApps. Here's how you can do it in different environments:

#### 📘 1. Using the Solidity Compiler Directly (solc)

The Solidity compiler (solc) can extract NatSpec documentation as JSON output. This is the most direct and customizable way.

🛠️ How to Use:
```bash
Copy
Edit
solc --userdoc --devdoc --pretty-json MyContract.sol
```
- --userdoc: extracts @notice and user-facing info
- --devdoc: extracts @dev, @param, @return, etc.
- --pretty-json: makes the JSON easier to read

**📄 Output:**
This command will give you two JSON documents:
- UserDoc: Designed for users, UIs, wallets
- DevDoc: Technical details for developers

You can then use that JSON to generate custom documentation in a website or tool (e.g., markdown conversion).

<p align="right">(<a href="#readme-top">back to top</a>)</p>

---

#### 🔧 2. Using Hardhat Plugin: hardhat-docgen
If you're using **Hardhat**, there's a handy plugin called hardhat-docgen that turns NatSpec comments into **beautiful markdown documentation** automatically.

**🔌 Installation:**
```bash
npm install --save-dev hardhat-docgen
```

**⚙️ Add to hardhat.config.js:**
```js
require("hardhat-docgen");

module.exports = {
  // ... existing config
  docgen: {
    path: './docs', // output directory
    clear: true,    // clear previous docs
    runOnCompile: false,
  },
};
```

**🏃‍♂️ Run:**
```bash
npx hardhat docgen
```

Then check the ==docs/== folder — you’ll see markdown files per contract, including descriptions from NatSpec.

<p align="right">(<a href="#readme-top">back to top</a>)</p>
--- 

#### 🧰 3. Using Foundry: ==forge== doc (experimental)

If you're using Foundry, documentation support is being added under the forge doc command (though it's still in early stages as of mid-2024).

**Example:**
```bash
forge doc --contract MyContract
```

It reads NatSpec and creates markdown or JSON-style docs. You'll need to keep your Forge version up to date for full support.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

---
#### 🌐 4. Build Your Own Docs Site (Advanced)

If you're building a full documentation site:
- Extract the JSON from solc
- Use a static site generator (like Docusaurus, VuePress, or MkDocs)
- Parse UserDoc/DevDoc and convert into HTML/Markdown


You could even write a script in Node.js or Python to automate this!


<p align="right">(<a href="#readme-top">back to top</a>)</p>
---