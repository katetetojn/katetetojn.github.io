---
layout: post
---
What are the odds of the players surviving Squid Game's glass bridge challenge, if they actually followed the rules? By this I mean players go in the exact order as they are assigned, so no jumping ahead, refusing to go, or pushing others off. I created a statistical simulation of the glass bridge challenge in Python with the following rules:

1. 16 players are assigned 1 to 16, and they cross the bridge in that order;
2. The bridge has 18 steps, each with 2 glass panels. At each step, the player at the front of the line ("current player") has 50% chance to choose the right panel, otherwise they are eliminated and player behind them become the current player;
3. Once a player has attempted a step, all players will safely pass that step;
4. The game is over when
    - at least one player has passed, or
    - all players have been eliminated.
5. Time limit is disregarded for simplicity.

(There are other possible collaborative strategies. For example, players may decide to rotate, i.e., player 1 takes the first try, player 2 takes the second, etc. I will consider them in the future.)