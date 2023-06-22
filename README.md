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
    value = str(index) + str(previous_hash) + str(timestamp) + str(data)
    return hashlib.sha256(value.encode('utf-8')).hexdigest()


def create_genesis_block():
    return Block(0, "0", time.time(), "Genesis Block", calculate_hash(0, "0", time.time(), "Genesis Block"))


def create_new_block(previous_block, data):
    index = previous_block.index + 1
    timestamp = time.time()
    hash = calculate_hash(index, previous_block.hash, timestamp, data)
    return Block(index, previous_block.hash, timestamp, data, hash)
import hashlib
import time

class Transaction:
    def __init__(self, sender, receiver, amount):
        self.sender = sender
        self.receiver = receiver
        self.amount = amount

class Block:
    def __init__(self, index, previous_hash, timestamp, transactions, proof_of_work, hash):
        self.index = index
        self.previous_hash = previous_hash
        self.timestamp = timestamp
        self.transactions = transactions
        self.proof_of_work = proof_of_work
        self.hash = hash

def calculate_hash(index, previous_hash, timestamp, transactions, proof_of_work):
    value = str(index) + str(previous_hash) + str(timestamp) + str(transactions) + str(proof_of_work)
    return hashlib.sha256(value.encode('utf-8')).hexdigest()

def create_genesis_block():
    transactions = []
    return Block(0, "0", time.time(), transactions, 0, calculate_hash(0, "0", time.time(), transactions, 0))

def proof_of_work(last_proof):
    incrementor = last_proof + 1
    while not (incrementor % 9 == 0 and incrementor % last_proof == 0):
        incrementor += 1
    return incrementor

def create_new_block(previous_block, transactions):
    index = previous_block.index + 1
    timestamp = time.time()
    proof = proof_of_work(previous_block.proof_of_work)
    hash = calculate_hash(index, previous_block.hash, timestamp, transactions, proof)
    return Block(index, previous_block.hash, timestamp, transactions, proof, hash)
