# Project 3: Collaboration and Competition


## Objective
The objective of this project is to implement two Deep Deterministic Policy Gradients (DDPG) agents to control rackets to bounce a ball over a net

In order to solve the environment, the average (over 100 episodes) of those scores is at least +0.5.

## Environment description 
The observation space consists of 24 variables corresponding to the position and velocity of the ball and racket. Each agent receives its own, local observation. Two continuous actions are available, corresponding to movement toward (or away from) the net, and jumping. 

## Agent Implementation

The "Tennis" environment similar to the "Reacher" environment, however, it has two contiguous actions and rewards available. A continuous space indicates that there are infinite range of action values to control the arm.

Therefore, two independent policy-based methods need to be used to solve the environment, I didn't choose to share the memory buffer between these two agents, I found the learning speed is a lot quicker without share memory buffer.

The agents in the project was built on the DDPG agent in the Continuous-control project.

### Actor-Critic Method

The Actor-Critic method contains two networks:

1. Actor network that learns how to react by estimating the best policy and maximum the reward
2. Critic network that learns how to state-action pair values

The Actor-Critic method combines the two networks to accelerate the learning process.

#### Actor Model

`Layers:`
-       self.fc1 = nn.Linear(24, 512)
        self.fc2 = nn.Linear(512, 512)
        self.fc3 = nn.Linear(512, 2)
        self.bn = nn.BatchNorm1d(fc1_units)
`Forward:`
-       def forward(self, state):
            if state.dim() == 1:
                state = torch.unsqueeze(state,0)
            x = F.relu(self.fc1(state))
            x = self.bn(x)
            x = F.relu(self.fc2(x))
            return F.tanh(self.fc3(x))


#### Critic Model
`Layers:`
-       self.fc1 = nn.Linear(24, 512)
        self.fc2 = nn.Linear(512+2, 512)
        self.fc3 = nn.Linear(512, 1)
        self.bn = nn.BatchNorm1d(fc1_units)
`Forward:`
-       def forward(self, state, action):
            if state.dim() == 1:
                state = torch.unsqueeze(state,0)
            x = F.relu(self.fcs1(state))
            x = self.bn(x)
            x = torch.cat((x, action), dim=1)
            x = F.relu(self.fc2(x))
            return self.fc3(x)

#### Hyperparameters

BUFFER_SIZE = 1e7  # replay buffer size
BATCH_SIZE = 256        # minibatch size
GAMMA = 0.99            # discount factor
TAU = 1e-3              # for soft update of target parameters
LR_ACTOR = 2e-4         # learning rate of the actor 
LR_CRITIC = 2e-4        # learning rate of the critic
WEIGHT_DECAY = 0        # L2 weight decay

#### Experience Reply

The agents utilizes a reply buffer each to obtain experience tuples wish state, action, reward, next state, and done to learn from the past experience.

The experiences are sampled randomly to avoid over-fitting and bias.

## Result

The agent was able to archive the an average reward of 0.5 over the last 100 episode with 1457 episodes

The scores diagram can be found in Tennis.ipynb notebook

## Future work
- Experiment with dropout layers on the agent
- Experience replay prioritization, select experience based on priority value instead of random samepling
- Hyper-parameters fine tuning
- Experiment with other methods, such as PPO, A3C, and D4PG



