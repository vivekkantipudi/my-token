## Building and Testing a Simple ERC-20 Token in Remix

This guide details the process of compiling, deploying, and testing a basic ERC-20-like token smart contract (`MyToken.sol`) using the **Remix IDE** (Integrated Development Environment).

---

### 1. Compilation

The first step is to compile the Solidity contract code to generate the bytecode necessary for deployment.

* **Action:** Navigate to the **Solidity Compiler** tab (the icon with the single Solidity logo).
* **Settings:** Ensure the correct **Compiler version** (e.g., `0.8.30+commit...` or a compatible version) is selected. The `Auto compile` option is enabled in the provided screenshot, which automatically compiles the contract upon changes.
* **Result:** The contract `MyToken.sol` is successfully compiled.


---

### 2. Deployment

After successful compilation, the contract is deployed to a simulated or real blockchain environment.

* **Action:** Navigate to the **Deploy & Run Transactions** tab (the icon with the Ethereum logo).
* **Environment:** The screenshot shows **Remix VM (Cancun)** is selected, which is a simulated local environment within the browser.
* **Account:** The default account (`0x5B38...`) is selected for deployment.
* **Contract:** The correct contract, `MyToken - contracts/MyToken.sol`, is selected.
* **Constructor Arguments:** The contract's constructor requires an `_initialSupply` argument. The value `1000000000000000000000` is provided for the initial token supply.
* **Transaction:** Clicking **transact** (or **Deploy**) executes the constructor and deploys the contract.
* **Result:** A successful deployment transaction is recorded: `[vm] from 0x5b3...eddC4 to: MyToken.constructor(uint256) 0x91...39138 value: 0 wei data: 0x608...00000 logs: 0 hash: 0xa9f...9a949`.


---

### 3. Checking Token Information

Once deployed, the public view functions can be called to check the token's basic properties.

* **Action:** In the deployed contract section, call the public view functions.
    * **`decimals`**: Returns **18**.
    * **`symbol`**: Returns **MTK**.
    * **`name`**: Returns **MyToken**.
    * **`getTotalSupply`**: Returns **1000000000000000000000**, confirming the initial supply.
    * **`getTokenInfo`**: Returns all token details in a single call.
* **Result:** All token properties are correctly returned, confirming the initial state.


---

### 4. Testing `transfer()`

The `transfer` function moves tokens directly from the sender's account to a specified recipient.

#### A. Successful Transfer

* **Action:**
    * **`to`**: Input a recipient address (e.g., `0xAB44B...696A`).
    * **`_value`**: Input the amount to transfer (e.g., `100000000000000000000`).
    * Click **transact**.
* **Result:** A successful transaction is recorded, emitting a **`Transfer` event**.
    * The transaction log shows: `[vm] from 0x5b3...eddC4 to: MyToken.transfer(address,uint256) 0x91...39138 value: 0 wei data: 0x98...00000 logs: 1 hash: 0x23b...57f9e`.


#### B. Transfer to Zero Address Error

The contract should prevent sending tokens to the zero address (`0x0...0`).

* **Action:**
    * **`to`**: Input the zero address (`0x0000000000000000000000000000000000000000`).
    * **`_value`**: Input an amount (e.g., `10`).
    * Click **transact**.
* **Result:** The transaction **reverts** with the error: **"Cannot transfer to zero address"**, which is enforced by the `require` statement in the `transfer` function.


---

### 5. Testing `approve()` and `transferFrom()`

These functions enable a spender to transfer tokens on behalf of the token owner, provided an **allowance** is set.

#### A. Setting Allowance (`approve`)

* **Action:**
    * **`spender`**: Input the address to be granted spending permission (e.g., `0xAB44B...696A`).
    * **`_value`**: Input the maximum amount the spender is allowed to spend (e.g., `50000000000000000000`).
    * Click **transact** under the `approve` function.
* **Result:** A successful transaction is recorded, emitting an **`Approval` event**.

 (Shows the successful `approve` transaction and the resulting `Approval` event in the transaction log).

#### B. Attempting `transferFrom` with Insufficient Allowance

The `transferFrom` function attempts to move tokens, which requires a sufficient **allowance** to be set for the spender by the owner.

* **Action:**
    * Switch to a **different account** (the *spender* account, `0xDB38...8103`).
    * Call **`transferFrom`**.
        * **`_from`**: The **owner** account (`0x5B38...eddC4`).
        * **`_to`**: The final recipient (e.g., `0xDB38...8103`).
        * **`_value`**: The amount to transfer (e.g., `5000` or a very small amount).
    * Click **transact**.
* **Result:** The screenshot shows a transaction attempting to transfer tokens that **reverts** with the error: **`"Insufficient allowance"`**. This likely occurred because the selected sender account **(the spender)** was attempting to move tokens from the owner without having a large enough allowance set, or the wrong account was used as the caller.


*(Note: The provided image shows the transaction from account `0xDB38...8103` reverting because of "Insufficient allowance," confirming the allowance check logic is functional.)*
