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

Glitchy Guesser is an app that is developed on the basis of Streamlit, a number guessing game, in which the player attempts to estimate an enigmatic number given a restricted count of tries. The gamer picks the level of difficulty, they are given hints after each attempt telling them whether to go high or down and score points based on the speed at which they guess correctly. The bugs in the game were designed to be there as a debugging exercise to train on the process of finding and fixing AI-generated code. 

The counter of attempts began at 1 rather than 0, and as such the game had one less attempt than the player actually had. Each even guess that the secret number was changed to a string, which made Python compare the hints alphabetically, inverted the hints, making Go Higher and Go Lower the reversed version. The score was paying +5 points on even attempts where the attempt had a wrong guess of Too High, rather than subtracting point always. Hard difficulty was based on a range of 1-50, which is in fact smaller and less difficult than the range of 1-100 of Normal. The New Game button would always randomly choose a number somewhere between 1 and 100 no matter what difficulty mode was chosen and this was a complete break to Easy and Hard modes.

Changed st.session_state.attempts initial value from 1 to 0.
Removed the string conversion block and always passed the secret as an integer to check_guess.
Simplified update_score to always deduct 5 points for any wrong guess, removing the +5 branch entirely.
Changed the Hard difficulty range to 1-200 so it is genuinely harder than Normal.
Updated the New Game button to use the difficulty-based low and high variables instead of hardcoded 1-100.

## 📸 Demo

<img width="548" height="190" alt="image" src="https://github.com/user-attachments/assets/b4c939db-b04a-40f7-b21e-f8e8db7872b9" />


## 🚀 Stretch Features

- [ ] [If you choose to complete Challenge 4, insert a screenshot of your Enhanced Game UI here]
