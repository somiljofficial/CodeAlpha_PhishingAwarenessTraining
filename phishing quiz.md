<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Phishing Awareness Quiz</title>
<style>
  body {
    font-family: Arial, sans-serif;
    max-width: 600px;
    margin: 40px auto;
    padding: 0 20px;
    color: #222;
  }
  h1 { font-size: 22px; }
  #meta {
    display: flex;
    justify-content: space-between;
    font-size: 13px;
    color: #666;
    margin-bottom: 12px;
  }
  #question {
    font-size: 16px;
    font-weight: bold;
    margin-bottom: 16px;
  }
  .option {
    display: block;
    width: 100%;
    text-align: left;
    padding: 10px 14px;
    margin-bottom: 8px;
    border: 1px solid #ccc;
    border-radius: 6px;
    background: #fff;
    cursor: pointer;
    font-size: 14px;
  }
  .option:hover { background: #f5f5f5; }
  .correct { background: #dff5e1 !important; border-color: #4caf50 !important; }
  .incorrect { background: #fde2e2 !important; border-color: #e53935 !important; }
  #feedback { margin-top: 14px; font-size: 14px; }
  #next-btn, #restart-btn {
    margin-top: 16px;
    padding: 8px 16px;
    border-radius: 6px;
    border: 1px solid #333;
    background: #222;
    color: #fff;
    cursor: pointer;
    display: none;
  }
  #result { text-align: center; }
  #result h2 { font-size: 22px; }
</style>
</head>
<body>

<h1>Phishing Awareness Quiz</h1>
<div id="quiz-root"></div>

<script>
const questions = [
  {
    q: "You get an email from 'support@paypa1.com' saying your account is locked and you must click a link within 1 hour to avoid suspension. What's the biggest red flag?",
    options: [
      "The misspelled domain (paypa1 instead of paypal) and artificial urgency",
      "The email uses your first name",
      "It was sent on a weekday",
      "It has a company logo"
    ],
    correct: 0,
    explain: "Look-alike domains and manufactured urgency are classic phishing signals. Real companies rarely threaten account closure within a strict, short deadline."
  },
  {
    q: "A coworker's email asks you to buy gift cards for a client and send the codes right away, saying they're in a meeting and can't talk. What tactic is this?",
    options: [
      "Authority and urgency combined (business email compromise)",
      "A normal work request",
      "Phishing doesn't happen over internal email",
      "This is a scarcity tactic only"
    ],
    correct: 0,
    explain: "This is a classic business email compromise pattern: impersonating someone with authority, creating urgency, and requesting something unusual (gift cards) that's hard to verify quickly."
  },
  {
    q: "What's the safest way to check if a suspicious email from your 'bank' is real?",
    options: [
      "Click the link and look at the website",
      "Reply to the email asking if it's legitimate",
      "Call the bank using the number on your card or their official website, not the number in the email",
      "Forward it to a friend for a second opinion"
    ],
    correct: 2,
    explain: "Always verify through a separate, trusted channel you look up yourself, never through contact info provided in the suspicious message itself."
  },
  {
    q: "Which of these is the strongest single defense against a stolen password from a phishing attack?",
    options: [
      "A very long email signature",
      "Multi-factor authentication (MFA)",
      "Changing your password every day",
      "Using the same strong password everywhere"
    ],
    correct: 1,
    explain: "MFA means a stolen password alone usually isn't enough to get in, since the attacker also needs your second factor (phone, authenticator app, etc)."
  },
  {
    q: "You spot a phishing email at work. What should you do first?",
    options: [
      "Just delete it and move on",
      "Reply and tell them to stop",
      "Report it to IT/security using your organization's reporting process",
      "Click the link to see where it goes, out of curiosity"
    ],
    correct: 2,
    explain: "Reporting helps your security team block the sender for everyone and track the wider campaign. Deleting alone leaves coworkers exposed."
  }
];

let current = 0;
let score = 0;
let answered = false;

function render() {
  const root = document.getElementById('quiz-root');
  if (current >= questions.length) {
    root.innerHTML = `
      <div id="result">
        <h2>Quiz complete</h2>
        <p>You scored ${score} out of ${questions.length}</p>
        <button id="restart-btn" style="display:inline-block;">Retake quiz</button>
      </div>`;
    document.getElementById('restart-btn').onclick = () => {
      current = 0; score = 0; answered = false; render();
    };
    return;
  }

  const item = questions[current];
  answered = false;

  root.innerHTML = `
    <div id="meta">
      <span>Question ${current + 1} of ${questions.length}</span>
      <span>Score: ${score}</span>
    </div>
    <div id="question">${item.q}</div>
    <div id="options"></div>
    <div id="feedback"></div>
    <button id="next-btn">Next question</button>
  `;

  const optionsDiv = document.getElementById('options');
  item.options.forEach((opt, i) => {
    const btn = document.createElement('button');
    btn.className = 'option';
    btn.textContent = opt;
    btn.onclick = () => selectAnswer(i, btn);
    optionsDiv.appendChild(btn);
  });
}

function selectAnswer(i, btn) {
  if (answered) return;
  answered = true;
  const item = questions[current];
  const buttons = document.querySelectorAll('#options .option');
  buttons.forEach((b, idx) => {
    b.disabled = true;
    if (idx === item.correct) b.classList.add('correct');
    else if (idx === i) b.classList.add('incorrect');
  });

  if (i === item.correct) score += 1;

  document.getElementById('feedback').textContent =
    (i === item.correct ? "✔ Correct — " : "✘ Incorrect — ") + item.explain;
  document.getElementById('next-btn').style.display = 'inline-block';
  document.getElementById('next-btn').onclick = () => { current += 1; render(); };
}

render();
</script>

</body>
</html>
