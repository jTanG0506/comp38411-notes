---
title: Attack Surface
markup: "mmark"
weight: 5
---

## Attack Surfaces and Attack Trees
### Attack surfaces
An **attack surface** consists of reachable and exploitable vulnerabilities in a system and can be classified into three categories:
- **Network attack surface** refers to vulnerabilities over an enterprise network or the internet. Examples of this include network protocol vulnerabilities, such as those used for a DDoS attack, disruption of communication links and various forms of intruder attacks.
- **Software attack surface** refers to vulnerabilities in application, utility, or operating system code. A particular focus in this category is web server software.
- **Human attack surface** refers to vulnerabilities created by personnel or outsiders, such as social engineering, human error and trusted insiders.

#### Examples
Some possible attack surfaces can be easily identified by reading the source code and identifying different points of entry and exit, such as:
- HTTP headers and cookies
- Databases and other local storage
- Run-time arguments
- Open ports on outward-facing servers
- Employees with access to sensitive information vulnerable to social engineering

------

### Attack surface analysis
An **attack surface analysis** is a technique used to map out what parts of a system need to reviewed and tested for security vulnerabilities. The purpose of an attack surface analysis is to understand the risk areas in an application and make developers and security specialists aware of where security mechanisms are required. Once this analysis is carried out, usually by security architects or penetration testers, developers then may be able to find ways to minimise this surface, thus making the task of an adversary more difficult.

------

### Attack trees
An **attack tree** is a tree structure that represents attacks against a system, with the security incident that is the goal of the attack being the root node, and different ways of achieving that goal as child nodes. Each subnode defines a subgoal, and each subgoal may, in turn, have its own set of subgoals, and so on. The leaf nodes of the attack tree represent different ways to initiate an attack. Each node other than a leaf is either an *AND*-node or an *OR*-node. To achieve the goal represented by an *AND*-node, all of the child subgoals must be achieved; and for an *OR*-node, at least one of the child subgoals must be achieved.

#### Example
![Attack Tree](/docs/figures/attack-tree.png)
Usually, nodes are labelled with values representing difficulty, cost (in terms of both monetary and time), or other attack attributes, so that alternative attacks can be compared. In the example above, our goal is to steal sensitive customer information. With the assumption that all subgoal nodes are *OR*-nodes, our attack tree shows that hacking the customers home system would be the cheapest attack, while a heist would be the most expensive attack.
