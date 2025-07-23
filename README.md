<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>AI StudyPal Assistant</title>
<style>
  body {
    margin: 0;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background: #000;
    color: #00e0ff;
    overflow-x: hidden;
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 20px;
  }
  .background {
    position: fixed;
    top: 0; left: 0; right: 0; bottom: 0;
    z-index: -1;
    overflow: hidden;
    background: radial-gradient(circle at center, #001f3f 0%, #000 80%);
  }
  .techlines {
    position: absolute;
    width: 200%; height: 200%;
    background: repeating-linear-gradient(
      45deg,
      #00ffff33,
      #00ffff33 2px,
      transparent 2px,
      transparent 10px
    );
    animation: techMove 40s linear infinite;
    opacity: 0.05;
  }
  @keyframes techMove {
    0% { background-position: 0 0; }
    100% { background-position: 400px 400px; }
  }
  .circle {
    position: absolute;
    border-radius: 50%;
    background: rgba(0, 255, 255, 0.1);
    box-shadow: 0 0 20px rgba(0, 255, 255, 0.4);
    animation: float 25s ease-in-out infinite;
  }
  .circle:nth-child(1) {
    width: 120px; height: 120px; top: 15%; left: 10%;
    animation-delay: 0s;
  }
  .circle:nth-child(2) {
    width: 180px; height: 180px; top: 65%; left: 25%;
    animation-delay: 7s;
  }
  .circle:nth-child(3) {
    width: 100px; height: 100px; top: 40%; left: 80%;
    animation-delay: 4s;
  }
  .circle:nth-child(4) {
    width: 140px; height: 140px; top: 80%; left: 70%;
    animation-delay: 12s;
  }
  @keyframes float {
    0%, 100% { transform: translateY(0) translateX(0) rotate(0deg);}
    50% { transform: translateY(-25px) translateX(10px) rotate(180deg);}
  }
  h1 {
    margin-bottom: 10px;
    font-weight: 700;
    font-size: 2.5rem;
    text-align: center;
    color: #00e0ff;
    text-shadow: 0 0 10px #00e0ff;
  }
  .input-group {
    background: rgba(0, 255, 255, 0.05);
    padding: 20px 25px;
    border-radius: 15px;
    max-width: 400px;
    width: 100%;
    box-shadow: 0 0 20px #00e0ff55;
    margin-bottom: 20px;
  }
  label {
    display: block;
    margin-bottom: 6px;
    font-weight: 600;
    color: #00e0ff;
  }
  select, button {
    width: 100%;
    padding: 10px;
    margin-bottom: 15px;
    border-radius: 10px;
    border: none;
    font-size: 1rem;
  }
  select {
    background: #000;
    color: #00e0ff;
    border: 1.5px solid #00e0ff;
  }
  button {
    background: #00e0ff;
    color: #000;
    font-weight: 700;
    cursor: pointer;
    transition: background 0.3s ease;
  }
  button:hover {
    background: #00b2cc;
  }
  .result {
    background: rgba(0, 224, 255, 0.1);
    border-radius: 15px;
    max-width: 700px;
    padding: 20px;
    color: #d0f0ff;
    box-shadow: 0 0 30px #00e0ff77;
    font-size: 1.1rem;
    white-space: pre-line;
  }
  .label {
    font-weight: 700;
    margin-bottom: 8px;
    color: #00ffff;
    font-size: 1.2rem;
  }
  .gif {
    margin-top: 20px;
    text-align: center;
  }
  .gif img {
    width: 100px;
    height: auto;
    filter: drop-shadow(0 0 5px #00e0ff);
  }
  .btn-group {
    max-width: 400px;
    width: 100%;
    display: flex;
    gap: 15px;
    margin-bottom: 30px;
  }
  .btn-group button {
    flex: 1;
  }
</style>
</head>
<body>

<div class="background">
  <div class="techlines"></div>
  <div class="circle"></div>
  <div class="circle"></div>
  <div class="circle"></div>
  <div class="circle"></div>
</div>

<h1>AI StudyPal Assistant</h1>

<div class="input-group">
  <label for="mood">Select Your Mood:</label>
  <select id="mood">
    <option value="motivated">Motivated</option>
    <option value="tired">Tired</option>
    <option value="anxious">Anxious</option>
    <option value="distracted">Distracted</option>
    <option value="curious">Curious</option>
    <option value="stressed">Stressed</option>
    <option value="relaxed">Relaxed</option>
  </select>

  <label for="subject">Select Your Subject:</label>
  <select id="subject">
    <option value="math">Mathematics</option>
    <option value="physics">Physics</option>
    <option value="medical coding">Biology</option>
    <option value="history">History</option>
    <option value="cs">Computer Science</option>
    <option value="english">English</option>
    <option value="economics">Economics</option>
  </select>

  <button id="generateBtn" onclick="generatePlan()">Generate Study Plan</button>
</div>

<div class="btn-group" style="display:none;" id="actionButtons">
  <button id="downloadBtn" onclick="downloadPlan()">Download Plan</button>
  <button id="regenerateBtn" onclick="regeneratePlan()">Regenerate Plan</button>
</div>

<div id="result" class="result" aria-live="polite" role="region"></div>

<div class="gif" id="gifContainer"></div>

<script>
  let currentPlanText = "";

  function generatePlan() {
    const mood = document.getElementById("mood").value;
    const subject = document.getElementById("subject").value;

    const moodTips = {
      motivated: "You're on fire! Set clear goals and challenge yourself today. 💪",
      tired: "Take a short nap or walk, then study in 25-minute focus blocks. 💤",
      anxious: "Start with easier topics to build confidence. Breathe deeply. 🧘‍♂️",
      distracted: "Turn off notifications, use noise-cancelling sounds, or try Pomodoro technique. ⏱️",
      curious: "Explore the subject deeply, watch videos or go beyond the syllabus. 🌌",
      stressed: "Break your study into small tasks and reward yourself. 🎯",
      relaxed: "Perfect time to revise past lessons or try mock tests. 🧠"
    };

    const quotes = {
      motivated: "\"Work hard in silence, let success make the noise.\" – Frank Ocean",
      tired: "\"A little progress each day adds up to big results.\" – Satya Nani",
      anxious: "\"You don’t have to control your thoughts; you just have to stop letting them control you.\" – Dan Millman",
      distracted: "\"The successful warrior is the average man, with laser-like focus.\" – Bruce Lee",
      curious: "\"The important thing is not to stop questioning.\" – Albert Einstein",
      stressed: "\"It’s not stress that kills us, it is our reaction to it.\" – Hans Selye",
      relaxed: "\"Almost everything will work again if you unplug it for a few minutes… Including you.\" – Anne Lamott"
    };

    const comedyScenes = {
      motivated: "Comedy Scene: \"Superstar style, ‘Poda poda! Study panna time illa!’\" – Rajinikanth vibes to boost energy!",
      tired: "Comedy Scene: \"Thanni kettudhu, but study kekka enna power!\" – Vadivelu on tiredness struggles!",
      anxious: "Comedy Scene: \"Dei parama padii daa\" – Goundamani’s stress relief style!",
      distracted: "Comedy Scene: \"Ennamo panni irukken, seri seri concentrate pannunga!\" – Santhanam's distraction comedy!",
      curious: "Comedy Scene: \"Eppadi irukka? Yenna pannanum nu kekkura?\" – Vivek’s curious cat moments!",
      stressed: "Comedy Scene: \"Stress ah? Chill panna, naan unakku comedy thara poren!\" – Senthil’s stress buster lines!",
      relaxed: "Comedy Scene: \"Vera level relax panra madhiri, inga ellarum onnum tension illa!\" – Goundamani’s chill talk!"
    };

    const subjectPlans = {
      math: "Solve 5 problems from each chapter. Focus on weak areas like calculus or algebra. Practice mental math daily.",
      physics: "Review formulas and practice derivations. Conduct simple experiments or watch simulation videos.",
      biology: "Make detailed flashcards. Draw diagrams. Group study helps in remembering facts.",
      history: "Create timelines for events. Use mnemonic devices. Discuss historical impacts with peers.",
      cs: "Code daily, solve algorithm problems. Watch tutorials on new tech. Build mini projects.",
      english: "Read articles, practice grammar exercises. Improve vocabulary through apps or books.",
      economics: "Understand concepts deeply, read case studies. Stay updated with current economic news."
    };

    const tips = {
      math: `• Break problems into smaller steps\n• Practice daily for retention\n• Use online resources like Khan Academy`,
      physics: `• Understand concepts rather than memorizing\n• Use practical examples\n• Solve previous year question papers`,
      biology: `• Use visual aids like charts\n• Group discussions to clarify doubts\n• Regular revisions for long-term memory`,
      history: `• Storytelling technique to remember dates\n• Connect events with current affairs\n• Practice writing answers`,
      cs: `• Code along with tutorials\n• Join coding communities\n• Participate in hackathons`,
      english: `• Practice speaking and writing\n• Listen to English podcasts\n• Use flashcards for vocabulary`,
      economics: `• Follow economic news daily\n• Practice diagram drawing\n• Discuss theories with peers`
    };

    let planText = `Mood: ${capitalize(mood)}\nSubject: ${capitalize(subject)}\n\n--- Study Plan ---\n${subjectPlans[subject]}\n\n--- Tips ---\n${tips[subject]}\n\n--- Motivation ---\n${moodTips[mood]}\n\nQuote:\n${quotes[mood]}`;

    planText += `\n\n${comedyScenes[mood]}`;

    currentPlanText = planText;

    const resultDiv = document.getElementById("result");
    resultDiv.textContent = planText;

    document.getElementById("actionButtons").style.display = "flex";

    showGif(mood);
  }

  function capitalize(text) {
    return text.charAt(0).toUpperCase() + text.slice(1);
  }

  function showGif(mood) {
    const gifs = {
      motivated: "https://media.giphy.com/media/3o6Zt481isNVuQI1l6/giphy.gif",
      tired: "https://media.giphy.com/media/l0MYt5jPR6QX5pnqM/giphy.gif",
      anxious: "https://media.giphy.com/media/l0MYt5jPR6QX5pnqM/giphy.gif",
      distracted: "https://media.giphy.com/media/3ohzdIuqJoo8QdKlnW/giphy.gif",
      curious: "https://media.giphy.com/media/xT0GqeSlGSRQutn3WM/giphy.gif",
      stressed: "https://media.giphy.com/media/3og0IPxMM0erATueVW/giphy.gif",
      relaxed: "https://media.giphy.com/media/3o7TKtnuHOHHUjR38Y/giphy.gif"
    };

    const gifContainer = document.getElementById("gifContainer");
    gifContainer.innerHTML = `<img src="${gifs[mood]}" alt="Motivational GIF" />`;
  }

  function downloadPlan() {
    if (!currentPlanText) return alert("Please generate a plan first.");

    const blob = new Blob([currentPlanText], { type: 'text/plain' });
    const link = document.createElement('a');
    link.download = 'StudyPlan.txt';
    link.href = window.URL.createObjectURL(blob);
    link.click();
  }

  function regeneratePlan() {
    currentPlanText = "";
    document.getElementById("result").textContent = "";
    document.getElementById("gifContainer").innerHTML = "";
    document.getElementById("actionButtons").style.display = "none";

    document.getElementById("mood").value = "motivated";
    document.getElementById("subject").value = "math";
  }
</script>

</body>
</html>
