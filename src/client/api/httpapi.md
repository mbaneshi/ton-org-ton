### File Description
This TypeScript file appears to define a client for interacting with a specific API related to the TON (Telegram Open Network) blockchain. The API provides functionalities such as retrieving information about addresses, transactions, shards, and more. The file also contains TypeScript types for various response structures and utility classes for caching and handling HTTP requests.

### Symbols List

#### Interfaces and Types
1. `HTTPTransaction`: Type for an array of transactions.
2. `HTTPMessage`: Type for a message.
3. `HttpApiParameters`: Interface for parameters that can be passed to the `HttpApi` constructor.

#### Zod Schemas
4. `blockIdExt`: Zod schema for a block ID with extended information.
5. `addressInformation`: Zod schema for information about an address.
6. `bocResponse`: Zod schema for a BOC (Bag of Cells) response.
7. `feeResponse`: Zod schema for a fees response.
8. `callGetMethod`: Zod schema for the result of calling a method.
9. `messageData`: Zod schema for various types of message data.
10. `message`: Zod schema for a message.
11. `transaction`: Zod schema for a transaction.
12. `getTransactions`: Zod schema for an array of transactions.
13. `getMasterchain`: Zod schema for information about the masterchain.
14. `getShards`: Zod schema for information about shards.
15. `blockShortTxt`: Zod schema for short transaction IDs.
16. `getBlockTransactions`: Zod schema for block transactions.

#### Utility Classes
17. `TypedCache`: A generic cache class with typed methods for getting and setting values.

#### Class
18. `HttpApi`: A class representing an HTTP API client for interacting with the TON blockchain. It includes methods for various blockchain-related operations such as getting address information, transactions, masterchain info, and shard information.

### Detailed Explanation
1. **Zod Schemas**: These are TypeScript schemas created using the Zod library for validating and parsing JSON objects received from the API.

2. **TypedCache Class**: A generic cache class designed to store and retrieve values based on a specified key type and value type. It is used within the `HttpApi` class for caching shard and shard transaction information.

3. **HttpApi Class**: The main class representing the HTTP API client. It includes methods for various blockchain-related operations such as getting address information, transactions, masterchain info, and shard information. The class also uses the `axios` library for making HTTP requests and caches responses using the `TypedCache` class.

4. **serializeStack Function**: A utility function for serializing a stack of TupleItems. It converts different types of stack items into a format suitable for API requests.

### Note
This code assumes that the reader is familiar with the TON blockchain and its related concepts. The functionality of the code revolves around making HTTP requests to a TON blockchain API and handling the responses in a typed manner using Zod schemas.

***


Certainly! Let's go deeper into each section of the code.

### 1. Zod Schemas

#### `blockIdExt`
- This Zod schema represents an extended block ID.
- It includes properties such as `workchain`, `shard`, `seqno`, `root_hash`, and `file_hash`.

#### `addressInformation`
- Represents information about an address on the TON blockchain.
- It includes properties like `balance`, `state`, `data`, `code`, `last_transaction_id`, `block_id`, and `sync_utime`.

#### `bocResponse`
- Describes the response structure for operations related to BOC (Bag of Cells).
- It expects an object with a literal property `@type` set to `'ok'`.

#### `feeResponse`
- Represents the response structure for querying fees.
- It includes detailed fee information such as `in_fwd_fee`, `storage_fee`, `gas_fee`, and `fwd_fee`.

#### `callGetMethod`
- Describes the response structure for calling a method.
- It includes properties like `gas_used`, `exit_code`, and `stack`.

#### `messageData`
- A union of different types of message data structures.
- Each variant corresponds to a different type of message data, such as raw, text, decrypted text, or encrypted text.

#### `message`
- Describes the structure of a message on the TON blockchain.
- It includes properties like `source`, `destination`, `value`, `fwd_fee`, `ihr_fee`, `created_lt`, `body_hash`, `msg_data`, and `message`.

#### `transaction`
- Describes the structure of a transaction on the TON blockchain.
- It includes properties like `data`, `utime`, `transaction_id`, `fee`, `storage_fee`, `other_fee`, `in_msg`, and `out_msgs`.

#### `getTransactions`
- Represents an array of transactions.
- It is based on the `transaction` schema.

#### `getMasterchain`
- Describes information about the masterchain.
- Includes properties like `state_root_hash`, `last`, and `init`, where `last` and `init` are blockIdExt structures.

#### `getShards`
- Describes information about shards.
- It includes an array of `blockIdExt` structures.

#### `blockShortTxt`
- Describes short transaction IDs for blocks.
- It includes properties such as `@type`, `mode`, `account`, `lt`, and `hash`.

#### `getBlockTransactions`
- Describes the structure of block transactions.
- It includes properties like `id`, `req_count`, `incomplete`, and an array of `blockShortTxt` structures.

### 2. Utility Classes

#### `TypedCache Class`
- A generic cache class designed to store and retrieve values based on a specified key type and value type.
- It has methods `get` and `set` for retrieving and storing values in the cache.

### 3. HttpApi Class

#### Constructor
- Accepts an endpoint URL and optional parameters like timeout, API key, and an adapter for Axios.
- Initializes the `TypedCache` instances for shards and shard transactions.

#### Methods

- `getAddressInformation(address: Address)`
  - Calls the `doCall` method for the 'getAddressInformation' API endpoint.

- `getTransactions(address: Address, opts: { limit: number, lt?: string, hash?: string, to_lt?: string, inclusive?: boolean })`
  - Calls the `doCall` method for the 'getTransactions' API endpoint.
  - Adjusts parameters like `hash` and `limit` based on options.

- `getMasterchainInfo()`
  - Calls the `doCall` method for the 'getMasterchainInfo' API endpoint.

- `getShards(seqno: number)`
  - Uses a DataLoader to efficiently load shard information.

- `getBlockTransactions(workchain: number, seqno: number, shard: string)`
  - Uses a DataLoader to efficiently load block transactions.

- `getTransaction(address: Address, lt: string, hash: string)`
  - Calls the `doCall` method for the 'getTransactions' API endpoint and retrieves a specific transaction.

- `callGetMethod(address: Address, method: string, stack: TupleItem[])`
  - Calls the `doCall` method for the 'runGetMethod' API endpoint.

- `sendBoc(body: Buffer)`
  - Calls the `doCall` method for the 'sendBoc' API endpoint.

- `estimateFee(address: Address, args: { body: Cell, initCode: Cell | null, initData: Cell | null, ignoreSignature: boolean })`
  - Calls the `doCall` method for the 'estimateFee' API endpoint.

- `doCall<T>(method: string, body: any, codec: z.ZodType<T>)`
  - Performs the actual HTTP request using Axios.
  - Handles headers, timeouts, and response parsing with Zod schemas.

### 4. serializeStack Function
- A utility function for converting a stack of TupleItems into a format suitable for API requests.

This comprehensive breakdown should provide a clearer understanding of the code's structure and purpose.

