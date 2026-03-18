# 💭 Reflection: Game Glitch Investigator

Answer each question in 3 to 5 sentences. Be specific and honest about what actually happened while you worked. This is about your process, not trying to sound perfect.

## 1. What was broken when you started?

- What did the game look like the first time you ran it?
- List at least two concrete bugs you noticed at the start  
  (for example: "the hints were backwards").

1. The hint was in the opposite direction (the secret was 17 but it told me to go higher)
2. The range of values is not enforced or checked
3. Attempts start off at 1 and not 0

---

## 2. How did you use AI as a teammate?

- Which AI tools did you use on this project (for example: ChatGPT, Gemini, Copilot)? Claude and Copilot
- Give one example of an AI suggestion that was correct (including what the AI suggested and how you verified the result).

- Give one example of an AI suggestion that was incorrect or misleading (including what the AI suggested and how you verified the result). \
Example of a correct suggestion: Claude identified that on every even-numbered attempt, the code was converting the secret number to a string before passing it to check_guess. Since in Python, 50 == "50" is always False, so the player could never win on attempt 2, 4, 6..... I verified this by opening the debug panel, noting the secret was 34, entering 34 on my second attempt, and watching the game respond with a wrong hint instead of a win. Removing the string coercion lines fixed it completely. 

- Example of a misleading or incomplete suggestion: Not really. The suggestion Claude gave or errors it pointed out were pretty accurate.

---

## 3. Debugging and testing your fixes

- How did you decide whether a bug was really fixed? \
I ran my tests which verified the logic is correct. Afterwards, I manually tested the UI to ensure the logic is correct. 
- Describe at least one test you ran (manual or using pytest) and what it showed you about your code. \
After implementing the functions in logic_utils.py and fixing the hint logic, I ran test_guess_too_high. Before the fix, all three would have failed because (a) the functions raised NotImplementedError, and (b) the tests asserted result == "Win" instead of result[0] == "Win". After both fixes, however, all three tests passed.

- Did AI help you design or understand any tests? How? \
Copilot pointed out that the existing tests had the wrong assertion style (result == "Win" vs result[0] == "Win"), which was the reason they would fail even after the logic was correct. That saved time I would have spent staring at a passing implementation but failing tests.

---

## 4. What did you learn about Streamlit and state?

- How would you explain Streamlit "reruns" and session state to a friend who has never used Streamlit? \
Every time you interact with a Streamlit app such as clicking a button, typing in a box, changing a dropdown, Streamlit reruns the entire Python script from top to bottom. It doesn't remember where it was and therefore, resets everything. \
st.session_state is how Streamlit solves this. It's kind of a dictionary that persists between reruns for the same browser session. You store things you want to keep like the attempt count and they survive each rerun. The pattern is: check if "key" not in st.session_state before setting a value, so you only initialize it once. After that, every rerun reads the saved value instead of creating a new one.

---

## 5. Looking ahead: your developer habits

- What is one habit or strategy from this project that you want to reuse in future labs or projects? \
 Run the code and try to break it manually before reading it. The most obvious bugs in this project (backwards hints) were immediately visible the moment I played the game with the debug panel open. Reading the code cold would have taken much longer to spot the string coercion bug.


- What is one thing you would do differently next time you work with AI on a coding task? \
Asking AI to brainstorm edge cases that I have to think about. This ensures that I write the most comprehensive code possible.


- In one or two sentences, describe how this project changed the way you think about AI generated code. \
AI can write code that looks correct and even runs without errors while still being completely broken in practice