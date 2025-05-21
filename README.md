from web3 import Web3
import time

# --- User Configuration ---
RPC_URL = "https://mainnet.infura.io/v3/YOUR_INFURA_KEY"
TOKEN_ADDRESS = "0xYourTokenAddress"  # ERC-20 token contract address
MAIN_ACCOUNT = "0xYourMainAccount"
WALLETS = [
    {"address": "0xWallet1", "private_key": "0xPrivateKey1"},
    {"address": "0xWallet2", "private_key": "0xPrivateKey2"},
    # Add more wallets as needed
]

# --- ERC-20 ABI fragment for balanceOf and transfer ---
ERC20_ABI = [
    {"constant":True,"inputs":[{"name":"owner","type":"address"}],"name":"balanceOf","outputs":[{"name":"balance","type":"uint256"}],"type":"function"},
    {"constant":False,"inputs":[{"name":"to","type":"address"},{"name":"value","type":"uint256"}],"name":"transfer","outputs":[{"name":"","type":"bool"}],"type":"function"},
    {"constant":True,"inputs":[],"name":"decimals","outputs":[{"name":"","type":"uint8"}],"type":"function"},
]

w3 = Web3(Web3.HTTPProvider(RPC_URL))
token = w3.eth.contract(address=TOKEN_ADDRESS, abi=ERC20_ABI)

def pullback(wallet):
    address = wallet["address"]
    private_key = wallet["private_key"]
    balance = token.functions.balanceOf(address).call()
    decimals = token.functions.decimals().call()
    if balance > 0:
        print(f"Transferring {balance/(10**decimals)} tokens from {address} to {MAIN_ACCOUNT}")
        nonce = w3.eth.get_transaction_count(address)
        txn = token.functions.transfer(
            MAIN_ACCOUNT,
            balance
        ).build_transaction({
            'from': address,
            'nonce': nonce,
            'gas': 80000,
            'gasPrice': w3.toWei('5', 'gwei')
        })
        signed_txn = w3.eth.account.sign_transaction(txn, private_key=private_key)
        tx_hash = w3.eth.send_raw_transaction(signed_txn.rawTransaction)
        print(f"Sent! Tx hash: {tx_hash.hex()}")
    else:
        print(f"No tokens to transfer for {address}")

def main():
    while True:
        for wallet in WALLETS:
            pullback(wallet)
        # Run every 10 minutes
        time.sleep(600)

if __name__ == "__main__":
    main()- 👋 Hi, I’m @JZ84-huck
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...

<!---
JZ84-huck/JZ84-huck is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
CD}:/ {{twist coding ($$%<^>)}}
<\\„Cod$$> 
{{control_<>_mord}}
