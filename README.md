# NFT-based-Virtual-Real-Estate
Create a WEB3-based virtual world where users can buy, sell, and develop unique virtual properties using NFTs.
import hashlib
import time

class Block:
    def __init__(self, index, transactions, timestamp, previous_hash):
        self.index = index
        self.transactions = transactions
        self.timestamp = timestamp
        self.previous_hash = previous_hash
        self.hash = self.compute_hash()
    
    def compute_hash(self):
        block_string = f"{self.index}{self.transactions}{self.timestamp}{self.previous_hash}"
        return hashlib.sha256(block_string.encode()).hexdigest()

class Blockchain:
    def __init__(self):
        self.chain = []
        self.create_genesis_block()
        self.transactions = []
    
    def create_genesis_block(self):
        genesis_block = Block(0, [], time.time(), "0")
        self.chain.append(genesis_block)
    
    def add_block(self, transactions):
        previous_hash = self.chain[-1].hash
        new_block = Block(len(self.chain), transactions, time.time(), previous_hash)
        self.chain.append(new_block)
    
    def display_chain(self):
        for block in self.chain:
            print(vars(block))

class VirtualPropertyNFT:
    def __init__(self, property_id, owner, details):
        self.property_id = property_id
        self.owner = owner
        self.details = details  # Details can include size, location in the virtual world, etc.
        self.is_for_sale = False
        self.price = 0
    
    def put_for_sale(self, price):
        self.is_for_sale = True
        self.price = price
    
    def buy_property(self, new_owner):
        if self.is_for_sale:
            self.owner = new_owner
            self.is_for_sale = False
            self.price = 0
            return True
        else:
            return False

# Example usage
virtual_world_blockchain = Blockchain()

# Creating some virtual properties as NFTs
property1 = VirtualPropertyNFT(1, "UserA", {"size": "100 sqm", "location": "Zone 1"})
property2 = VirtualPropertyNFT(2, "UserB", {"size": "200 sqm", "location": "Zone 2"})

# UserA decides to sell property1
property1.put_for_sale(100)  # Price is an arbitrary number for this demo

# Record the sale transaction on our simplified blockchain
virtual_world_blockchain.add_block([{"property_id": property1.property_id, "seller": property1.owner, "buyer": "UserC", "price": property1.price}])

# UserC buys property1
property1.buy_property("UserC")

# Display the updated blockchain
virtual_world_blockchain.display_chain()

# Note: This code is a simplified demonstration and does not interact with an actual blockchain or handle secure transactions.
