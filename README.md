# SpeedTrap CAPTCHA

**Concept and Implementation by Nikunj Anand, Age 13**

---

## Overview

SpeedTrap CAPTCHA is an innovative bot-detection system designed to identify automated scripts by measuring the speed at which users complete a simple verification action. Instead of complicating CAPTCHAs with difficult puzzles, SpeedTrap makes the test intentionally easy — and flags users who respond unrealistically fast.

---

## How It Works

- **Bots** tend to respond instantly, clicking buttons or elements faster than a human ever could.  
- **Humans** naturally pause, read, and interact with slight delay.

SpeedTrap leverages this timing difference by:

- Disabling the verification button initially, then enabling it after a brief delay.  
- Monitoring the interval between enabling the button and the user’s click.  
- Flagging users who click too quickly as potential bots, forcing a retry delay.  
- Including a hidden “trap button” invisible to humans but detectable by bots scanning page elements.

---

## Why SpeedTrap Matters

Traditional CAPTCHAs are often:

- Frustrating and time-consuming for users.  
- Increasingly vulnerable to advanced bots and human farms.

SpeedTrap offers a low-friction alternative focused on behavioral timing, providing a fresh layer of bot defense while maintaining a smooth user experience.

---

## Example Implementation

Below is the full code snippet you can deploy immediately in an HTML compiler online. It uses standard HTML and JavaScript, requires no external libraries, and can be integrated easily into existing web pages:

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- 
''html

<p style="font-weight: bold; color: orange;">
  ⚠️ Please wait a few seconds before clicking the verification button. This helps our system verify your browser's authenticity.
</p>

<!-- Hidden trap button -->
<button id="trapBtn" style="display: none;">
  ⚠️ SYSTEM BUTTON — DO NOT CLICK
</button>

<button id="captchaBtn" disabled style="padding: 12px 24px; font-size: 16px; cursor: not-allowed;">
  Click to Verify
</button>

<!-- Invisible countdown -->
<p id="countdown" style="display: none;"></p>

<p id="message" style="margin: 12px 0; font-weight: bold;"></p>

<script>
  let trapClicked = false;
  let attempts = 0;
  let start;
  const trapBtn = document.getElementById('trapBtn');
  const captchaBtn = document.getElementById('captchaBtn');
  const msg = document.getElementById('message');
  let retryInterval;

  // Initial wait: 0.5 seconds before enabling the button
  let initialWait = 0.5;
  let secondsLeft = initialWait;

  const interval = setInterval(() => {
    secondsLeft -= 0.1;
    if (secondsLeft <= 0) {
      clearInterval(interval);
      captchaBtn.disabled = false;
      captchaBtn.style.cursor = 'pointer';
      start = Date.now();
    }
  }, 100);

  trapBtn.addEventListener('click', () => {
    trapClicked = true;
    msg.textContent = "Non-human activity detected. Please try verification again, clicking a bit slower.";
  });

  captchaBtn.addEventListener('click', () => {
    if (!start) return;

    if (trapClicked) {
      msg.textContent = "Non-human activity detected. Please try verification again, clicking a bit slower.";
      return;
    }

    const delay = Date.now() - start;
    const threshold = 800 + Math.random() * 400;

    if (delay < threshold) {
      attempts++;
      msg.textContent = "Too fast! You might be a bot. Try again and wait a few moments before clicking.";
      start = Date.now();
      captchaBtn.disabled = true;
      captchaBtn.style.cursor = 'not-allowed';

      // Retry wait: 2 seconds on failures
      if (retryInterval) clearInterval(retryInterval);
      secondsLeft = 2;
      retryInterval = setInterval(() => {
        secondsLeft--;
        if (secondsLeft <= 0) {
          clearInterval(retryInterval);
          captchaBtn.disabled = false;
          captchaBtn.style.cursor = 'pointer';
          start = Date.now();
        }
      }, 1000);
    } else {
      msg.textContent = "Verification passed. Welcome, human!";
      captchaBtn.disabled = true;
    }
  });
</script>

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

This project represents a foundational prototype of what the future of CAPTCHA technology could evolve into. 
It demonstrates a simple yet effective approach to distinguishing humans from bots through behavioral timing rather than traditional complex puzzles. 
As the original creator of this concept, I respectfully request proper recognition and due credit for developing this innovative idea.
I am genuinely interested in discussing opportunities to license or sell this technology to forward-thinking companies aiming to enhance their bot-detection systems. 
Collaborating on advancing this concept could offer significant value to the cybersecurity community and end users alike.


For more information and future partnership please contact me at; 

nikunj.anand.2011@gmail.com

