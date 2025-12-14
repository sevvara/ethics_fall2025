---
title: "Interactive Activity: How Fast Do You Accept Cookies?"
author_profile: true
layout: default
---
<style>
  #game-wrapper {
    font-family: sans-serif;
    max-width: 800px;
    margin: 20px auto;
    padding: 20px;
    background: #f8f9fa; /* Light grey background */
    border: 1px solid #ddd;
    border-radius: 8px;
    position: relative;
    min-height: 400px;
  }

  .hidden { display: none !important; }

  /* Overlay that covers the game area */
  #banner-overlay {
    position: absolute;
    top: 0; left: 0; width: 100%; height: 100%;
    background: rgba(0,0,0,0.6);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 10;
    border-radius: 8px;
  }

  /* The "Popup" box */
  .cookie-card {
    background: white;
    width: 90%;
    max-width: 500px;
    padding: 30px;
    border-radius: 8px;
    box-shadow: 0 4px 15px rgba(0,0,0,0.3);
    text-align: center;
  }

  .cookie-text { font-size: 16px; margin-bottom: 25px; color: #333; }

  /* Button Styles */
  .game-btn {
    padding: 12px 24px;
    margin: 8px;
    cursor: pointer;
    border-radius: 4px;
    font-size: 14px;
    border: none;
    transition: all 0.2s;
  }

  /* The "Trap" styles */
  .style-neutral { background: #e2e6ea; color: #333; border: 1px solid #cbd3da; }
  .style-primary { background-color: #007bff; color: white; font-weight: bold; box-shadow: 0 2px 4px rgba(0,0,0,0.2); }
  .style-link    { background: none; color: #6c757d; text-decoration: underline; font-size: 12px; padding: 5px; }
  
  /* Start Screen Styles */
  #start-screen { text-align: center; padding-top: 60px; }
  #start-btn { 
    background-color: #28a745; 
    color: white; 
    font-size: 18px; 
    padding: 15px 30px; 
    border: none; 
    border-radius: 5px; 
    cursor: pointer;
  }
  #start-btn:hover { background-color: #218838; }

</style>

<div id="game-wrapper">
  
  <div id="start-screen">
    <h2>Ready to test your autonomy?</h2>
    <p style="margin-bottom: 20px;">Try to <strong>REJECT</strong> tracking on the following banners.</p>
    <button id="start-btn">Begin Experiment</button>
  </div>

  <div id="banner-overlay" class="hidden">
    <div id="cookie-card" class="cookie-card">
      </div>
  </div>

  <div id="results-screen" class="hidden" style="text-align: center; padding-top: 40px;">
    <h3>Test Complete</h3>
    <div style="background: white; padding: 20px; border-radius: 8px; margin: 20px 0;">
      <h4>Average Reaction Time:</h4>
      <h2 id="score-time" style="color: #007bff; font-size: 3em; margin: 10px 0;">0.0s</h2>
      <p id="score-text"></p>
    </div>
    <div id="analysis-output" style="text-align: left; font-size: 14px; line-height: 1.6;"></div>
    <button class="game-btn style-neutral" onclick="location.reload()">Try Again</button>
  </div>

</div>

<script>
  (function() {
    // Game Variables
    let round = 0;
    let startTime = 0;
    let results = [];
    const maxRounds = 5;

    // Scenarios (The Data)
    const scenarios = [
      { text: "We use cookies for functionality.", yes: "Accept", no: "Reject", type: "neutral" },
      { text: "We value your privacy. Agree to continue.", yes: "I AGREE", no: "Settings", type: "visual" },
      { text: "Get the full personalized experience?", yes: "Yes, please!", no: "No, thanks", type: "framing" },
      { text: "Privacy Update: We have updated our partners.", yes: "Accept & Close", no: "Manage Options", type: "friction" },
      { text: "Enhance your experience with tailored ads.", yes: "OK", no: "More Info", type: "visual" }
    ];

    // Safe Start Function
    function initGame() {
      const startBtn = document.getElementById('start-btn');
      if(startBtn) {
        startBtn.addEventListener('click', function() {
          document.getElementById('start-screen').classList.add('hidden');
          document.getElementById('banner-overlay').classList.remove('hidden');
          round = 0;
          results = [];
          nextRound();
        });
      }
    }

    // Run the round
    function nextRound() {
      if (round >= maxRounds) {
        endGame();
        return;
      }

      const data = scenarios[round];
      const card = document.getElementById('cookie-card');
      
      // Determine button styles based on "Dark Pattern" type
      let btnYesClass = "style-neutral";
      let btnNoClass = "style-neutral";

      if (data.type === 'visual' || data.type === 'friction') {
        btnYesClass = "style-primary"; // Bright Blue
        btnNoClass = "style-link";     // Hidden link
      }

      // Randomize Left/Right positions to prevent muscle memory
      const showYesFirst = Math.random() > 0.5;

      const btnYesHTML = `<button class="game-btn ${btnYesClass}" data-choice="accept">${data.yes}</button>`;
      const btnNoHTML = `<button class="game-btn ${btnNoClass}" data-choice="reject">${data.no}</button>`;

      card.innerHTML = `
        <p class="cookie-text">${data.text}</p>
        <div style="display:flex; justify-content:center; align-items:center; flex-wrap:wrap;">
          ${showYesFirst ? btnYesHTML + btnNoHTML : btnNoHTML + btnYesHTML}
        </div>
      `;

      // Add click listeners to the new buttons
      const buttons = card.querySelectorAll('button');
      buttons.forEach(btn => {
        btn.addEventListener('click', function() {
          recordChoice(this.getAttribute('data-choice'));
        });
      });

      startTime = Date.now();
      round++;
    }

    function recordChoice(choice) {
      const timeTaken = (Date.now() - startTime) / 1000;
      results.push({ choice: choice, time: timeTaken });
      nextRound();
    }

    function endGame() {
      document.getElementById('banner-overlay').classList.add('hidden');
      document.getElementById('results-screen').classList.remove('hidden');

      const totalTime = results.reduce((a, b) => a + b.time, 0);
      const avg = (totalTime / maxRounds).toFixed(2);
      const rejects = results.filter(r => r.choice === 'reject').length;

      document.getElementById('score-time').innerText = avg + "s";
      document.getElementById('score-text').innerText = `You rejected tracking ${rejects} out of ${maxRounds} times.`;
      
      let analysis = "<p><strong>Analysis:</strong> ";
      if(avg < 1.5) analysis += "You reacted very quickly (System 1 thinking), making you susceptible to visual manipulation. ";
      [cite_start]if(rejects < 3) analysis += "Despite your goal, the design nudged you into accepting cookies you didn't want (Structural Consent Failure). [cite: 55]";
      else analysis += "You successfully resisted the design, likely by paying a 'time tax'—taking longer to find the hidden buttons.";
      
      document.getElementById('analysis-output').innerHTML = analysis + "</p>";
    }

    // Initialize when page loads
    if (document.readyState === 'loading') {
      document.addEventListener('DOMContentLoaded', initGame);
    } else {
      initGame();
    }
  })();
</script>

[Return to Case Study Home](../) | [Next: Discussion Questions](../discussion/)

[↩ Back to main page](https://sevvara.github.io/ethics_fall2025/casestudy/)

