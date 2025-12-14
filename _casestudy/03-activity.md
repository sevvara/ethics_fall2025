---
title: "Ethics"
layout: single 

collection: casestudy
permalink: /casestudy/activity/
author_profile: true
#nav_order: 3
---

This interactive experiment tests your reaction time and decision-making when faced with different types of cookie banners.

**The Hypothesis:** Research suggests that users do not read cookie banners; they merely react to visual cues. [cite_start]Dark patterns exploit this by making the "Accept" action significantly faster and physically easier to perform than the "Reject" action[cite: 160, 161].

### Instructions
1. Click **"Start Experiment"** below.
2. You will be presented with **5 random cookie banners**.
3. Your goal is to **REJECT** non-essential cookies or **MANAGE** preferences whenever possible.
4. The system will track your reaction time and your choices.

<div id="game-container" style="border: 1px solid #ccc; background-color: #ffffff !important; color: #222222 !important; padding: 30px; border-radius: 8px; margin: 20px 0; min-height: 350px; text-align: center; box-shadow: 0 4px 10px rgba(0,0,0,0.1);">

  <div id="intro-screen">
    <h3 style="color: #222222 !important; margin-top: 0;">Ready to test your autonomy?</h3>
    <p style="color: #555555 !important;">Try to <strong>REJECT</strong> tracking on the following banners.</p>
    <br>
    <button id="start-btn" class="btn btn--primary" style="font-size: 1.2em;">Start Experiment</button>
  </div>

  <div id="game-screen" style="display: none;">
    <div id="banner-box" style="background: #fdfdfd !important; padding: 30px; border: 1px solid #eee; border-radius: 5px; box-shadow: 0 5px 15px rgba(0,0,0,0.1); max-width: 500px; margin: 0 auto; color: #333 !important;">
      </div>
  </div>

  <div id="result-screen" style="display: none;">
    <h3 style="color: #222222 !important;">Test Complete</h3>
    <h1 id="score-display" style="color: #2c3e50 !important; margin: 10px 0; font-size: 3em;">0.0s</h1>
    <p style="color: #555555 !important;">Average Reaction Time</p>
    <div id="analysis-text" style="text-align: left; margin-top: 20px; color: #333 !important;"></div>
    <br>
    <button onclick="location.reload()" class="btn">Try Again</button>
  </div>

</div>

<style>
  /* Force text colors inside the game to stay dark even in dark mode */
  #game-container h3, 
  #game-container p, 
  #game-container div,
  #banner-box p {
    color: #222222 !important;
  }

  /* Game Buttons */
  .game-option-btn {
    padding: 10px 20px;
    margin: 5px;
    border-radius: 4px;
    cursor: pointer;
    font-size: 14px;
    border: 1px solid #ccc;
    background: #e1e1e1 !important; /* Force light grey */
    color: #333 !important; /* Force dark text */
  }
  
  /* Dark Pattern Styles */
  .highlighted { 
    background-color: #2980b9 !important; 
    color: white !important; 
    border-color: #2980b9 !important; 
  }
  .hidden-link { 
    background: transparent !important; 
    border: none; 
    text-decoration: underline; 
    color: #7f8c8d !important; 
    font-size: 12px; 
  }
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
If you found yourself clicking "Accept" just to make the test end faster, you experienced **cognitive fatigue**. In the real world, this leads to what researchers call "structural consent failure"—where the system is designed to extract consent rather than request it.
{: .notice}