---
title: "Interactive Activity: How Fast Do You Accept Cookies?"
author_profile: true
layout: default
---
# Lab: The "Accept All" Reflex Test

This interactive experiment tests your reaction time and decision-making when faced with different types of cookie banners.

**The Hypothesis:** Research suggests that users do not read cookie banners; they merely react to visual cues. [cite_start]Dark patterns exploit this by making the "Accept" action significantly faster and physically easier to perform than the "Reject" action[cite: 42, 62].

### Instructions
1.  Click **"Start Test"** below.
2.  You will be presented with **5 random cookie banners**.
3.  Your goal is to **REJECT** non-essential cookies or **MANAGE** preferences whenever possible.
4.  The system will track your reaction time and your choices.

<style>
  /* Game Container Styles */
  #game-container {
    border: 2px solid #e0e0e0;
    border-radius: 8px;
    background-color: #f9f9f9;
    padding: 20px;
    margin: 20px 0;
    min-height: 400px;
    position: relative;
    font-family: sans-serif;
    box-shadow: 0 4px 6px rgba(0,0,0,0.1);
  }

  .hidden { display: none !important; }

  /* Overlay for the popup simulation */
  #banner-overlay {
    position: absolute;
    top: 0; left: 0; width: 100%; height: 100%;
    background: rgba(0,0,0,0.5);
    display: flex;
    justify-content: center;
    align-items: center;
    border-radius: 6px;
  }

  /* The Cookie Banner Itself */
  .cookie-banner {
    background: white;
    width: 80%;
    padding: 20px;
    border-radius: 5px;
    box-shadow: 0 10px 25px rgba(0,0,0,0.2);
    text-align: center;
  }

  .banner-text { margin-bottom: 20px; font-size: 14px; color: #333; }

  /* Button Styles - Dynamic classes for dark patterns */
  .btn-game {
    padding: 10px 20px;
    border-radius: 4px;
    cursor: pointer;
    margin: 5px;
    border: none;
    font-size: 14px;
  }

  /* "Neutral" Design */
  .btn-neutral { background: #ddd; color: #333; border: 1px solid #ccc; }
  
  /* "High Visibility" (The Trap) */
  .btn-primary { background-color: #007bff; color: white; font-weight: bold; transform: scale(1.05); }
  
  /* "Low Visibility" (The Hidden Option) */
  .btn-link { background: none; color: #666; text-decoration: underline; font-size: 12px; }

  #results-area { text-align: center; padding-top: 40px; }
  .stat-box { background: white; padding: 15px; margin: 10px; border: 1px solid #ddd; border-radius: 5px; }
</style>

<div id="game-container">
  <div id="start-screen" style="text-align: center; padding-top: 100px;">
    <h3>Ready to test your autonomy?</h3>
    <p>Try to <strong>REJECT</strong> tracking on the following banners.</p>
    <button class="btn-game btn-primary" onclick="startGame()">Start Test</button>
  </div>

  <div id="banner-overlay" class="hidden">
    <div id="banner-area" class="cookie-banner">
      </div>
  </div>

  <div id="results-area" class="hidden">
    <h3>Test Complete</h3>
    <div class="stat-box">
      <h4>Your Average Reaction Time:</h4>
      <h2 id="avg-time-display">0.0s</h2>
    </div>
    <div class="stat-box">
      <h4>Did you successfully Reject?</h4>
      <p id="compliance-score"></p>
    </div>
    <div id="analysis-text" style="text-align: left; margin-top: 20px;"></div>
    <button class="btn-game btn-neutral" onclick="location.reload()">Try Again</button>
  </div>
</div>

<script>
  // Game State
  let round = 0;
  let startTime = 0;
  let results = [];
  const maxRounds = 5;

  // The "Scenarios" simulating different dark patterns
  const scenarios = [
    {
      type: "neutral",
      text: "We use cookies for functionality. Please choose.",
      btnAccept: { text: "Accept All", class: "btn-neutral" },
      btnReject: { text: "Reject All", class: "btn-neutral" }
    },
    {
      type: "visual_interference",
      text: "We value your privacy. Agree to our terms to continue.",
      btnAccept: { text: "I AGREE", class: "btn-primary" },
      btnReject: { text: "Settings", class: "btn-link" }
    },
    {
      type: "framing",
      text: "Accept cookies to get the full personalized experience?",
      btnAccept: { text: "Yes, I love cookies!", class: "btn-primary" },
      btnReject: { text: "No, I hate fun", class: "btn-neutral" }
    },
    {
      type: "friction",
      text: "Privacy Update: We have updated our partners list.",
      btnAccept: { text: "Accept & Close", class: "btn-primary" },
      btnReject: { text: "Manage Vendor Preferences", class: "btn-neutral" } // In a real dark pattern, this would open a new confusing menu
    },
    {
      type: "visual_interference_2",
      text: "Enhance your experience with tailored ads.",
      btnAccept: { text: "OK", class: "btn-primary" },
      btnReject: { text: "More Info", class: "btn-link" }
    }
  ];

  function startGame() {
    document.getElementById('start-screen').classList.add('hidden');
    document.getElementById('banner-overlay').classList.remove('hidden');
    round = 0;
    results = [];
    nextRound();
  }

  function nextRound() {
    if (round >= maxRounds) {
      endGame();
      return;
    }

    // Pick a random scenario (or cycle through them)
    const scenario = scenarios[round];
    
    // Build HTML for the banner
    const html = `
      <p class="banner-text">${scenario.text}</p>
      <div style="display: flex; justify-content: center; align-items: center; gap: 10px; flex-wrap: wrap;">
        ${Math.random() > 0.5 
          ? `<button class="btn-game ${scenario.btnReject.class}" onclick="recordChoice('reject')">${scenario.btnReject.text}</button>
             <button class="btn-game ${scenario.btnAccept.class}" onclick="recordChoice('accept')">${scenario.btnAccept.text}</button>`
          : `<button class="btn-game ${scenario.btnAccept.class}" onclick="recordChoice('accept')">${scenario.btnAccept.text}</button>
             <button class="btn-game ${scenario.btnReject.class}" onclick="recordChoice('reject')">${scenario.btnReject.text}</button>`
        }
      </div>
    `;

    document.getElementById('banner-area').innerHTML = html;
    
    // Slight delay before measuring to simulate page load
    startTime = Date.now();
    round++;
  }

  function recordChoice(choice) {
    const endTime = Date.now();
    const reactionTime = (endTime - startTime) / 1000;
    
    results.push({
      choice: choice,
      time: reactionTime,
      scenarioType: scenarios[round-1].type
    });

    nextRound();
  }

  function endGame() {
    document.getElementById('banner-overlay').classList.add('hidden');
    document.getElementById('results-area').classList.remove('hidden');

    // Calculate Stats
    const totalTime = results.reduce((acc, curr) => acc + curr.time, 0);
    const avgTime = (totalTime / results.length).toFixed(2);
    const rejectCount = results.filter(r => r.choice === 'reject').length;

    // Display Stats
    document.getElementById('avg-time-display').innerText = avgTime + " seconds";
    document.getElementById('compliance-score').innerText = `You rejected tracking ${rejectCount} out of ${maxRounds} times.`;

    // Generate Analysis Text based on behavior
    let analysis = "";
    
    if (avgTime < 1.5) {
      analysis += "<p><strong>Fast Reaction:</strong> You clicked very quickly. Research indicates that at high speeds, users rely on 'System 1' thinking (intuition), making them highly susceptible to visual dark patterns[cite: 52].</p>";
    } else {
      analysis += "<p><strong>Deliberate Action:</strong> You took your time. This suggests you were fighting the 'Cognitive Fatigue' that most users succumb to[cite: 51].</p>";
    }

    if (rejectCount < 3) {
      analysis += "<p><strong>The Effectiveness of Dark Patterns:</strong> You accepted most tracking cookies. This often happens due to 'Default Bias'—users stick with the easiest option presented[cite: 60]. Even with the intention to reject, the visual highlighting of the 'Accept' button likely influenced your choice[cite: 63].</p>";
    } else {
      analysis += "<p><strong>Resisting the Design:</strong> You successfully navigated the dark patterns. In the real world, this often requires significantly more clicks (friction), which is why most users eventually give up[cite: 62].</p>";
    }

    document.getElementById('analysis-text').innerHTML = analysis;
  }
</script>

---

## Analysis: Why Did You Click That?

In the activity above, you likely noticed that identifying the "Reject" option became progressively harder. This was not accidental; it was a simulation of **Dark Patterns**.

### 1. Visual Interference
In several rounds, the "Accept" button was bright blue, while the "Reject" option was a dull grey or a small text link. This exploits **pre-attentive processing**—your eyes are drawn to the high-contrast element before you even read the text.
> [cite_start]"Users are far more likely to accept cookies when banners use color and placement to highlight the 'Accept All' button"[cite: 63].

### 2. Ambiguous Wording (Framing)
Did you hesitate on the banner that said *"No, I hate fun"*? This is known as **confirmshaming** or manipulative framing. [cite_start]It attempts to guilt the user into compliance by framing the privacy-preserving option as a negative character trait[cite: 41].

### 3. The Result: Structural Consent Failure
If you found yourself clicking "Accept" just to make the test end faster, you experienced **cognitive fatigue**. [cite_start]In the real world, this leads to what researchers call "structural consent failure"—where the system is designed to extract consent rather than request it[cite: 55].

[Return to Case Study Home](../) | [Next: Discussion Questions](../discussion/)

[↩ Back to main page](https://sevvara.github.io/ethics_fall2025/casestudy/)

