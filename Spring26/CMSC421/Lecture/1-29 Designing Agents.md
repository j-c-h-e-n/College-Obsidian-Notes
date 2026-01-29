## Designing Agents
- The field of AI "is concerned with not just understanding but also *building* intelligent entities - machines that can compute how to act effectively and safely in a wide variety of novel situations".
	- The science of making machines that:
		- Think like people.
		- Act like people.
		- Act rationally.
- Rational: maximally achieving pre-defined goals.
	- Goals are expressed in terms of the **utility** of outcomes.
	- World is uncertain, so we use **expected** utility.
	- Thus, being rational means to act in a way that **maximizes your expected utility**.

Sense-reason-act loop
- Sense: gather input (sensors)
- Reason: thinking, compute.
- Act: outward behavior, performing an action.
- Questions to reflect on:
	- What is the robot trying to achieve?
	- What does the robot need to notice in this scene?
	- What options does the robot have?
	- What action should it take, and why?
	- What makes the problem difficult?

Agent: an entity that perceives and acts.

Reflex agent:
- Acts based on only the current perception.
- Follows condition-action rules, such as:
	- IF apple detected, then move forward.

Model-based reflex Agent
- Maintains an internal model of the environment.
	- What objects are
	- How actions change the world
	- What states are dangerous
	- What it cannot do safely.
- Uses memory of past percepts to infer the current state.
	- Ex: remembers there being a hole in front of the tree.
- Still uses condition-action rules, like a reflex agent, but decisions can be influenced by inferred states (hole is still in front of tree from a past percept), not just the current state.

Model-based, goal-based Agent
- Combines a world model (model-based) with explicit goals.
- Enables planning by evaluating sequences of actions to reach desired outcomes.
	- Will find any way to complete the goal, will likely not find the "best". ("Pass-fail")

Model-based, utility-based Agent
- Adds a utility function to evaluate trade-offs among competing goals or outcomes.
- Chooses the action that maximizes **expected utility**, not just one that achieves a goal.
	- Utility based will find the best, most optimal way to complete the goal. ("A-F")

General learning Agent:
- Adapts to new experiences and data.
- Performance element: carries out the robot's current policy.
- Critic: evaluate the robot's action based on the feedback from the environment.
- Learning element: improves the robot's internal model and policy over time.
- Problem generator: encourages exploration so the robot can discover better strategies.

## Representing Agents and the World
- PEAS: framework for specifying task environments.
	- Performance measure: What is success?
	- Environment: Where does the agent operate?
	- Actuators: How does the agent operate?
	- Sensors: How does the agent perceive?
![[Pasted image 20260129144206.png]]
Properties of Worlds:
- Observable or Partially Observable
	- Chess vs Poker
- Deterministic or Stochastic
	- Route planning vs Autonomous navigation
- Episodic or Sequential
	- Image classifier vs Complex navigation
- Static or Dynamic
	- Layout planning vs Autonomous navigation
- Discrete or Continuous
	- Optimization (scheduling) vs Robotic path planning

Selecting a state space
- Leverage abstraction as much as you can.