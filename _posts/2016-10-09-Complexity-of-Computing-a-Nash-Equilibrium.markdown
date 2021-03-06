---
title: Review of Complexity of Computing a Nash Equilibrium
date: 2016-10-09
tags: Game Theory, Computational Complexity
layout: post
---



This is my review of [Complexity of Computing a Nash Equilibrium](https://people.csail.mit.edu/costis/simplified.pdf), by Daskalakis et al. 



# What is this paper about?

In game theory, we study the behaviour of rational agents in various model problems. Problems in game theory are modelled as a game in which several actors strategize in a self-interested fashion. In a game, each actor (player) has a set of possible actions that they could take. Games are played in a single round, where each player picks an action, and an outcome occurs as a function of the joint actions of all the players. Each agent receives some reward or punishment as a function of the outcome. Usually in game theory, we're interested in figuring out what rational agents would do when confronted with a problem. We can imagine a rational agent to be a computer program that selects actions stategicially to minimize its punishment or maximize its reward. When all the players are rational agents, it is sometimes straightforward to determine how a game will be played. For example, it might be the case the there's an obviously "best" action for every player. When the actions of the players are predictable, we say that the game supports an _equilibrium_ consisting of that particular arrangement of actions.

We might wonder whether there's always an equilibrium where every player's action is completely predictable. It is easy to show that there is not. For example, consider the game Matching Pennies. In this game, each player holds a penny concealed in thier hand. The players can elect to arrange the coin so that either heads or tails will be on top when their palm is opened.The outcome of the game is that if both players reveal the same side of the coin, player 1 gets both pennies. If the players reveal different sides of their coins, player 2 gets them both instead. Here's a table summarizing the game from the perspective of player 1:


| | Player 1 heads | Player 1 Tails|
|---|---|---|
|Player 2 heads |+1¢ |-1¢|
|Player 2 Tails |-1¢ |+1¢|



It should be obvious that deciding to always reveal the same side of the coin is a recipe for disaster. For example, if player 1 always revealed heads, then player 2 could win a penny from every time by always revealing tails. It's clear that rational actors won't play this game in a fully predictable way. 

Despite this, we can still say something about how rational actors will play this game. Observe that if a player selects heads or tails uniformly at random (i.e. each of them 50% of the time), then it does not matter what their opponent does: the coins will match as often as not. If a player selects heads more often than tails, or tails more often than heads, their opponent could exploit this to make more money than in the case where the coins match 50% of the time. For example, if player 1 pays heads 60% of the time, then if player 2 plays tails 100% of the time, the coins will be different 60% of the time, and player 2 will make more money. So there's an equilibrium where both players reveal heads half the time and tails half the time. Here's an illustration of this idea:

![A graph showing the optimal probability for Player 2 to reveal heads, as a function of the probability that Player 1 picks heads.](https://docs.google.com/drawings/d/1jC6pDqEal5l7tOb1qhik6X7Wl4U1qw7AFYlmfXLbv30/pub?w=960&h=720)


In the blue region, no matter how often player 2 picks heads, the coins will match half the time. This type of equilibrium, where the probability distribution over rational actors' actions is predictable, even though the exact actions they will take are not, is called a mixed strategy [Nash Equilibrium](https://en.wikipedia.org/wiki/Nash_equilibrium#Definitions) (because each player will play some mixture of the actions). Perhaps the biggest result in game theory is Nash's theorem. Nash's theorem shows something rather exciting: every game has at least one mixed strategy Nash equilibrium. 

This means that the behaviours
of rational agents should always be predictable to some degree (there might be more than one equilibrium, so it might not always be clear which one different rational agents will play). Nash's proof shows the existence of such an equilibrium, but does not explain how to find it in a practical way. Some 50 years after Nash's paper, there is still no general and efficient algorithm for finding Nash Equilibria. The main result of this paper by Papadimitriou et al. is to answer the question: how hard is it to find a Nash Equilibrium? The authors accomplish this by showing that finding a Nash Equilibrium is at least as hard as some other problems for which no known fast algorithms exist, and also that it is no hard than those problems. Surprisingly, these other problems are not NP-Complete, as we might expect, but in a more exotic complexity class called PPAD.

## Complexity 

Usually computational complexity is expressed (at least, outside of academic publications on complexity theory) in terms of the two familiar complexity classes seen in an undergraduate course on algorithms, or a popular science treatment of the subject: [P](https://en.wikipedia.org/wiki/P_(complexity)) and [NP](https://en.wikipedia.org/wiki/NP_(complexity)). 

P is the set of problems for which there exists a known algorithm that solves problem instances is a number of operations that is a polynomial function of the input size. For example, if the problem is sorting a list of numbers, the existence of a simple, slow, algorithm like Bubble Sort (which needs a number of operations that is a quadratic function of the length of the list) serves to show that the problem of sorting a list of numbers is in P. 

![Sorting within P](https://docs.google.com/drawings/d/1j63CsRHoGIUpcml4lD0fCiYcvJ4fDTv6XiVePKhi1JM/pub?w=960&h=720)

NP is the set of problems that can be _verified_ in polynomial time. Verification consists of taking a problem instance and proposed solution, and checking whether or not the solution is correct. For example, sorting a list of numbers is in NP because if I give you a list of numbers that are unsorted, and then a list that I claim contains the same numbers in sorted order, you can verify my claim by simply making sure the two lists contain the same numbers (quadratic time if done naively), and then making sure that the second list is indeed sorted (linear time). It should be obvious that every problem in P is also in NP: we can always run the algorithm in a polynomial number of steps, and see if we get a solution. However, there exist problems that can be verified in polynomial time, but for which no polynomial time algorithm is known. 


![P within NP](https://docs.google.com/drawings/d/1sAKdatgsu24H6rHUgU1gdMZcwHnWmmgesDJDMvmR5Ps/pub?w=960&h=720)


An example is something like [integer knapsack](https://en.wikipedia.org/wiki/Knapsack_problem). The integer knapsack problem consists of being given a bag with a certain total capacity (in liters, say), and a multiset of items of different sizes that are worth different amounts. The question that is asked is whether there exists a way to pack the knapsack such that the total value of the items in it is at least $$k$$. It should be apparent that brute-forcing this is not an option, since the number of subsets that can fit in the bag is going to be exponential in the size of the multiset. Likewise, it should be obvious that if I present you with a packed bag, it's easy to count up the value of the items in the bag, and see whether that's more than $$k$$ or not. There's no known deterministic polynomial time algorithm for finding such a packed bag though, so the problem is in NP, but not in P. In fact, the integer knapsack problem is a member of the NP-Complete (NPC) problems: the very hardest problems in NP. We might wonder what "hardest" means. In this case, the NPC problems are the hardest ones in NP insofar as figuring out how to solve any NPC problem efficiently would allow us to solve _every_ problem in NP efficiently.

![Knapsack in NPC](https://docs.google.com/drawings/d/1nKs_UJeD8e2X-SLeEAzVd4pQlreGCNf58ZX2LwrcNqs/pub?w=960&h=720)


### How hard is finding a Nash Equilibrium?
Finding a Nash Equilibrium is clearly in NP. If I give you a proposed equilibrium, you can pick a player, and efficiently determine whether they could earn more by playing differently. If no player can improve their profits by playing differently, then the players must be in an equilibrium. However, we can't be sure whether this problem is in P or not, because we don't have an efficient algorithm for solving it (yet). 

Usually complexity theorists use this starting point to show that a problem is at least as hard as one of the NP-Complete problems. However, most (all?) NP-Complete problems rely on the fact that a solution might not exist, and that proving that there's no solution is very difficult without enumerating all possible solutions. Nash's theorem ensures that there _is_ an equilibrium to find. Therefore, we suppose NASH is probably not NP-Complete.

![Where is NASH?](https://docs.google.com/drawings/d/1JfWGKftvY23OxOlNeVHcZc152C37zbKxxy5OehxMIcs/pub?w=960&h=720)

So NASH is easier than NP-Complete problems (or at least, incomparable with them), but it's in NP, and we don't have any algorithm that would put it in P after about 50 years of trying. A reasonable guess is that NP > NASH > P. As it turns out, there are some other problems that fit this description. We're just looking for other problems that have a non-constructive proof that there is always a solution. The nature of the non-constructive step turns out to determine similar sets of problems. PPAD is the set of problems where the non-constructive step in the proof is of the form "If a digraph has one node with different numbers of in- and out-edges, then there must be another such node.". PPAD is thought to be harder than P, but problems in PPAD do not have a definitive relationship to the NPC problems. 

PPAD stands for "Polynomial Parity Arguments on Directed graphs". The "Parity Arguments" bit means that we know any instance of a problem in this class has a solution because of some non-constructive proof based on a "parity" property. For example, if I tell you that socks are made in matching pairs of left and right chirality, and then give you a left sock, you can be sure that a right sock was made that matches it, even though you've never seen the right sock and have no idea how to find it. 


![Parity Example for Socks](https://docs.google.com/drawings/d/1MsGlBuMDCBsczjBmLupFMnl5KGcjprTn71PoO3fvoFs/pub?w=960&h=720)

The "on Directed graphs" bit means that the parity arguments are going to have to do with the properties of directed graphs. A directed graph is a collection of edges and vertices. Edges show connections from one vertex to another, but not necessarily the other way round. Here's an example:

![An example di-graph](https://docs.google.com/drawings/d/1BeyAWJGxgyuw2XinlEWjnnnM7ye82quwpo2dIvcIGgM/pub?w=960&h=720)

In this graph there are 3 vertices (A, B, and C), and four edges (A->B, B->A, B->C, A->C). Usually graph problems involve walking around on the graph. If you start at vertex A, you can walk to B and then back to A, by following the edges (the arrows). If you walk to C though, there's no way to get back out. C is said to be a _sink_ in this graph: stuff flows into it, but can't get back out. In contrast, vertices that you can leave, but never return to, are called _sources_. The terminology conjures up ideas of water flow: water can leave a source (tap), and flow in certain directions along various pipes, eventually reaching a sink (drain) that it can't leave from. Graphs of this kind can be used to model all sorts of different problems.

Finally, the Polynomal part of PPAD's name refers to the idea that certain questions can be answered about the graph in a polynomial number of steps. In particular, it must be possible to determine whether and how two vertices are connected.

### PPAD-Complete Problems

The canonical problem for this PPAD (i.e. the SAT of PPAD) is "END-OF-THE-LINE". It works like this: Suppose we have a graph G, where every vertex has at most one in-edge, and at most one out-edge. We are given particular vertex that has either an in- or an out-edge, but not both. We must output some other vertex of the graph that also has different numbers of such edges. 

The problem certainly has the flavour of PPAD! There's a directed graph, and if you think about it for a minute, it should be apparent really the problem is saying "Here's a source, find a sink." in a very particular type of graph. Here's an example instance of PPAD:

![Example of PPAD](https://docs.google.com/drawings/d/1GbmBKyAy1sE7p1eSVjFj2eAM0zs8O6Bu8zqCjRXBlDs/pub?w=960&h=720)

Notice that every vertex has at most one edge coming in, and at most one edge leaving. Looking at this example graph, it should become obvious that if there's a source (a vertex we can leave, but never enter) there must also be a sink (a vertex we can enter, but never leave). In this case, the problem consists of some description of the graph, and pointing at the vertex D as a source.

Of course, if the graph is provided in a standard format like the picture above, this is a boring (and very simple) problem to solve! Just count the number of edges of each type for every vertex. If we look at each vertex in turn, it's easy to tell that G is a sink because it has no out-edges. There are at most 2 directed edges for each vertex, so work is polynomial in the size of the graph. 

However, END-OF-THE-LINE is proposed to take a rather arcane input format. A graph is represented by a function that accepts the name of a vertex, and produces its successor, if one exists. For example, the graph in the example above would be encoded as follows:


|_v_ |_F(v)_
|A |B
|B |A
|C |C
|D |E
|E |F
|F |G
|G |G



Further, it is required that the function accept $$2^k$$ values for some integer $$k$$. That is, the number of vertices in the graph must be a power of 2. A second function is also provided that tells us what in-edge (if any) a vertex has. 

Now, notice that it's still a polynomial amount of work to check every vertex to see whether or not it's a sink: We just input some vertex $$v$$ into both the predecessor and successor functions. If it has a predecessor but not a successor in the graph, it's a sink. However, since the number of vertices is exponential in $$k$$, the amount of work does is exponential in $$k$$. As we'll later see, some reasonable problems with inputs that are of length $$l$$ can be reduced to the problem of solving END-OF-THE-LINE on a graph with $$2^l$$ vertices. 

Solving END-OF-THE-LINE efficiently requires us to have a general strategy that only looks at $$log_2(\vert V \vert)$$ nodes, where $$\vert V \vert$$ is the number of vertices in the graph. Looking at the problem, that seems impossible. It's like being dropped into a maze, and told you can only look at a logarithmic fraction of the rooms. Consider this graph:


![Hard EOTL instance](https://docs.google.com/drawings/d/1jOV0AWrfJzKcroTwdu0gJcNJoruAgJH_TOORJEgpkmY/pub?w=960&h=720)

If I told you to start at A, and count the edges on no more than 4 of the other nodes, how could you possibly hope to find the sink at L? That's what makes END-OF-THE-LINE a challenge.

# Brouwer's Theorem and PPAD-Hardness


Okay. So we have that NASH is an important problem, and that probably $$NP > NASH > P$$. We also have this weird complexity class PPAD, based around the equally weird problem END-OF-THE-LINE. The meat of the paper is the authors showing that NASH can be converted into END-OF-THE-LINE, and that END-OF-THE-LINE can be converted into NASH. These reductions establish that NASH is exactly as hard as END-OF-THE-LINE, and frankly, END-OF-THE-LINE seems ridiculously hard. To accomplish this, the authors rely on Brouwer's fixed point theorem, which is what's used in the core non-constructive step of Nash's theorem.

[Brouwer's fixed point theorem](https://en.wikipedia.org/wiki/Brouwer_fixed-point_theorem) says that if you map any "reasonable" subset of a Euclidian space to a "reasonable" subspace of itself, there's at least one point that doesn't move (i.e. the "fixed" point). Here, "reasonable" means that it's a contiguous proper sub-region of the space. So the unit ball is good (for any number of dimensions), but something like two disjoint balls isn't. 



Nash's theorem relies on this notion of fixed points. The dimensions of the space are given by the set of probabilities that each player uses to decide which strategy to play. This ends up being some sort of scaling of the unit ball for a high dimensional space, since the probabilities for each player need to sum to 1 (so we should get a ball with radius $$n$$, for $$n$$ players. Suppose that players adjust their strategies to improve utility, given the strategies of their opponents. Then each of these points has a successor point, the strategy profile that the players would move to if they started here. The mapping from points to successors is "reasonable", so by Brouwer's theorem, there's a fixed point, a point where the players don't want to move, which must be a mixed strategy Nash equilibrium. 


Here’s an example showing the idea for the matching pennies game from earlier in this post:

![NashFP](https://docs.google.com/drawings/d/1bxQlprnz-yKUVhiNLBjW4NW5eLLr9pPtjKOXiJOHuo4/pub?w=480&h=360)

In the lower right quadrant, player 1 wants to play heads less often, and player 2 wants to play heads more often. The game will move up and to the left. In the top right quadrant, player 1 wants to play heads more often, and player 2 wants to play heads less often. The game will drift down and to the right. It is possible to define an direction and magnitude of this drift by limiting each player to a finite change $$\epsilon$$, and observing the direction they will prefer to move in. In a Nash Equilibrium the players won’t want to move, so the equilibrium is a fixed point under this transformation.

## Brouwer is PPAD-Hard

The authors propose the computational search problem BROUWER, which takes the unit hyper-cube with $$m$$ dimensions, and a polynomial time-computable mapping $$F$$ from points in the cube $$x$$ to other points in the cube $$F(x)$$. The goal of the search problem is to find a fixed point of the mapping. They also require that $$F$$ obeys a Lipschitz condition: i.e. if two points $$x$$ and $$y$$ are a distance $$d$$ apart, then $$F(x)$$ and $$F(y)$$ are no more than $$K\times d$$ apart for some constant $$K$$.

To show that BROUWER maps to END-OF-THE-LINE, the authors propose the following technique:

1. Imagine a lattice of points over the hypercube, with spacing that "depends" on $$K$$, $$\epsilon$$ and $$m$$. Exactly how this dependency works is not explained.  I believe that the spacing needs to be such that the distance between diagonally adjacent points in the lattice is no more than $$\epsilon/K$$, but this might not quite be correct.
2. For every point in the lattice $$x$$, compute $$F(x)$$, which is an efficient operation. 
3. Divide the unit ball of dimension $$m$$ into three contiguous regions, and color them red, blue and yellow.
4. Compute the direction of the vector $$F(x) - x$$, and map that vector onto the unit ball. Color lattice point $$x$$ based on the corresponding color from step 3.


Notice that the points along each edge of the hypercube will naturally omit one color: if you're as far left as you can go, then there's no way to map a point to the left of where it is now, for instance. There's a related result from combinatorics called [Sperner's Lemma](https://en.wikipedia.org/wiki/Sperner%27s_lemma) that says if you make this kind of triangular tessellation of a space, and  color the vertices of the tessellation in this way, one of the triangles has a vertex of every one of the three colors. The Lipschitz condition means that if three points are close enough together (again, I wish they'd be more explicit about the lattice spacing), and yet mapped in three such radically different directions, they're near a fixed point of $$F$$. This kind of makes sense. The Lipzschitz condition ensures that under $$F$$, the image of the three points all need to be "close" to each other, within some constant multiple of the distance of the three points in the original arrangement. One supposes that if the lattice is arranged such that the points are within $$\epsilon/K$$ from each other in the original space (which we can do by making the lattice spacing sufficiently small), then the Lipschitz condition ensures that the three points all have to be with $$\epsilon$$ of each other in the resulting space. So probably the lattice isn't spaced with a distance of $$2\epsilon$$, but with a distance of $$2\epsilon/K$$.  

So now it's easy to convert the problem of finding an $$\epsilon$$-approximate fixed point (BROUWER) to END-OF-THE-LINE. Make a boolean circuit that encodes the direction of $$F(x) - x$$ for any mapping. This should be possible because we assumed $$F$$ was easy to compute. Enumerate  triangles that tesselate the space. There are countably many since the lattice spacing is finite. Build a boolean circuit that maps from each of the triangles to one of its neighbours according to the following rule: If one corner of the triangle is red, and the next corner clockwise from around the parameter is yellow, then create an out edge from the triangle with this number to its neighbour across the edge (here, clockwise just means with respect to some self-consistent view of the points). Notice that this ensures there is at most one out-edge for each triangle, and at most one in-edge for each triangle. You can draw the triangles out to prove this, or just look at this picture from the paper for a while:


![Excerpt from Figure 7 of the paper, to illustrate the triangle colouring.](https://github.com/jdoncs/jdoncs.github.io/raw/master/images/Fig7Exerpt.png)

Notice that if a triangle has two yellow vertices, or two red vertices, then it has both an in-edge and an out-edge. If it has two blue-vertices, it has no edges at all. However, there exists triangles on the perimeter of the space that _could_ have an in-edge, but only from a region outside the space. Any such triangle is a source. We know any problem will have at least one of these, because Sperner's lemma ensures there's a sink in the graph, and the PPAD observation itself ensures that if there's a sink then somewhere there must be a source. 

So we now imagine we had a fast algorithm for END-OF-THE-LINE, meaning one that was polynomial in $$k$$. We can define boolean circuits to compute these successor relationships with respect to different points in the space. The only other input PPAD requires is a vertex of this graph that has different numbers of in-edges and out-edges. This would have to be a point with 2 yellow and 1 red vertices (or 1 yellow and 2 red), but located on the the perimeter of the space. The authors use a clever trick to ensure that the perimeter of the space has a side that will start with every vertex along the side colored yellow, and at some point transition to every vertex being colored red. The transition point is sure to be a source, and can be found efficiently by doing, e.g. a binary search along the side, though the authors do not explain this part in detail. Anyway, the point is: we can define the triangles and the boolean circuits in polynomial time, and we can find a source vertex in time that is polynomial in the logarithm of the inverse of the lattice spacing. However, the number of triangles is proportionate to the inverse of the lattice spacing raised to the power of the number of dimensions. So this instance of END-OF-THE-LINE has something like $$O(2^{\frac{1}{\epsilon}}$$ nodes. Since we assumed there was an END-OF-THE-LINE algorithm that needed a polynomial number of steps in terms of $$k$$, and translating BROUWER to END-OF-THE-LINE needed only a polynomial number of steps in $$\frac{1}{\epsilon}$$, we can solve BROUWER in a number of steps polynomial in $$\frac{1}{\epsilon}$$. From this, we can conclude that BROUWER is no harder than END-OF-THE-LINE. IF we can solve END-OF-THE-LINE in a number of steps polynomial in $$k$$, we can also solve BROUWER efficiently with respect to $$\epsilon$$.

Of course, we were originally interested in NASH, but it's easy to see how to turn an instance of NASH into an instance of BROUWER, so it should be apparent that NASH is no harder than BROUWER. This means NASH is PPAD-Hard. Any efficient algorithm for problems in the class PPAD-Complete (like END-OF-THE-LINE) can be converted into an efficient algorithm for NASH. 

# NASH is PPAD-Complete

So NASH is no harder than PPAD, but is it any easier?

To show this, the authors first reduce solving an instance of END-OF-THE-LINE to solving an instance of BROUWER. After reading this part of the paper, I understand the gist of this reduction, but the details are described by the authors as "hard", and are left out. The idea is to that we'll be looking for a fixed point in 3-space. The space is partitioned into tiny "cubelets". The centers of the cublets define a lattice, and the lattice nodes are to be colored with one of _four_ colors (0, 1, 2, 3). Initially all nodes are colored "0". The mesh is fine enough so that each of the nodes in the END-OF-THE-LINE graph can be assigned to one cublet on each of the top-left and bottom right corners of the cube. If there is an edge from $$u$$ to $$v$$ in the END-OF-THE-LINE graph, then the coloring of the cublets on the interior of the cube can be defined so that the directions of $$F(x)-x$$ will yield a edge rule much like with the triangular tessellation from earlier, and there is a path formed by these colorings from the top-left point corresponding to $$u$$ to the bottom-left point corresponding to $$v$$. Likewise, the colors can be used to define a path from the bottom-left $$u$$ to the top-left $$v$$. After this encoding is complete, define a function $$F$$ such that $$F(x) - x$$ produces a vector whose's angle can be colored with one of the four colours used for the cublets. The authors claim (without proof here) that such a function can be defined so that it is easy to compute, and that it can be easily interpolated between the centers of the cubelets. If we had an efficient algorithm for BROUWER, we could then run that algorithm on $$F$$ to find a fixed point, and such a fixed point would be a solution to END-OF-THE-LINE when its coordinates were mapped onto the nearest cublet. I can kind of see how this works, but don't want to think too hard about it. The upshot is, BROUWER is PPAD-Complete, since it's no harder than END-OF-THE-LINE, and END-OF-THE-LINE is no harder than BROUWER is.

The final step then is to show that if we had an efficient algorithm for NASH, we could efficiently solve BROUWER. 

## Games as Boolean Circuits

The first time I read over the paper, I skimmed this part, which seemed almost like a footnote tacked onto the end. However, on closer reading I found this to be the most exciting part! 

Here's the basic idea:

1. Suppose we have an instance of BROUWER with some function $$F$$. Recall that $$F$$ must be easy to compute, with a polynomial-depth boolean circuit.
2. Define a game such that each gate in the boolean circuit representation of $$F$$ is converted to the actions of some subset of players of the game. (Wat?)
3. Define some more players of the game the respectively decide the inputs and outputs for the boolean circuit. Link their payoffs, so that these players are only paid if they adopt identical strategies.
4. Show that, in the Nash equilibrium of this game, the input and output players must have identical values, and the computation players must faithfully implement $F$. This is only possible if the input is a fixed-point of $$F$$. 


So the neat part of this proof was the process of defining a game that does arithmetic. The outline is that $$F$$ can be broken down into just a few kinds of boolean functions, notable addition, multiplication, and comparison. You can make a game that computes each of these, and then compose these games together into a larger game.

The paper gives a nice example with for computing $$Z=X*Y$$. We define 4 players $$w,x,y,z$$. Each player can play one of two actions, "STOP" and "GO". Their strategy is then defined as a probability ($$W,X,Y,Z$$) of playing GO. We do not pay $$x$$ or $$y$$ anything in this subgame, so they'll use whatever values they like. Usually these values will be defined by some other game. $$w$$ gets paid $$X \times Y$$ if it plays STOP, and $$Z$$ if it plays GO. $$z$$ gets paid $$X\times Y$$ for playing GO, and $$W$$ for playing STOP.  The unique equilibrium for this game is for $$w$$ to play $$GO$$ with probability $$X\times Y$$, and $$z$$ to play GO with probability $$X\times Y$$. Thus, if $$x$$ and $$y$$'s probabilities of playing GO were fixed, then the probability that $$z$$ plays GO is always $$X\times Y$$. We can then define the interior connections of the boolean circuit by connecting, e.g. $$z$$ as the $$x$$ or $$y$$ player in some other circuit game.

The input of $F$ is a three-dimension value in a finite cube. Simply map the range $$(0,1)$$ onto each axis of the cube, and define three players, one for each dimension. Like the others, they play either STOP or GO. Define three more points as players at the output of the $$F$$ circuit in the same way. The payoffs for the six input players are set so that they are only in equilibrium when the three input players and the three output players represent the same point with their probabilities.

The big catch with this is that if our circuit for $$F$$ had a polynomial number of gates $$n$$, then we have $$n+6$$ players each with a binary action, and thus a game with $$2^{n+6}$$ payoffs that need to be encoded. It's not obvious that a game with exponential size like this can be compactly encoded. If it can't, then the reduction from BROUWER to NASH is not polynomial time, and so even if we had a fast algorithm for NASH, we would still do exponential work to solve BROUWER (since we'd do exponential work just to convert an instance of BROUWER into an instance of NASH).

To fix this, the authors show that the game can be reduced to one played between 3 players. Basically, each boolean circuit will have some input players, some middle player, and some output player. As long as each of these groups is controlled by a different player, the circuit will end up in the right equilibrium. The authors show that you can color the players such that only three colors are needed, and therefore the game can be played by three people, each selecting between a linear number of actions that (Somehow? This point is not well explained) encode the exponential number of actions their gates might produce. The upshot is, any instance of BROUWER can be reduced to NASH for a 3-player game, so NASH with 3 players must be PPAD-Complete. Of course, it's easy to make this maping with more than 3 players (just add some dummy players that don't interact with the main 3). This means that NASH must be PPAD-Complete when there are 3 or more players.


# Other Tidbits

The authors mention Bibelus as an author who showed that any game played among more than 3 players could be reduced to a game among exactly 3. Their results show this in a different way (I think: Convert the n-player game to its END-OF-THE-LINE instance, then convert that END-OF-THE-LINE instance into NASH for 3 players; a fixed point in this 3-player game can be converted back into a fixed point in the original game. Weird!). I think I should read the paper by Bibelus in the future.

The authors also reference a paper by Chen and Deng, that shows a much more surprising result: the circuits created by converting BROUWER to NASH never contain Multiply gates, and so can actually be colored using just _2_ colors. This implies that the problem of finding an equilibrium in any n-player game can be efficiently converted into the problem of finding an equilibrium in a 2-player game, which seems ludicrous on the surface, but makes sense the more I think about it.


# So What?

NASH is PPAD-Complete. PPAD looks hard (in fact, it seems there isn't even a good stochastic approximation algorithm right now?).

Practically, this means it's hard to predict how rational agents might play a game. Real-world games are pretty complex beasts (say, the global economy), so if our algorithms for solving them scale exponentially, then we probably can't do much of anything.

Much more important is a point the authors raise: if it's not tractable to find fixed points, then why would we suppose that agents would (could?) play strategies that lie at a fixed point? That is, if in general Nash equilibria cannot be found without exponential computational efforts, then does finding a Nash Equilibrium actually tell us much of anything? Maybe the whole solution concept is kind of useless.

The paper is also 8 years old. I haven't heard anything about efficient algorithms (approximate or otherwise) for NASH, but I do wonder what sort of work people have done since towards this. Probably a reverse citation search on the paper would make it pretty apparent.

I wonder too about the implications of quantum computers for PPAD. I know NP and BQP overlap, but are not subsets of one another. Where exactly does PPAD sit relative to BQP? It certainly _seems_ like the sort of thing that would be easy with something like Grover's algorithm, because we'd have a polynomial depth circuit to act as an oracle. However, we're not looking something that matches a signature, but something that's unchanged when it goes through the function. Have people worked on this topic? Maybe I should ask Chris Granade what he thinks about this.




#### Why did I pick this paper?

This is an older paper I've been meaning to read for a couple of years. One of the authors is Christos Papadimitriou. Papadimitriou does a lot of exciting work. The two things that come to my mind are co-inventing https://en.wikipedia.org/wiki/Price_of_anarchy[Price of Anarchy], a metric for the cost of having self-interested (and rational) individuals do as they please, rather than imposing a solution on all of them from some central authority. It's very useful in mechanism design, because it allows us to quantify the improvement in social utility from using a given mechanism, vs. allowing the agents to settle into whatever the usual Nash Equilibrium is for the game of interest. Papadimitriou has also done some work in genetic algorithms, which purports to show that mutation is entirely pointless in evolutionary search. I haven't read the paper on this yet. Perhaps it should be my next review.

#### Things I still don't understand

The authors say that NASH is probably not in NPC, because there's always a solution to NASH.

My understanding is that this is because the fundamental NP-Complete problem is https://en.wikipedia.org/wiki/Boolean_satisfiability_problem[SAT], and other problems are reduced to SAT. The authors of this paper give an argument that I found a bit handwavy for this. Basically, if we had an efficient way to translate SAT instances (i.e. answering the question of whether or not a particular boolean formula can ever output true), to instances of NASH (Finding an equilibrium where no player can gain more than some fixed amount $$\epsilon$$ by playing differently), then we could solve SAT instances by _guessing_ a solution to NASH, and checking whether that solution was a solution to SAT or not. The authors say it is easy to show that this could yield a non-deterministic algorithm for solving SAT efficiently, but I'm not sure I see how. Obviously solutions will always exist to NASH, since there must be at least one equilibrium with $$\epsilon=0$$ by Nash's theorem. But would _guessing_ really allow one to find solutions quickly? The authors don't say that an efficient non-deterministic algorithm is needed for NASH, but it seems to me like it must be. 

Anyhow, the point is, complexity theorists have some (inscrutable) reason for thinking that guess and check on NASH would allow efficient non-deterministic solutions to SAT. Therefore they suppose that reductions from SAT to NASH are unlikely to exist. I find this uncompelling, but I'm not a complexity theorist, and I certainly can't find such a reduction myself to contradict them! If Nash were NP-Complete then by definition, an instance of SAT could be turned into NASH efficiently _somehow_. 


My complaint about the presentation of END-OF-THE-LINE's encoding is that it seems like a pretty contrived way to represent a graph. I guess it's saying: if you can only observe the local topology of a graph (like if you were trapped in a maze), you might have to check every path to find the exit. But the work is still linear in the size of the graph. It's only exponential in $k$, and $k$ just seems like a contrived notion. Mostly the design seems to be a theoretical contrivance, insofar as some other hard problems with input sizes of $k$ can be reduced to END-OF-THE-LINE with $$2^k$$ vertices. I kind of wish they'd said so when presenting END-OF-THE-LINE, but I suppose this is the cost of reading outside one's core area.

