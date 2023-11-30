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
In the preceding lesson, we delved into the memory layout and instantiation of TVM. Now, let's explore the process of message handling and understand what a transaction truly entails. To initiate any change in the state of a contract, it must receive a single message, referred to as an incoming message. There are two types of incoming messages: external and internal.

An external message is essentially a data string that originates seemingly from nowhere, at least from the blockchain's perspective. This data lacks inherent authentication and doesn't carry any monetary value in the form of tone coins. Its content can vary, determined by the contract author. The contract's responsibility is to interpret and parse this data within its code.

On the other hand, internal messages are those sent by contracts to other contracts. These messages possess a more structured format. Firstly, internal messages can include balances, allowing contracts to attach any amount of coins when sending a message to another contract. Secondly, these messages are securely authenticated by the sending contract's address, a crucial aspect of authentication in later lessons.

Upon receiving a message, a contract may have separate handlers for internal and external messages. Both message types carry arbitrary payload data defined by the contract author. If a contract intends to process different message types, it utilizes op codes, denoted by a four-byte prefix in the message's custom data, indicating the supported operation type.

So, what is a transaction? A transaction represents a series of state changes in a contract. The message serves as an input to the transaction, while the transaction itself, through TVM operations, alters the state and generates a new state for the contract, along with a list of outgoing actions.

There are five phases in a transaction:

1. **Storage Phase:** The blockchain charges the contract rent for its existence during this phase. Rent is computed as a price per bit per second, deducted from the contract's balance. If the contract runs out of money at this point, the execution aborts.

2. **Credit Phase:** Coins attached to incoming messages are credited to the contract during this phase. External messages lack coins, but internal messages typically carry them.

3. **Computation Phase:** This is where the program executes, with TVM verifying each operation and tracking gas usage. Gas limits can be set by the program designer.

4. **Action Phase:** The new state of the contract is a key action in this phase. Other actions include outgoing messages to other contracts, defining interactions.

5. **Bounce Phase:** In case of failure and a bounceable message flag, any remaining money triggers the contract to create an outgoing message back to the sender, a safety feature allowing fund recovery in case of errors.

In summary, a contract receives incoming messages, internal or external, processes them through five transaction phases, and produces a new state along with outgoing actions. This comprehensive overview outlines the intricate journey of messages and transactions in TVM. Thank you.
So far, we've delved into the memory layout and the anatomy of transactions. Now, let's shift our focus to message authentication in contracts. In this session, I'll discuss four distinct categories of authentication. The first method involves simple classic authentication using a signature.

As a quick recap, any event in the TON blockchain initiates with an external message. These external messages, initially unauthenticated data, typically land in wallet contracts associated with individual users. The wallet's primary functions include storing TON coin balances, managing sequence numbers to prevent message replay, and storing public keys. When an external message arrives, the wallet reads the 64-byte signature, verifies it with the stored public key, and interprets the remaining message as instructions for internal contract communication.

It's essential to reserve this classic authentication for wallets to enhance system composability and flexibility. The second authentication mechanism centers on authenticating the message sender for internal messages within the TON ecosystem. Each internal message carries a guaranteed and secure message sender identity, simplifying the verification process. This approach is more cost-effective and powerful than relying on signatures, enabling flexibility in connecting various contracts.

The third authentication method builds upon the message sender concept by utilizing cryptographic hashes of contract code and data. TON ecosystem addresses not only serve as unique identifiers but also secure hashes. This enables a "DNA check," allowing contracts to authenticate communication by confirming that the sender executed their part of the protocol correctly.

Lastly, the fourth type of authentication emphasizes the deliberate lack of authentication. In certain scenarios, not authenticating messages becomes a security measure. For instance, in a decentralized staking pool requiring censorship resistance, a message handler could forego authentication, allowing anyone to trigger the message while incorporating other protective measures.

In summary, TON employs four authentication types: external signature-based authentication, internal authentication by message sender, DNA checks for code/data verification, and intentional lack of authentication for specific use cases requiring censorship resistance. Each method serves distinct purposes in building secure and flexible decentralized applications.

Certainly, here's a more polished and professionally rephrased version:

"The Telegram Open Network (TON) is a complex system, with its model of transaction costs and fees standing out as particularly intricate. In this exploration, we delve into the significance of understanding these fees. TON, being a public network, is susceptible to attacks by any party. Consequently, safeguarding this shared resource from denial-of-service attacks requires careful consideration of any non-trivial costs borne by participants.

Within TON, three primary fee categories exist: gas costs, rent, and message fees. Gas costs, originating from Ethereum, represent the computation platform's fuel for executing code. Adjusting gas prices in response to fluctuations in the TON coin's value ensures that the real cost of execution is reflected.

TON differs from Ethereum and Bitcoin by not imposing a market for the scarce gas or block size. Instead, it scales infinitely, allowing validators to earn higher fees during periods of increased network load. Mechanically, the TON Virtual Machine (TVM) checks each instruction for correctness during code execution, reducing the sender's balance by the gas cost incurred.

The second cost category is rent, defined as the cost per bit of data stored by a contract per second. Rent is charged when a transaction begins, based on the contract's storage size and the time since the previous transaction.

The third category encompasses fees for importing, creating outgoing messages, and updating contract storage. These fees, applied per message or byte, play a role in the action phase when contracts create outgoing messages.

Designing smart contracts requires careful consideration of gas and rent payments to prevent issues like running out of gas or being frozen due to depleted rent. Decentralized apps can rely on users topping up contract balances. Additionally, by placing gas and execution costs on senders, you can ensure an upper bound on execution costs, with excess funds returned in a separate message.

To enhance user experience, a common pattern involves sending excess coins back to the sender's address. Finally, designing contracts with a constant cost in terms of storage and computation enhances predictability and mitigates the risk of entering inconsistent states or running out of funds."

Certainly! Here's a rephrased version:

Let's delve into contracts and discuss scalability. In the initial chapter, we touched upon the concept of infinite horizontal scalability within the entire TON blockchain. However, it's crucial to understand the practical implications of this scalability and how you can leverage it in your applications.

In the TON blockchain, scalability is guaranteed down to the granularity of individual contracts. This means that if you have two distinct accounts, they could potentially exist in separate shard chains operated by independent consensus groups. Consequently, operations on one contract won't impede operations on another. The TON Network manages the message routing between them.

It's essential to note that TON provides a specific guarantee on scalability at the contract level. While it ensures scalability at this level, it doesn't offer tools within the code and data of your application to make your contracts more scalable. Therefore, the responsibility falls on you to optimize for the entire scalability of the blockchain and prevent bottlenecks in your contracts.

Now, let's address potential challenges in designing your application. One common pitfall is designing contracts similarly to monolithic applications with variable-length storage, as seen in traditional systems or Ethereum. For instance, Ethereum implements tokens as a banking ledger within a single smart contract, leading to scalability issues.

In TON, this approach won't scale. Placing a lengthy list in a single contract results in growing rent costs for stored data, making it challenging to distribute this cost among users. Moreover, transaction fees increase for users operating on larger amounts of data in the contract.

To address this, guidelines are provided for designing multi-user and large-scale applications in TON. First, prioritize avoiding variable-length data for constant storage and predictable operation costs. If a list is necessary, keep it short and bounded. For dynamically growing lists, ensure the contract is owned by a single user with exclusive control.

A scalable approach involves using the blockchain itself as an array. For example, in a decentralized exchange, individual contract instances can be created for each user to track balances. This decentralizes internal storage into a scalable array of contracts, keeping the central contract a constant size.

A notable example is the design of tokens in the TON ecosystem. Rather than a single contract storing a list of all users, a minor contract authorizes the minting of tokens for users. Balances are maintained in separate contracts (jeton wallets), tied to users' wallets. These contracts collaborate through messages, ensuring scalability without bottlenecks.

In summary, a successful, scalable application in TON involves contracts with constant storage and operating costs. This leads to predictable rent and gas costs for users, enabling seamless integration with other contracts. Predictability ensures user satisfaction, making your application successful and scalable to millions. In the upcoming lesson, we'll explore specific instances where these principles are implemented.
Welcome to Lesson Six. In this session, we will examine three examples illustrating the application of contract design principles to diverse applications. We'll explore how the TON platform facilitates the creation of scalable and secure applications. Let's commence with an exploration of tokens.

As you may recall from Ethereum or non-blockchain implementations, tokens are traditionally managed as a simple ledger of accounts. However, this approach proves inefficient in the blockchain context, as smart contracts grow with the user base, resulting in increased interaction costs. In TON, the token approach involves implementing distinct contracts for each user, termed "jeton wallets," along with a separate Minter contract to facilitate the creation of new token units. Authentication of token transfers between jeton wallets is achieved through a two-step process, ensuring secure transactions. Despite the breakdown of atomicity, the TON blockchain guarantees eventual balance synchronization, enabling infinite scalability for token operations.

Moving on to our second example, we'll delve into multi-signature contracts. In traditional implementations, managing pending requests and handling potential malicious actions can be challenging. TON offers a simpler and more scalable solution by tokenizing user-initiated requests. Users receive unique tokens encapsulating their requests and votes, preventing abuse by malicious users. This tokenization strategy simplifies contract logic, providing effective protection against denial-of-service attacks.

Our final example focuses on user wallets, specifically version 4, which supports plugins for subscription payments. While powerful, this approach can be optimized. Instead of listing specific plugin addresses, TON suggests listing distinct code implementations. This not only reduces storage costs but also allows multiple plugins sharing the same code to be represented by a single record. Mutual authentication between the wallet and plugin contracts ensures security, resembling the tokenization principles discussed earlier. This approach streamlines wallet functionality, alleviating concerns about storage costs and enhancing user experience.

In summary, these examples showcase the application of scalable contract design principles in various scenarios, highlighting TON's innovative solutions. Thank you for your attention, and I look forward to our next chapter.
