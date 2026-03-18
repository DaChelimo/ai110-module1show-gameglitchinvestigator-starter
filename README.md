# 🎮 Game Glitch Investigator: The Impossible Guesser

## 🚨 The Situation

You asked an AI to build a simple "Number Guessing Game" using Streamlit.
It wrote the code, ran away, and now the game is unplayable. 

- You can't win.
- The hints lie to you.
- The secret number seems to have commitment issues.

## 🛠️ Setup

1. Install dependencies: `pip install -r requirements.txt`
2. Run the broken app: `python -m streamlit run app.py`

## 🕵️‍♂️ Your Mission

1. **Play the game.** Open the "Developer Debug Info" tab in the app to see the secret number. Try to win.
2. **Find the State Bug.** Why does the secret number change every time you click "Submit"? Ask ChatGPT: *"How do I keep a variable from resetting in Streamlit when I click a button?"*
3. **Fix the Logic.** The hints ("Higher/Lower") are wrong. Fix them.
4. **Refactor & Test.** - Move the logic into `logic_utils.py`.
   - Run `pytest` in your terminal.
   - Keep fixing until all tests pass!

## 📝 Document Your Experience

- **Game purpose:** A number-guessing game where the player tries to guess a secret number within a limited number of attempts, receiving higher/lower hints and a score that decreases with each wrong guess.

- **Bugs found:** The hints were backwards: guessing too high said "Go HIGHER!" and vice versa. On every even-numbered attempt, the secret was silently converted to a string, making it impossible to win on attempts 2, 4, 6, etc. The attempt counter started at 1 instead of 0, the Hard difficulty range `(1, 50)` was easier than Normal `(1, 100)`, invalid inputs consumed an attempt, the scoring formula had an off-by-one error, "Too High" guesses inconsistently added or subtracted points based on odd/even attempts, New Game didn't reset the score or status, and the `logic_utils.py` functions were all unimplemented stubs.

- **Fixes applied:** Swapped the hint messages in `check_guess` and removed the string coercion block so the secret is always compared as an integer. Initialized attempts to `0`, changed Hard's range to `(1, 500)`, moved the attempt increment to after input validation, and corrected the scoring formula to `100 - 10 * attempt_number` with a flat −5 for any wrong guess. New Game now resets score, status, and history and uses the correct difficulty range; all four game functions were implemented in `logic_utils.py` and imported into `app.py`; and the test assertions were fixed from `result == "Win"` to `result[0] == "Win"` to match the tuple return type.

## 📸 Demo

![](/winning_game.png "")

## 🚀 Stretch Features

- [ ] [If you choose to complete Challenge 4, insert a screenshot of your Enhanced Game UI here]
