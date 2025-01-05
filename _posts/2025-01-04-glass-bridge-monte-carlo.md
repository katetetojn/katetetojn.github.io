---
layout: post
---
**What are the odds of the players surviving Squid Game's glass bridge challenge, if they actually followed the rules?** By this I mean players go in the exact order as they are assigned, so no jumping ahead, refusing to go, or pushing others off. I created a statistical simulation of the glass bridge challenge in Python with the following rules:

1. 16 players are assigned 1 to 16, and they cross the bridge in that order;
2. The bridge has 18 steps, each with 2 glass panels. At each step, the player at the front of the line ("current player") has 50% chance to choose the right panel, otherwise they are eliminated and player behind them become the current player;
3. Once a player has attempted a step, all players will safely pass that step;
4. The game is over when
    - at least one player has passed, or
    - all players have been eliminated.
5. Time limit is disregarded for simplicity.

Here are key findings from 1 million simulations:

**On average, 7 players survived the challenge.** This is much more than the 3 survivors in the show! In fact, the probability that 3 or fewer players survive is only about 1.6%, if they followed the rules! On the other hand, the probability that at least 1 player (i.e., the last player) survives is over 99.9%, so [Seong Gi-hun](https://en.wikipedia.org/wiki/List_of_Squid_Game_characters#Seong_Gi-hun), who chose number 16, was quite right that he didn't have much to worry about.

You can move the slider below to see the probabilities yourself.

<div id="slider-container">
  <input type="range" id="number-slider" min="0" max="100" value="50" />
  <p>Current Value: <span id="slider-value">50</span></p>
</div>

<script>
  // JavaScript to update the displayed number dynamically
  document.addEventListener('DOMContentLoaded', function() {
    const slider = document.getElementById('number-slider');
    const valueDisplay = document.getElementById('slider-value');

    slider.addEventListener('input', function() {
      valueDisplay.textContent = slider.value;
    });
  });
</script>

You can also find an exhaustive table of probabilities in the end.

There are other possible collaborative strategies. For example, players may decide to rotate, i.e., player 1 takes the first try, player 2 takes the second, etc. I will consider them in the future.

*Appendix: Table of Probabilities*

| # of Survivors (k) | P(survivors ≥ k) | P(survivors < k) |
|----------------------|------------------|------------------|
| 1  | 0.999350 | 0.000650 |
| 2  | 0.996189 | 0.003811 |
| 3  | 0.984360 | 0.015640 |
| 4  | 0.951677 | 0.048323 |
| 5  | 0.881019 | 0.118981 |
| 6  | 0.759753 | 0.240247 |
| 7  | 0.592996 | 0.407004 |
| 8  | 0.407557 | 0.592443 |
| 9  | 0.241016 | 0.758984 |
| 10 | 0.119258 | 0.880742 |
| 11 | 0.048316 | 0.951684 |
| 12 | 0.015456 | 0.984544 |
| 13 | 0.003825 | 0.996175 |
| 14 | 0.000677 | 0.999323 |
| 15 | 0.000069 | 0.999931 |
| 16 | 0.000002 | 0.999998 |