# Representation-of-a-Real-World-Problem-as-a-Markov-Decision-Process


## Aim

To represent the real-world problem of charging a mobile phone battery as a Markov Decision Process (MDP) by defining its states, actions, transition probabilities, reward function, and Python representation.

---

## Problem Statement
A smartphone user needs to charge a phone with a low battery and must choose the most suitable charging method. Three charging options are available: Fast Charger, Normal Charger, and Power Bank. The Fast Charger provides the quickest charging, while the Normal Charger and Power Bank may experience interruptions such as power cuts or low battery. If charging is interrupted, the phone takes longer to become fully charged. This charging process can be modeled as a Markov Decision Process (MDP) because the next state depends only on the current battery status and the selected charging method.

### Problem Description

Keeping a smartphone charged is a real-world sequential decision-making problem. The process begins when the phone battery becomes low. The user chooses one of the available charging methods: Fast Charger, Normal Charger, or Power Bank. The Fast Charger usually charges the phone quickly and reliably. The Normal Charger may be affected by power interruptions, while the Power Bank may have limited charge remaining. If charging continues successfully, the phone reaches full battery. However, if charging is interrupted, the user must wait or retry charging, increasing the total charging time.

This problem can be modeled as a Markov Decision Process (MDP), where the user is the agent, the battery and charging conditions represent the states, the selection of charging methods and charging decisions represent the actions, and reaching a fully charged battery quickly provides higher rewards, while interruptions and delays result in lower or negative rewards. The process terminates when the phone battery is fully charged.


---

## MDP Components

A Markov Decision Process is represented as:

$$
MDP = (S, A, P, R, \gamma)
$$

Where:

| Symbol | Meaning |
|---|---|
| $S$ | Set of states |
| $A$ | Set of actions |
| $P$ | Transition probability function |
| $R$ | Reward function |
| $\gamma$ | Discount factor |

---

## State Space

The state space consists of all possible situations that the smartphone can experience during the battery charging process.

Example format:

```text
S = {
    S0: Battery Low,
    S1: Fast Charger Connected,
    S2: Normal Charger Connected,
    S3: Power Bank Connected,
    S4: Charging Successfully,
    S5: Charging Interrupted,
    S6: Battery Fully Charged
}
```



---

## Sample State

S5: Charging Interrupted

This state represents the situation where the phone stops charging because of a power cut, loose charging cable, or low power bank battery. The user must reconnect the charger or wait before charging continues.


---

## Action Space

The action space consists of all possible actions the user can take during the charging process.
Example format:

```text
A = {
    A1: Connect Fast Charger,
    A2: Connect Normal Charger,
    A3: Connect Power Bank,
    A4: Continue Charging,
    A5: Retry Charging,
    A6: Battery Fully Charged,
    A7: Stop
}
```


---

## Sample Action

A1: Connect Fast Charger

This action represents the user's decision to charge the phone using a fast charger. It provides the quickest charging speed and usually leads to the highest reward.



---

## Transition Probability

The transition probability defines the likelihood of moving from one state to another after the user chooses a particular charging method. In this problem, the next state depends on the selected charging option and whether the charging process continues successfully or gets interrupted. The Fast Charger always charges the phone successfully, while the Normal Charger and Power Bank may either continue charging or experience an interruption due to a power cut or low battery.
General form:

$$
P(s' \mid s,a)
$$


where:

where:

- $s$ = Current state
- $a$ = Action taken
- $s'$ = Next state

The transition probabilities for this MDP are:

| Current State | Action | Next State | Probability |
|---------------|--------|------------|-------------|
| Battery Low (S0) | Connect Fast Charger | Fast Charger Connected (S1) | 1.0 |
| Battery Low (S0) | Connect Normal Charger | Normal Charger Connected (S2) | 1.0 |
| Battery Low (S0) | Connect Power Bank | Power Bank Connected (S3) | 1.0 |
| Normal Charger Connected (S2) | Continue Charging | Charging Successful (S4) | 0.8 |
| Normal Charger Connected (S2) | Continue Charging | Charging Interrupted (S5) | 0.2 |
| Power Bank Connected (S3) | Continue Charging | Charging Successful (S4) | 0.7 |
| Power Bank Connected (S3) | Continue Charging | Charging Interrupted (S5) | 0.3 |
| Fast Charger Connected (S1) | Battery Fully Charged | Battery Fully Charged (S6) | 1.0 |
| Charging Successful (S4) | Continue Charging | Battery Fully Charged (S6) | 1.0 |
| Charging Interrupted (S5) | Retry Charging | Battery Fully Charged (S6) | 1.0 |
---

## Reward Function
The reward function provides feedback to the user (agent) after taking an action and moving from one state to another. Charging methods that help the phone reach a fully charged battery quickly receive higher rewards, while charging delays caused by power interruptions or a low power bank receive lower or negative rewards.

General form:

$$
R(s,a,s')
$$


The reward values for this MDP are:

| Current State | Action | Next State | Reward |
|---------------|--------|------------|--------|
| Battery Low (S0) | Connect Fast Charger | Fast Charger Connected (S1) | +10 |
| Battery Low (S0) | Connect Normal Charger | Normal Charger Connected (S2) | +6 |
| Battery Low (S0) | Connect Power Bank | Power Bank Connected (S3) | +4 |
| Normal Charger Connected (S2) | Continue Charging | Charging Successful (S4) | +5 |
| Normal Charger Connected (S2) | Charging Interrupted | Charging Interrupted (S5) | -3 |
| Power Bank Connected (S3) | Continue Charging | Charging Successful (S4) | +3 |
| Power Bank Connected (S3) | Charging Interrupted | Charging Interrupted (S5) | -5 |
| Fast Charger Connected (S1) | Battery Fully Charged | Battery Fully Charged (S6) | +10 |
| Charging Successful (S4) | Continue Charging | Battery Fully Charged (S6) | +8 |
| Charging Interrupted (S5) | Retry Charging | Battery Fully Charged (S6) | +4 |

---

## Graphical Representation



<img width="1536" height="1024" alt="RL-1" src="https://github.com/user-attachments/assets/973e9624-b350-4920-b808-a0e801f82c4c" />

---

## Python Representation




```python
# MDP Representation using Python
from pprint import pprint

print("Name: T MOUNISH")
print("Register Number:212223240098")

P = {

    "Battery Low": {

        "Use Fast Charger": [
            (1.0, "Fast Charger", 10, False)
        ],

        "Use Normal Charger": [
            (1.0, "Normal Charger", 6, False)
        ],

        "Use Power Bank": [
            (1.0, "Power Bank", 5, False)
        ]
    },

    "Fast Charger": {

        "Fully Charged": [
            (1.0, "Battery Fully Charged", 10, True)
        ]
    },

    "Normal Charger": {

        "Continue Charging": [
            (0.8, "Charging Successfully", 5, False)
        ],

        "Charging Interrupted": [
            (0.2, "Charging Interrupted", -3, False)
        ]
    },

    "Power Bank": {

        "Continue Charging": [
            (0.7, "Charging Successfully", 4, False)
        ],

        "Charging Interrupted": [
            (0.3, "Charging Interrupted", -4, False)
        ]
    },

    "Charging Successfully": {

        "Continue Charging": [
            (1.0, "Battery Fully Charged", 8, True)
        ]
    },

    "Charging Interrupted": {

        "Retry Charging": [
            (1.0, "Battery Fully Charged", 4, True)
        ]
    },

    "Battery Fully Charged": {

        "Stop": [
            (1.0, "Battery Fully Charged", 0, True)
        ]
    }
}

print("\nMDP Representation (P):\n")
pprint(P, width=100)

```
---
## Output
<img width="653" height="247" alt="image" src="https://github.com/user-attachments/assets/175b3e24-ab96-4661-a7ed-440ef83b5eff" />

---

## Result
Thus, the Phone Battery Charging problem was successfully modeled as a Markov Decision Process (MDP) and implemented in Python by defining its states, actions, transition probabilities, reward function, and terminal state, demonstrating the decision-making process involved in selecting the most efficient charging method to achieve a fully charged battery.



---

