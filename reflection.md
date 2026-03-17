# 💭 Reflection: Game Glitch Investigator

Answer each question in 3 to 5 sentences. Be specific and honest about what actually happened while you worked. This is about your process, not trying to sound perfect.

## 1. What was broken when you started?

- What did the game look like the first time you ran it?
- List at least two concrete bugs you noticed at the start  
  (for example: "the hints were backwards").

The first time I played the game the UI displayed Attempts left: 1 on its own before I made a guess, and that was a clear indication that there was something wrong with the counter. The counter was started to count up to 1 rather than 0 and hence the count on display was one less at the very beginning. On all even numbered guesses, the hints were inverted since the code matched my integer guess with a string form of the secret resulting in Go HIGher and Go LOWER flipping randomly. Even the scoring system was faulty - attempting to guess wrong on the even ones of the attempt that were actually +5 should have deducted. Lastly, Hard difficulty was also paradoxically less challenging than 1 -100 of Normal, and New Game button was always set to 1-100, depending on the difficulty chosen.

## 2. How did you use AI as a teammate?

- Which AI tools did you use on this project (for example: ChatGPT, Gemini, Copilot)?
- Give one example of an AI suggestion that was correct (including what the AI suggested and how you verified the result).
- Give one example of an AI suggestion that was incorrect or misleading (including what the AI suggested and how you verified the result).

GitHub Copilot was the AI tool I used in VS Code and also Claude. When I inquired of Copilot as to why the hints were flipping, correctly identified that attempting to convert the secret to a string on even-numbered tries caused Python to compare lexicographically rather than numerically - e.g. the string "9" is larger than the string "36" in the alphabet. I confirmed that this was the actual reason by deleting the block of if st.sessionstate.attempts % 2 =0 and inserting a single block of secret = st.sessionstate.secret, and then replayed the game and ensured that the hints were always correct. But when I requested Copilot to fix the update score function, it hinted at a more complicated rewrite that utilized a multiplier system that depended on attempted number, creating another bug whereby getting a victory on a late attempt would yield a negative score - something that the original game did not aim to do. The only thing that made me accept the suggestion was that I manually followed through with the function using a few test values before I was willing to accept it, also a reminder that no AI output should be copy-pasted blindly.

---

## 3. Debugging and testing your fixes

- How did you decide whether a bug was really fixed?
- Describe at least one test you ran (manual or using pytest)  
  and what it showed you about your code.
- Did AI help you design or understand any tests? How?

I only came to the decision that a bug was actually solved after two things would happen: the pytest test on that particular bug would succeed and the live game in the browser would act correctly after I played it by hand. To give an example, I had just fixed the string conversion bug in checkguess, and I had written a test which called checkguess(99, 36) and got the result which I would have expected, that is, Too High, which would have given the result Too Low before the bug was fixed. The fact that pytest ran and all 15 tests did pass gave me the assurance that the logic was correct, and running the game again after this helped me to realize that the hints were no longer backwards. Claude made me see what the tests were actually checking that is, it pointed out that I needed to specifically test an even-attempt scenario to demonstrate that the string conversion bug had been eliminated, and not simply test a generic correct/incorrect guess.

---

## 4. What did you learn about Streamlit and state?

- How would you explain Streamlit "reruns" and session state to a friend who has never used Streamlit?

Streamlit does not operate in the same way as most programs - every time a user clicks a button, a slider, or types a single something, the whole Python program restarts at the bottom. It implies that any regular variable you made is wiped out and set every time you interact with it and it would not be possible to track variables such as the current score or the number of attempts. And that is the role of session state, st.session state is a small notebook that Streamlit stores between reruns, effectively giving the values in this notebook persistence across reruns. And when you fail to use session state and simply replace it with a plain ordinary variable, your game will restart itself each time your player makes any action and this is just the reason that the bugs in this project were so difficult to detect at first sight.

---

## 5. Looking ahead: your developer habits

- What is one habit or strategy from this project that you want to reuse in future labs or projects?
  - This could be a testing habit, a prompting strategy, or a way you used Git.
- What is one thing you would do differently next time you work with AI on a coding task?
- In one or two sentences, describe how this project changed the way you think about AI generated code.

A single practice that I wish to bring into my future work is the addition of the comment FIXME where the first sign of a suspicious situation is detected, before I really know what the bug is - this is to make me go slow, and add the comment where the suspicious behavior is, rather than in my memory where I am trying to store all the information at once. The next time I used AI on a coding problem I would provide it with a lot more context at the beginning, such as copy-pasting the faulty function instead of explaining the issue in plain language since the AI recommendations were much more precise when it was given the actual code. The project made me reconsider my view of AI generated code - I had already thought that when it was running without crashing it was likely correct but now I understand that the worst bugs are those that run correctly and give incorrect answers without alerting you, and that AI can just as easily cause bugs to be fixed as they can fix bugs.
