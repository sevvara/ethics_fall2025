---
title: "Activity"
layout: single 

collection: casestudy
permalink: /casestudy/activity/
author_profile: true
#nav_order: 3
---
This interactive experiment tests your reaction time and decision-making when faced with different types of cookie banners.

**The Hypothesis:** Research suggests that users do not read cookie banners; they merely react to visual cues. [cite_start]Dark patterns exploit this by making the "Accept" action significantly faster and physically easier to perform than the "Reject" action. [cite: 160, 161]

### Instructions
1. Click **"Start Experiment"** below.
2. You will be presented with **5 random cookie banners**.
3. Your goal is to **REJECT** non-essential cookies or **MANAGE** preferences whenever possible.
4. The system will track your reaction time and your choices.

<div id="game-container" style="border: 1px solid #ccc; background: #f9f9f9; padding: 20px; border-radius: 8px; margin: 20px 0; min-height: 300px; text-align: center;">

  <div id="intro-screen">
    <h3>Ready to test your autonomy?</h3>
    <p>Try to REJECT tracking on the following banners.</p>
    <br>
    <button id="start-btn" class="btn btn--primary" style="font-size: 1.2em;">Start Experiment</button>
  </div>

  <div id="game-screen" style="display: none;">
    <div id="banner-box" style="background: white; padding: 30px; border-radius: 5px; box-shadow: 0 5px 15px rgba(0,0,0,0.1); max-width: 500px; margin: 0 auto;">
      </div>
  </div>

  <div id="result-screen" style="display: none;">
    <h3>Test Complete</h3>
    <h1 id="score-display" style="color: #2c3e50; margin: 10px 0;">0.0s</h1>
    <p>Average Reaction Time</p>
    <div id="analysis-text" style="text-align: left; margin-top: 20px;"></div>
    <br>
    <button onclick="location.reload()" class="btn">Try Again</button>
  </div>

</div>

<style>
  /* Custom styles for the game elements to prevent theme conflicts */
  .game-option-btn {
    padding: 10px 20px;
    margin: 5px;
    border-radius: 4px;
    cursor: pointer;
    font-size: 14px;
    border: 1px solid #ccc;
    background: #e1e1e1;
    color: #333;
  }
  
  /* Dark Pattern Styles */
  .highlighted { background-color: #2980b9; color: white; font-weight: bold; border-color: #2980b9; }
  .hidden-link { background: none; border: none; text-decoration: underline; color: #7f8c8d; font-size: 12px; }
  
  /* Prevent code highlighting on game text */
  #banner-box p { font-size: 16px; margin-bottom: 20px; }
</style>

<script>
(function() {
  var rounds = 0;
  var maxRounds = 5;
  var startTime = 0;
  var results = [];
  
  var scenarios = [
    { text: "We use cookies for functionality.", yes: "Accept", no: "Reject", type: "neutral" },
    { text: "Agree to our terms to continue?", yes: "I AGREE", no: "Settings", type: "dark" },
    { text: "Enable personalized ads?", yes: "Yes, enable", no: "No, thanks", type: "dark" },
    { text: "Privacy Update: Partners List Modified.", yes: "Accept & Close", no: "Manage Options", type: "friction" },
    { text: "Enhance your experience!", yes: "OK", no: "More Info", type: "dark" }
  ];

  var startBtn = document.getElementById('start-btn');
  var introScreen = document.getElementById('intro-screen');
  var gameScreen = document.getElementById('game-screen');
  var bannerBox = document.getElementById('banner-box');
  var resultScreen = document.getElementById('result-screen');

  if(startBtn) {
    startBtn.addEventListener('click', function() {
      introScreen.style.display = 'none';
      gameScreen.style.display = 'block';
      nextRound();
    });
  }

  function nextRound() {
    if(rounds >= maxRounds) {
      showResults();
      return;
    }

    var data = scenarios[rounds];
    var yesClass = "game-option-btn";
    var noClass = "game-option-btn";

    // Apply Dark Patterns
    if(data.type === "dark" || data.type === "friction") {
      yesClass += " highlighted";
      noClass = (data.type === "dark") ? "hidden-link" : "game-option-btn";
    }

    // Randomize Button Order
    var btnsHTML = "";
    if(Math.random() > 0.5) {
      btnsHTML = `<button class="${yesClass}" onclick="record('accept')">${data.yes}</button>
                  <button class="${noClass}" onclick="record('reject')">${data.no}</button>`;
    } else {
      btnsHTML = `<button class="${noClass}" onclick="record('reject')">${data.no}</button>
                  <button class="${yesClass}" onclick="record('accept')">${data.yes}</button>`;
    }

    bannerBox.innerHTML = `<p>${data.text}</p><div>${btnsHTML}</div>`;
    
    // Explicitly attach functions to window for inline onclick to work in this scope
    window.record = function(choice) {
      var time = (Date.now() - startTime) / 1000;
      results.push({c: choice, t: time});
      rounds++;
      nextRound();
    };

    startTime = Date.now();
  }

  function showResults() {
    gameScreen.style.display = 'none';
    resultScreen.style.display = 'block';

    var totalTime = results.reduce((a,b) => a + b.t, 0);
    var avg = (totalTime / maxRounds).toFixed(2);
    var rejects = results.filter(r => r.c === 'reject').length;

    document.getElementById('score-display').innerText = avg + "s";
    
    var msg = `<strong>Analysis:</strong> You rejected tracking <strong>${rejects}/${maxRounds}</strong> times.<br><br>`;
    if(avg < 1.5) msg += "Your fast reaction suggests you were relying on visual cues (System 1 thinking).";
    else msg += "You took your time, likely fighting the interface friction.";

    document.getElementById('analysis-text').innerHTML = msg;
  }
})();
</script>
## Reflection

**Did you feel the friction?** In the activity above, you likely noticed that identifying the "Reject" option became progressively harder. This was not accidental; it was a simulation of **Friction Asymmetry**.

**Key Concept: Structural Consent Failure**
If you found yourself clicking "Accept" just to make the test end faster, you experienced **cognitive fatigue**. [cite_start]In the real world, this leads to what researchers call "structural consent failure"—where the system is designed to extract consent rather than request it. [cite: 173]
{: .notice}