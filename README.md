import hashlib
import time

class Block:
    def __init__(self, index, previous_hash, timestamp, data, hash):
        self.index = index
        self.previous_hash = previous_hash
        self.timestamp = timestamp
        self.data = data
        self.hash = hash

def calculate_hash(index, previous_hash, timestamp, data):
    return hashlib.sha256(f"{index}{previous_hash}{timestamp}{data}".encode()).hexdigest()

def create_genesis_block():
    return Block(0, "0", time.time(), "Genesis Block", calculate_hash(0, "0", time.time(), "Genesis Block"))

def create_new_block(previous_block, data):
    index = previous_block.index + 1
    timestamp = time.time()
    hash = calculate_hash(index, previous_block.hash, timestamp, data)
    return Block(index, previous_block.hash, timestamp, data, hash)

# Initialize the blockchain with the genesis block
blockchain = [create_genesis_block()]
previous_block = blockchain[0]

# Provided dataset
transactions = [
    {"TransactionNo": 581482, "Date": "12-09-2019", "ProductNo": 22485, "ProductName": "Set Of 2 Wooden Market Crates", "Price": 21.47, "Quantity": 12, "CustomerNo": 17490, "Country": "United Kingdom"},
    {"TransactionNo": 581475, "Date": "12-09-2019", "ProductNo": 22596, "ProductName": "Christmas Star Wish List Chalkboard", "Price": 10.65, "Quantity": 36, "CustomerNo": 13069, "Country": "United Kingdom"},
    # Add more transactions here as needed
]

# Add transactions to the blockchain
for transaction in transactions:
    transaction_data = f"TransactionNo: {transaction['TransactionNo']}, Date: {transaction['Date']}, ProductNo: {transaction['ProductNo']}, ProductName: {transaction['ProductName']}, Price: {transaction['Price']}, Quantity: {transaction['Quantity']}, CustomerNo: {transaction['CustomerNo']}, Country: {transaction['Country']}"
    new_block = create_new_block(previous_block, transaction_data)
    blockchain.append(new_block)
    previous_block = new_block

# Print the blockchain
for block in blockchain:
    print(f"Index: {block.index}")
    print(f"Previous Hash: {block.previous_hash}")
    print(f"Timestamp: {block.timestamp}")
    print(f"Data: {block.data}")
    print(f"Hash: {block.hash}")
    print("\n" + "="*50 + "\n")
