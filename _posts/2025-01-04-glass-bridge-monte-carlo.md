---
layout: post
---

**What are the odds of the players surviving Squid Game's glass bridge challenge, if they actually followed the rules?**

<style type="text/css">
.lined-box {
  border: 1px solid #ccc;
  padding: 10px;
  margin: 10px 0;
  border-radius: 5px;
  background-color: #f9f9f9;
}
</style>

By this I mean players go in the exact order as they are assigned, so no jumping ahead, refusing to go, or pushing others off. To answer this question, I created a statistical simulation of the glass bridge challenge in Python ([source code here](https://github.com/katetetojn/glass-bridge-monte-carlo)).

<!-- <div id="group-survival-line-chart"></div> -->
![survival probability line plot](/assets/2025-01-04-glass-bridge-monte-carlo-1.png)

**Here are the rules I used in the simulation.**

1. 16 players are assigned 1 to 16, and they cross the bridge in that order;
2. The bridge has 18 steps, each with 2 glass panels. At each step, the player at the front of the line ("current player") has 50% chance to choose the right panel, otherwise they are eliminated and player behind them become the current player;
3. Once a player has attempted a step, all players will safely pass that step;
4. The game is over when
   - at least one player has passed, or
   - all players have been eliminated.
5. Time limit is disregarded for simplicity.

**Here's what I found after running 1 million simulations.**

On average, 7 players survived the challenge. This is much more than the 3 survivors in the show! In fact, the probability that 3 or fewer players survive is only about 1.6%. On the other hand, the probability that at least 1 player (i.e., the last player) survives is over 99.94%, so [Seong Gi-hun](https://en.wikipedia.org/wiki/List_of_Squid_Game_characters#Seong_Gi-hun), who chose number 16, was quite right that he didn't have much to worry about.

<!-- <div id="player-survival-bar-chart"></div> -->
![survival probability bar plot](/assets/2025-01-04-glass-bridge-monte-carlo-2.png)

But of course, the survival probabilities of the first few players are so poor that they have no incentive to cooperate. What maximizes collective welfare does not necessarily maximize individual welfare. If they had an accurate assessment of their survival probabilities, they would have even less reason to follow the rules than in the show.

**Try it yourself!**

Change `k` with the slider below to see the probability that at least `k` players survive:

<div class="lined-box" id="prob-ge-k-slider-container">
  <label for="prob-ge-k-slider"># of survivors (k): </label>
  <input type="range" id="prob-ge-k-slider" min="1" max="16" value="7" step="1" />
  <p>The probability that at least <span id="prob-ge-k-value">7</span> players survive is <span id="prob-ge-k-display">59.30%</span>. Conversely, the probability that fewer than <span id="prob-ge-k-value-2">7</span> players survive is <span id="prob-ge-k-complement-display">40.70%</span>.</p>
</div>

Similarly, change `i` to see the survival probability for any individual player with number `i`

<div class="lined-box" id="prob-i-slider-container">
    <label for="prob-i-slider">player (i): </label>
    <input type="range" id="prob-i-slider" min="1" max="16" value="7" step="1" />
    <p>
      The probability that player <span id="prob-i-value">7</span> survives is <span id="prob-i-display">11.93%</span>.
      Conversely, the probability that they do not survive is <span id="prob-i-complement-display">88.07%</span>.
    </p>
</div>

Does anything surprise you?

**Appendix**

_Table 1: Group Survival Probabilities_

<table id="prob-ge-k-table">
  <thead>
    <tr>
      <th>k</th>
      <th>P(# of survivors ≥ k)</th>
      <th>P(# of survivors < k)</th>
    </tr>
  </thead>
  <tbody>
  </tbody>
</table>

_Table 2: Individual Player Survival Probabilities_

<table id="prob-i-table">
  <thead>
    <tr>
      <th>i</th>
      <th>P(player i survives)</th>
      <th>P(player i doesn't survive)</th>
    </tr>
  </thead>
  <tbody>
  </tbody>
</table>

<script src="https://d3js.org/d3.v7.min.js"></script>

<script>
	/* probs data */
  // P(# of survivors ≥ k)
  const probGeK = [
    0.999350, 0.996189, 0.984360, 0.951677, 0.881019, 
    0.759753, 0.592996, 0.407557, 0.241016, 0.119258, 
    0.048316, 0.015456, 0.003825, 0.000677, 0.000069, 0.000002
  ];

  // P(player i survives)
  const probsI = [
    0.000002, 0.000069, 0.000677, 0.003825, 0.015456, 
    0.048316, 0.119258, 0.241016, 0.407557, 0.592996, 
    0.759753, 0.881019, 0.951677, 0.984360, 0.996189, 0.999350
  ];

  document.addEventListener('DOMContentLoaded', function() {
		/* sliders */
    const probGeKSlider = document.getElementById('prob-ge-k-slider');
    const probGeKValue = document.getElementById('prob-ge-k-value');
    const probGeKValue2 = document.getElementById('prob-ge-k-value-2');
    const probGeKDisplay = document.getElementById('prob-ge-k-display');
    const probGeKComplementDisplay = document.getElementById('prob-ge-k-complement-display');
    const probGeKTableBody = document.querySelector('#prob-ge-k-table tbody');

    const probISlider = document.getElementById('prob-i-slider');
    const probIValue = document.getElementById('prob-i-value');
    const probIDisplay = document.getElementById('prob-i-display');
    const probIComplementDisplay = document.getElementById('prob-i-complement-display');
    const probITableBody = document.querySelector('#prob-i-table tbody');

    probGeKSlider.addEventListener('input', function() {
      const k = probGeKSlider.value;
      probGeKValue.textContent = k;
      probGeKValue2.textContent = k;
      const probK = (probGeK[k - 1] * 100).toFixed(2);
      const probKComplement = ((1 - probGeK[k - 1]) * 100).toFixed(2);
      probGeKDisplay.textContent = `${probK}%`;
      probGeKComplementDisplay.textContent = `${probKComplement}%`;
    });

    probISlider.addEventListener('input', function() {
      const i = probISlider.value;
      probIValue.textContent = i;
      const probI = (probsI[i - 1] * 100).toFixed(2);
      const probIComplement = ((1 - probsI[i - 1]) * 100).toFixed(2);
      probIDisplay.textContent = `${probI}%`;
      probIComplementDisplay.textContent = `${probIComplement}%`;
    });

    probGeK.forEach((p, index) => {
      const survivors = index + 1;
      const row = document.createElement('tr');
      row.innerHTML = `
        <td>${survivors}</td>
        <td>${(p * 100).toFixed(2)}%</td>
        <td>${((1 - p) * 100).toFixed(2)}%</td>
      `;
      probGeKTableBody.appendChild(row);
    });

    probsI.forEach((p, index) => {
      const player = index + 1;
      const row = document.createElement('tr');
      row.innerHTML = `
        <td>${player}</td>
        <td>${(p * 100).toFixed(2)}%</td>
        <td>${((1 - p) * 100).toFixed(2)}%</td>
      `;
      probITableBody.appendChild(row);
    });

    /* d3 visualizations */
	  // P(# of survivors ≥ k) line plot
    const groupData = probGeK.map((p, index) => ({ k: index + 1, prob: p }));
    const margin = { top: 20, right: 30, bottom: 40, left: 50 };
    const width = 600 - margin.left - margin.right;
    const height = 400 - margin.top - margin.bottom;

    const svgGroup = d3
      .select("#group-survival-line-chart")
      .append("svg")
      .attr("width", width + margin.left + margin.right)
      .attr("height", height + margin.top + margin.bottom)
      .append("g")
      .attr("transform", `translate(${margin.left},${margin.top})`);

    const xGroup = d3
      .scaleLinear()
      .domain([1, 16])
      .range([0, width]);

    const yGroup = d3.scaleLinear().domain([0, 1]).nice().range([height, 0]);

    const lineGroup = d3
      .line()
      .x(d => xGroup(d.k))
      .y(d => yGroup(d.prob));

    svgGroup
      .append("path")
      .datum(groupData)
      .attr("fill", "none")
      .attr("stroke", "steelblue")
      .attr("stroke-width", 2)
      .attr("d", lineGroup);

    svgGroup.append("g").attr("class", "x-axis").attr("transform", `translate(0,${height})`).call(d3.axisBottom(xGroup).ticks(16));
    svgGroup.append("g").attr("class", "y-axis").call(d3.axisLeft(yGroup).tickFormat(d3.format(".0%")));

    svgGroup
      .append("text")
      .attr("x", width / 2)
      .attr("y", height + margin.bottom - 5)
      .attr("text-anchor", "middle")
      .text("k");

    svgGroup
      .append("text")
      .attr("x", -height / 2)
      .attr("y", -margin.left + 15)
      .attr("text-anchor", "middle")
      .attr("transform", "rotate(-90)")
      .text("P(# of survivors ≥ k)");

    // P(player i survives) bar plot
    const playerData = probsI.map((p, index) => ({ i: index + 1, prob: p }));

    const svgPlayer = d3
      .select("#player-survival-bar-chart")
      .append("svg")
      .attr("width", width + margin.left + margin.right)
      .attr("height", height + margin.top + margin.bottom)
      .append("g")
      .attr("transform", `translate(${margin.left},${margin.top})`);

    const xPlayer = d3
      .scaleBand()
      .domain(playerData.map(d => d.i))
      .range([0, width])
      .padding(0.1);

    const yPlayer = d3.scaleLinear().domain([0, 1]).nice().range([height, 0]);

    svgPlayer
      .selectAll(".bar")
      .data(playerData)
      .enter()
      .append("rect")
      .attr("class", "bar")
      .attr("x", d => xPlayer(d.i))
      .attr("y", d => yPlayer(d.prob))
      .attr("width", xPlayer.bandwidth())
      .attr("height", d => height - yPlayer(d.prob))
      .attr("fill", "steelblue");

    svgPlayer.append("g").attr("class", "x-axis").attr("transform", `translate(0,${height})`).call(d3.axisBottom(xPlayer));
    svgPlayer.append("g").attr("class", "y-axis").call(d3.axisLeft(yPlayer).tickFormat(d3.format(".0%")));

    svgPlayer
      .append("text")
      .attr("x", width / 2)
      .attr("y", height + margin.bottom - 5)
      .attr("text-anchor", "middle")
      .text("i");

    svgPlayer
      .append("text")
      .attr("x", -height / 2)
      .attr("y", -margin.left + 15)
      .attr("text-anchor", "middle")
      .attr("transform", "rotate(-90)")
      .text("P(player i survives)");
  });  
</script>
