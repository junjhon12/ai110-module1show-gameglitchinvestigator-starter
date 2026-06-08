# 🎮 Game Glitch Investigator: The Impossible Guesser

## 🚨 The Situation

You asked an AI to build a simple "Number Guessing Game" using Streamlit.
It wrote the code, ran away, and now the game is unplayable.

- You can't win.
- The hints lie to you.
- The secret number seems to have commitment issues.

---

## 🛠️ Setup

1. Install Python 3.12 (required — Python 3.14 breaks pandas/Streamlit)
2. Install dependencies:
   ```
   py -3.12 -m pip install streamlit pytest
   ```
3. Run the app:
   ```
   py -3.12 -m streamlit run app.py
   ```

---

## 🕵️‍♂️ Your Mission

1. **Play the game.** Open the "Developer Debug Info" expander to see the secret number. Try to win.
2. **Find the State Bug.** Why does the secret number change every time you click "Submit"? Ask an AI: *"How do I keep a variable from resetting in Streamlit when I click a button?"*
3. **Fix the Logic.** The hints ("Higher/Lower") are wrong. Fix them.
4. **Refactor & Test.** Move the logic into `logic_utils.py`, run `pytest`, and keep fixing until all tests pass.

---

## 📝 Document Your Experience

- [x] **Game's purpose:** A number guessing game where the player picks a difficulty, then tries to guess a randomly chosen secret number within a limited number of attempts. Each wrong guess returns a directional hint (Too High / Too Low). The final score equals the number of attempts still remaining when the player wins — so guessing faster scores higher.

- [x] **Bugs found:**

  | # | Bug | Root Cause |
  |---|-----|------------|
  | 1 | Hints were backwards | `check_guess` alternated `secret` between `int` and `str` on even/odd attempts; Python string comparison (`"30" > "95"`) is lexicographic, flipping the direction |
  | 2 | `check_guess` returned a tuple | Returned `("Too High", "📉 Go LOWER!")` instead of a plain string, so `result == "Too High"` always evaluated `False` silently |
  | 3 | New Game broke after a win/loss | Only reset 2 of 5 session state keys; `status` stayed `"won"` so `st.stop()` fired immediately on the next run |
  | 4 | Input not cleared on New Game | Widget key never changed, so Streamlit kept the old typed value and Submit re-submitted the previous guess |
  | 5 | Attempts counter off by one | `attempts` initialised to `1` instead of `0`, showing one attempt used before the player did anything |
  | 6 | Score used wrong formula | Used `attempt_number` (attempts used) instead of `attempt_limit - attempt_number` (attempts remaining) |
  | 7 | Score off by one on winning guess | `attempts` incremented before `update_score` was called, so the score was one less than the "Attempts left" value the player saw |

- [x] **Fixes applied:**
  1. Removed the `int`/`str` alternation — `secret` is always passed as an `int` to `check_guess`
  2. Split `check_guess` into two functions: `check_guess` returns a plain outcome string; `get_hint_message` returns the emoji UI text separately
  3. New Game now resets all five state keys (`attempts`, `secret`, `score`, `status`, `history`) and picks from the correct difficulty range
  4. Added a `game_id` counter; included in the text input `key` so every New Game mounts a fresh empty widget
  5. Initialised `attempts` to `0`
  6. Added `attempt_limit` as a parameter to `update_score`; changed formula to `attempt_limit - attempt_number`
  7. Pass `attempts - 1` to `update_score` so the score reflects the remaining count the player saw before their winning guess

---

## 📸 Demo Walkthrough

Difficulty: **Normal** (range 1–100, 8 attempts allowed). Secret number: **63**.

1. App loads. Info bar reads: *"Guess a number between 1 and 100. Attempts left: 8"*
2. Player enters **40** → 📈 **Go HIGHER!** (Too Low). Attempts left: 7. Score: 0.
3. Player enters **80** → 📉 **Go LOWER!** (Too High). Attempts left: 6. Score: 0.
4. Player enters **60** → 📈 **Go HIGHER!** (Too Low). Attempts left: 5. Score: 0.
5. Player enters **70** → 📉 **Go LOWER!** (Too High). Attempts left: 4. Score: 0.
6. Player enters **65** → 📉 **Go LOWER!** (Too High). Attempts left: 3. Score: 0.
7. Player enters **63** → 🎉 **Correct!** Balloons appear.
   Final score: **3** (3 attempts were still showing before this guess: `8 limit − 5 prior guesses = 3`). ✅
8. Player clicks **New Game 🔁** — all state resets instantly, input clears, attempts return to 8, a new secret is chosen. Game is playable again regardless of whether the previous game was won or lost.

---

## 🧪 Test Results

```
pytest tests/test_game_logic.py -v
============================= test session starts =============================
platform win32 -- Python 3.14.4, pytest-9.0.3, pluggy-1.6.0
rootdir: C:\AI110\ai110-module1show-gameglitchinvestigator-starter
collected 12 items

tests/test_game_logic.py::test_winning_guess PASSED                      [  8%]
tests/test_game_logic.py::test_guess_too_high PASSED                     [ 16%]
tests/test_game_logic.py::test_guess_too_low PASSED                      [ 25%]
tests/test_game_logic.py::test_score_equals_attempts_remaining PASSED    [ 33%]
tests/test_game_logic.py::test_score_wins_on_first_attempt PASSED        [ 41%]
tests/test_game_logic.py::test_score_wins_on_last_attempt PASSED         [ 50%]
tests/test_game_logic.py::test_score_uses_attempts_remaining_not_attempts_used PASSED [ 58%]
tests/test_game_logic.py::test_score_unchanged_on_too_high PASSED        [ 66%]
tests/test_game_logic.py::test_score_unchanged_on_too_low PASSED         [ 75%]
tests/test_game_logic.py::test_score_unchanged_on_loss_nonzero_starting_score PASSED [ 83%]
tests/test_game_logic.py::test_score_easy_difficulty PASSED              [ 91%]
tests/test_game_logic.py::test_score_hard_difficulty PASSED              [100%]

============================= 12 passed in 0.05s ==============================
```

---

## 🚀 Stretch Features

- [ ] [If you choose to complete Challenge 4, describe the Enhanced UI changes here — a screenshot is optional]