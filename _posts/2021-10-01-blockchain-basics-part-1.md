---
layout: post
title:  "Blockchain Data Structures: Arrays & Trees"
categories: blockchain, data structures, algorithms
date:   2021-10-01 14:17:15 -0600
description: "A simple explanation of a blockchain with code examples"

---

Conceptually, a blockchain is an array of blocks of data that are cryptographically back-linked so that each block contains a reference to the previous block.  

Each block after the genesis block is an object with an index, a hash, data, a timestamp, and the hash of the previous block. To create the hash, the `index`, `prevHash`, `data`, and `timestamp` properties are passed through a SHA-256 hash function. SHA-256 is a secure hashing algorithm that outputs a 256 bit value for any given input.

```jsx
let crypto = require("crypto");

let blockchain = { 
	blocks: []
}

// Genesis Block
blockchain.blocks.push({
	index: 0,
	hash: "000000",
	data: "",
	timestamp: Date.now()
});

function createBlock(data) {
	let bl = {
		index: Blockchain.blocks.length,
		prevHash: Blockchain.blocks[Blockchain.blocks.length-1].hash,
		data: data,
		timestamp: Date.now(),
	};

	bl.hash = blockHash(bl);

	return bl;
}

function blockHash(bl) {
	return crypto.createHash("sha256").update(
		`${bl.index};${bl.prevHash};${JSON.stringify(bl.data)};${bl.timestamp}`
	).digest("hex");
}
```

Any change to the input of the SHA-256 function results in a different output, so if a malicious party tries to tamper with the data in a block it would change the hash. Then, the `prevHash` property of the next block would no longer match the `hash` of the block that was changed. For this reason it is important to always verify the chain by verifying each block for the following: 

- `index` is a non-negative integer
- `prevHash` exists and is equal to the `hash` of the previous block
- `data` and `timestamp` are non-empty
- `hash` is the SHA-256 hash of the block's `index` , `data` , `prevHash` and `timestamp`  properties
    - except the `hash` of genesis block which should be `000000` in this example

```jsx
function verifyBlock(bl) {
	if (bl.data == null) return false;
	if (bl.index === 0) {
		if (bl.hash !== "000000") return false;
	}
	else {
		if (!bl.prevHash) return false;
		if (!(
			Number.isInteger(bl.index) &&
			bl.index > 0
		)) {
			return false;
		}
		if (bl.hash !== blockHash(bl)) return false;
	}
	return true;
}

async function verifyChain(chain) {
	let prevHash;
	for (let bl of chain.blocks) {
		if (prevHash && bl.prevHash !== prevHash) return false;
		if (!(await verifyBlock(bl))) return false;
		prevHash = bl.hash;
	}

	return true;
}
```

The `data` in a block is actually a Merkle tree of several transactions. A Merkle tree, or binary hash tree, is a data structure that allows for efficient data storage to minimize computing resource consumption and verify data integrity. When transactions are approved by a blockchain network to be added to a block, the transactions are paired and hashed repeatedly until there is one single hash representing all of the transactions. This final hash is called the Merkle root.

![Merkle Tree Hash](/assets/images/merkle-tree-hash.png)  



## Resources
[Building a Blockchain](https://observablehq.com/@consensys-academy/building-a-blockchain)  
[SHA-256 Online](https://emn178.github.io/online-tools/sha256.html)  
[Merkle Trees](https://selfkey.org/what-is-a-merkle-tree-and-how-does-it-affect-blockchain-technology/)  
