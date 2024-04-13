# *`__SuperModels`*

SuperModels is an open-source project running multiple LLM APIs in one desktop app. 

![Not a llama](examples/output.gif)

Give it a try! [Download the executable here!](https://drive.google.com/file/d/1-Gxk9jkKhGLpx7jq6kFIVsU9OTPjgDfv/view?usp=sharing)

---
## *__instructions*

To run the packaged Electron app (windows only)

- [Download](https://www.python.org/downloads/) and install python on your hd at `C:/python312/python.exe`
- Join the FREE beta at [groq.com](https://groq.com/) to get a free API KEY 
- Input the API key when the app prompts you
---

## *__purpose*: `a self-reflecting machine` 

- Think Wittgenstein not Frankenstein
- Built-in reflection as a mechanism to steer *thinking*: ie. research and analysis
- Fully customisable and an exercise in best-practice OOP

It also avoids sprawling API wrappers like LangChain

*__in short*
> `SuperModels is lightweight, simple (or at least the Python part is), and easy to adapt and customise.`

Stack: `Python`, `Node.js Electron`, and `TailwindCSS`.

---

## *__3 modes, a walk through:*

#### 1. SuperModel
- [Groq](https://groq.com/) is cool as it's faster than any other inference engine as it uses LPUs instead of GPUs. 
- And it's free :)

#### 2. ToolUser
- A main argument for the Python is the `python_wizzard` - although running generated code is not without risk. (Be prepared for SuperModels to take control over your OS and start operating your home automation.)
- A second draw is this: Bing sucks, while Perplexity AI rocks. So the second tool is the `online_llm` drawing from Perplexity's API. 

#### 3. Agent
The **real magic** happens [here (click for source code)](https://github.com/JohannesVC/supermodels/tree/master/python/dispatch/_agent): 

1. The agent breaks down complex problems into steps:
```python
self._break_down = BreakdownReflection(prompt, model=self._model)
...
```
2. Every step is passed to the tooluser:
```python
for step in break_down_call.steps:
    self._answer = self._tooluser.call(step)
```
3. The tooluser's answer is fed back into the model:
```python
new_prompt = "Let's take a moment to consider."
            + "We just tried to answer: \n"
            + step + "\n\n"
            + "This as part of: \n"
            + f"- {"\n- ".join(step for step in break_down_call.steps)} \n\n"
            + f"This to answer the original question: {prompt}."
         
self._should_continue = YesNoReflection(new_prompt, self._answer, model=self._model)
```
4. The agent is then forced into a yes or no reflection:
```python
reflection:YesOrNo = self._should_continue.call()

if reflection.userInputRequired:
    ...
elif reflection.answer: 
    ...
```
>`The combination of a super fast tooluser and nominally typed answers allows for quite impressive reflections and reasoning.` Although I still haven't got it to turn the lights off.