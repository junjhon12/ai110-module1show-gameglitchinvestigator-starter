# 💭 Reflection: Game Glitch Investigator

Answer each question in 3 to 5 sentences. Be specific and honest about what actually happened while you worked. This is about your process, not trying to sound perfect.

## 1. What was broken when you started?

- What did the game look like the first time you ran it?
- List at least two concrete bugs you noticed at the start  
  (for example: "the hints were backwards").
  1- The game's hints were wrong since they were backward.
  2- The New Game button was meaningless since nothing change upon being pressed.
**Bug Reproduction Log**

Document at least 3 bugs you found. Add rows as needed.

| Input | Expected Behavior | Actual Behavior | Console Output / Error |
|-------|-------------------|-----------------|------------------------|
| | | | | Correct Hints     | Hints were backward | 
| | | | | New Game          | Nothing happend | 
| | | | | Correct number of attempts left | Incorrect number of attemps left | 

---

## 2. How did you use AI as a teammate?

- Which AI tools did you use on this project (for example: ChatGPT, Gemini, Copilot)?
Claude.ai
- Give one example of an AI suggestion that was correct (including what the AI suggested and how you verified the result).
The AI correclty resolved the button issue by connecting the missing links.
- Give one example of an AI suggestion that was incorrect or misleading (including what the AI suggested and how you verified the result).
It solved the button issue but by doing so it caused some calculation error for the scoring.
---

## 3. Debugging and testing your fixes

- How did you decide whether a bug was really fixed?
  The frontend all appeared working as intended and the backend logic had no major issues.
- Describe at least one test you ran (manual or using pytest)  
  and what it showed you about your code.
  There are some logic that needs to be redo and some that are missing connections.
- Did AI help you design or understand any tests? How?
  It provided solutions that doesn't affect the code overall nor did the solutions broke it further. At least, not majorly.
---

## 4. What did you learn about Streamlit and state?

- How would you explain Streamlit "reruns" and session state to a friend who has never used Streamlit?
It's a testing application that allow the programmer to test and deploy their project within one motion.
---

## 5. Looking ahead: your developer habits

- What is one habit or strategy from this project that you want to reuse in future labs or projects?
1- Finish one functionality at a time.
  - This could be a testing habit, a prompting strategy, or a way you used Git.
- What is one thing you would do differently next time you work with AI on a coding task?
1- Have the AI explain the problem before they explain their solutions.
- In one or two sentences, describe how this project changed the way you think about AI generated code.
Just because they solved the probelm doesn't mean the they solved it correctly.