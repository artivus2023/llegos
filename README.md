# Llegos

Welcome to Llegos, a toolkit for crafting advanced multi-generative-agent systems.

In the rapidly evolving field of artificial intelligence, multi-agent systems have emerged as a powerful paradigm for modeling complex behaviors and interactions. Whether it's a team of robots, a colleciton of software agents, or a group of virtual characters interacting in a video game, the frontier multi-agent systems powered by LLMs remains to be explored.

That's where Llegos comes in. With Llegos, you can create, manage, and simulate a wide variety of multi-agent systems, from the simple to the complex. This toolkit offers a range of features designed to make it simpler to build multi-agent systems.

## Features

1. **Multi-Agent Systems:** With Llegos, you can create systems composed of multiple autonomous agents, enabling rich and intricate behaviors and interactions.
2. **Concurrent Message-Passing:** Communication between agents in Llegos is facilitated through a flexible message-passing paradigm, allowing for efficient and versatile inter-agent dialogue. Agents and/or process-based concurrency.
4. **Event-Driven Architecture:** Llegos embraces an event-driven programming approach, allowing agents to dynamically respond to events occurring within the system.
5. **Flexibility:** The toolkit provides foundational structures, namely Behavior and Context, for you to develop and customize your own autonomous agents and networks. This flexibility allows you to tailor Llegos to suit your unique requirements and use cases.

## Elegant Abstractions

Llegos centers around three abstractions: Messages, Generative Agents, and Generative Contexts. These elements work together to facilitate complex interactions and behaviors in your multi-agent systems.

## Messages

Communication is facilitated through the [Message](./Llegos/message.py) data model. In Llegos, a Message carries various forms of information, requests, or commands, acting as the primary conduit for inter-agent communication.

Messages have the following attributes:

1. **method**: This is a Union of string and `Method` type. It determines the type of operation or action that the message is meant to invoke. By default, nodes will dispatch messages to a method named after the action. For example, a message with action "step" will call the "step" method. For async nodes, the method name will be prefixed with "a", so "step" becomes "astep".
2. **role**: This is of type `Role`, and it is used to categorize the sender of the message. The sender can be an assistant, user, or system. This is useful for serializing to chat endpoints.
3. **body**: This is a string that contains the actual content of the message.
4. **created_at**: This is a datetime object that indicates when the message was created. It is automatically set to the current time when the message is instantiated.
5. **sender**: This is an `AbstractObject` (which could be a `Behavior`) that sent the message. It is set to None by default, and can be set when the message is created.
6. **receiver**: This is an `AbstractObject` (which could be a `Behavior`) that is the intended recipient of the message. Like `sender`, it is also set to None by default, and can be set when the message is created.
7. **parent**: This is a reference to another `Message` object. It is used when a message is a reply to a previous message. This allows for the creation of message threads and is useful for maintaining context in conversations.

The `Message` class also includes a couple of class methods:

1. **reply**: This method creates a new message that is a reply to a previous message. It automatically sets the `sender`, `receiver`, `parent`, `role`, and `method` fields appropriately.
2. **init_fn**: This is a property that returns a dictionary representing the schema for initializing a new `Message` object. This is useful for using JSON schema function completions to generate a new Message.

### Generative Agents

At the heart of Llegos are the Generative Agents, or Behaviors. A [Behavior](./llegos/ephemeral.py) represents an individual autonomous agent in your system. These agents can be customized to suit a wide range of use cases, allowing you to implement your own unique agent behaviors.

The Behavior class in Llegos provides a foundation for creating these agents. It includes built-in methods for receiving and handling messages, emitting events, and listening to events from other agents. This event-driven architecture allows for a reactive programming paradigm where agents can dynamically respond to events occurring within the system.

Behaviors can be used to model:

1. Stateful Agent: An agent maintains some internal state that is updated each time it receives a message.
2. Decision-Making Agent: An agent receives a message, makes a decision based on the content of the message, and then sends a message with its decision.
3. Task-Performing Agent: An agent receives a message instructing it to perform a certain task, such as making an API call or computing a result, and then sends a message with the result of the task.
4. Listening Agent: An agent listens for certain events from other agents and reacts when those events occur.

## Generative Contexts

Llegos also introduces the concept of Contexts.  Contexts provide a way to pass events up the agent network without having to pass intermediate results up manually at every level.

A Context a higher-level structure that represents a network of Behaviors. A Context is a multi-graph where nodes represent Behaviors and edges point to other Behaviors listening to emitted Messages. This EventEmitter edge forms a communication channel, where an emitted Message is received by a listening Behavior. This graph-based structure supports a broad spectrum of multi-agent architectures, from simple linear flows to complex network interactions.

Moreover, Contexts provide a context for the agents they contain. This is particularly valuable in more complex scenarios where an agent's behavior might depend on its network context, say, by providing a directory of other agents. Behaviors can leverage their Context during message propagation. This enables more sophisticated interactions and cooperation between agents.

A key advantage of using Contexts is that it allows for encapsulation and modularity. You can design individual Behaviors with specific behaviors and then compose them into larger, more complex Contexts. Since Contexts subclass Behaviors, you can compose Contexts within Contexts! This recursive ability makes it easier to build deep multi-agent systems. In essence, Contexts act as a scaffolding for the structure of your multi-agent systems.

Contexts can be used to model:

1. **Multi-Agent Coordination:** Multiple agents coordinate to solve a problem, with each agent handling a different part of the problem.
2. **Agent Hierarchy:** Agents are organized into a hierarchy, with some agents directing the actions of others.
3. **Feedback Loop:** Agents form a feedback loop, where the output of one agent feeds into the input of another, and so on.
4. **Distributed System:** A complex system where tasks are distributed among multiple agents, and the results are collected and combined by another agent.

## Future Work

As Llegos is still in its early stages, there are many exciting directions for future development. Here are some areas we're particularly interested in:

1. **Autonomous Agent Architectures**: We're interested in developers like you creating your own agent architectures with Llegos and are happy to feature your repository.
1. **Expanded Communication Protocols**: While Llegos currently supports simple message passing, we're interested in adding more advanced communication protocols. This might include negotiation protocols, voting mechanisms, or more complex dialogue management strategies.
3. **Integration with Other Libraries**: Llegos is designed to be flexible and adaptable. We're interested in making it easy to integrate Llegos with other popular machine learning, natural language processing, and agent-based modeling libraries.

We're excited to see where the community takes Llegos, and we look forward to collaborating with you!

## Contribution

Contributions to Llegos are welcome and greatly appreciated. We believe that the community's collective wisdom and creativity will enable us to build a more powerful and flexible tool. If you're interested in contributing, here are some guidelines:

1. **Issues**: Feel free to submit issues regarding bugs, feature requests, or other areas of concern. We value your feedback and will do our best to respond in a timely manner.
2. **Code**: If you're interested in adding a feature or fixing a bug, please start by submitting an issue. Once we've had a chance to discuss your proposal, you can make a pull request with your changes.
3. **Documentation**: Clear and comprehensive documentation makes any project more accessible. If you notice an area of the documentation that could be improved, or if you want to create new tutorials or guides, we would love your help.
