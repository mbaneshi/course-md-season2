# course-md-season2
Title: Mastering Contract Design in TVM: A Comprehensive Tutorial

Introduction:

In our ongoing exploration of the Telegram Virtual Machine (TVM), we've covered topics ranging from memory layout to transaction phases. Today, we'll delve into the intricate process of message handling and transactional anatomy within TVM. Understanding these concepts is pivotal for anyone looking to build secure and scalable decentralized applications. In this tutorial, we'll break down the key elements and stages involved in processing messages and transactions within TVM.

Section 1: Types of Messages

To initiate any change in the state of a contract in TVM, it must receive a single message, known as an incoming message. These messages come in two types: external and internal. External messages are data strings arriving seemingly from nowhere in the blockchain's perspective. In contrast, internal messages are sent by contracts to other contracts, offering more structure and authentication. We'll explore how contracts handle these messages, interpret their data, and distinguish between the two types.

Section 2: Transaction Phases

A transaction in TVM comprises changes to the contract's state, with the message serving as input. We'll guide you through the five phases of a transaction:

1. **Storage Phase:** Understand how the blockchain charges the contract for rent during this phase, and what happens if the contract runs out of funds.

2. **Credit Phase:** Explore how coins attached to incoming messages are credited to the contract, and the distinctions between internal and external messages in this phase.

3. **Computation Phase:** Dive into the execution of the program, gas usage tracking, and the flexibility developers have in setting gas usage limits.

4. **Action Phase:** Learn about the crucial outcomes of this phase, including the contract's new state and potential outgoing messages to other contracts.

5. **Bounce Phase:** Explore the safety feature or intentional use of returning remaining funds to the sender if the contract fails, marked as bounceable.

Section 3: Transaction Overview

Summarize the entire transaction process, emphasizing that a contract receives incoming messages, processes them through the five transaction phases, and produces a new state along with outgoing actions. This comprehensive overview provides a deep understanding of how messages and transactions function in TVM.

Conclusion:

As we celebrate one year of exploring TVM and its intricacies, this tutorial marks another milestone in our journey. Armed with this knowledge, you're better equipped to design and implement secure, scalable contracts in the Telegram ecosystem. Thank you for being part of our learning community, and stay tuned for more insights into the fascinating world of decentralized applications. Happy coding!
