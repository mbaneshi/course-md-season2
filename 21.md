In our previous lesson, we delved into the memory layout and instantiation of TVM. Now, let's explore how messages are processed and what constitutes a transaction. To alter the state of a contract, it must receive a single message, known as an incoming message. There are two types of incoming messages: external and internal.

An external message is essentially a string of data arriving seemingly from nowhere in the blockchain's perspective. This data isn't inherently authenticated and lacks any attached currency. The contract's responsibility is to interpret and parse this data within its code. On the other hand, internal messages are sent by contracts to other contracts, offering more structure. They can carry balances, and their authenticity is securely verified by the sending contract's address.

Upon receiving a message, a contract may have separate handlers for internal and external messages. Both types can contain arbitrary payload data designed by the contract's author. To process different message types, contracts can use op codes, indicating the operation type through a prefixed four-byte convention in the message's custom data.

So, what exactly is a transaction? A transaction comprises changes to the contract's state, with the message serving as input. The transaction, during its execution, goes through five phases.

1. **Storage Phase:** The blockchain charges the contract for its rent during this phase, deducting the necessary amount from the contract's balance. If the contract runs out of money at this point, execution halts, and the contract is frozen until replenished by its owner.

2. **Credit Phase:** Coins attached to incoming messages are credited to the contract during this phase. This doesn't apply to external messages, as they lack attached coins.

3. **Computation Phase:** This is where the program comes to life. TVM executes the code, verifies each operation, and tracks gas usage. Developers can set gas usage limits based on various criteria.

4. **Action Phase:** The contract's new state is a crucial outcome of this phase. Other actions may include outgoing messages to communicate with other contracts.

5. **Bounce Phase:** If the contract fails and the incoming message is marked as bounceable, any remaining funds are returned to the sender. This acts as a safety feature or can be intentionally used as a contract feature.

In summary, a contract receives incoming messages, which can be internal or external, and processes them through five transaction phases: storage, credit, computation, action, and bounce. This overview provides a comprehensive understanding of messages and transactions in TVM. Thank you!
