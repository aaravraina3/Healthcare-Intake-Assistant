# Marco by Shaurya Jain, Aarav Raina, Daniel Yang, and Veeru Senthil
## Inspiration

We built Marco because I saw firsthand how disabilities can slowly take away someone’s ability to do the work they love. My favorite high school teacher, Mrs. Vetter, was diagnosed with Multiple Sclerosis. Over the years her smile faded as everyday tasks became harder, and her job filled with manual entry and spreadsheets turned into something she struggled to keep up with. That experience stuck with me. Marco was born out of wanting to give people like her tools that don’t just make healthcare information accessible, but help them keep their independence, confidence, and voice in a world that often overlooks them.

## What it does 

Marco allows you to seamlessly interact with a browser with just your voice – it can be anything from making an appointment with your doctor, booking plane tickets, or even just doing research. Marco responds back to you with the results of its actions in voice too! It is a great tool for people who have sensorimotor problems and impaired vision, since it is entirely voice and audio powered. You never have to lift a finger!

## How we built it

Marco is essentially a voice controlled browser agent inside of a NextJS application.

We built a FastAPI server that hosts the Chromium browser page and provides a websocket. We then display the browser being used by our AI agent in the NextJS application by streaming the page to the frontend through a websocket. The FastAPI server also contains our Langchain AI agent powered by Claude 4 Sonnet. 

The AI agent workflow for Marco is as follows: We use WebVAD to detect voice, and then run it through an Eleven Labs Speech to Text model. The text is then passed into our Langchain agent that determines if it should use a tool to analyze the page with Gemini 2.5 Flash by taking a screenshot, or if it should use a Stagehand agent to manipulate the web page, take actions, and navigate between pages. The LangChain Agent can call these tools as many times as necessary to complete the task, and once it is done it returns the text, which is passed through Eleven Labs’ Text to Speech for audio. 



## Challenges we ran into

One of the biggest issues we faced was reliably streaming the browser page to the frontend with minimum latency, no flickering, and ensuring that it did not block the agent from accessing the page, since the agent and the streaming operations were being done on the same Playwright page object. Choosing a good frame rate and using the websocket provided by Chromium, CDP solved this.

Another challenge was making sure that the speech to text pipeline was not active while the agent was completing its task on the web. We solved it by disabling the mic while the agent was speaking or executing

Testing the application and managing the memory and context was the hardest part by far. Browser agents and AI agents in general, are only as good as the content you give them, and this was definitely the case for Marco. We solved this by using short term memory modules in LangChain.

## Accomplishments that we're proud of

We are most proud of how versatile Marco is. We had so much fun testing it doing things like comparing stock performances, getting news summaries, and getting predictions for our favorite football teams! Alongside this, we are also proud of the quality, detailed, and insightful text-to-speech messages that were generated, to easily communicate to the user when it matters most. 


## What we learned

We learned about web automation with Playwright, using Chrome Devtools Protocol(CDP), and building intelligent AI Agents with LangChain. Dabbling with different tool calls through web research and analyzing webpage screenshots, we learned how optimizing effective tool calls with LangChain allows us to easily streamline our responses to be clear, concise, and accurate. And we learned that dividing up different tool calls and chromium operations with multiple agents allows us to also preserve context and memory, which is important for an LLM in order to give proper reasoning that is based on previous, factual evidence.

## What's next for Marco

In the future, we hope to scale Marco to bigger and better avenues. Marco works seamlessly at the local, small scale level, but we hope to scale to users across the globe, especially users with little technology experience or with impeded communication skills who need to seek the truthful, correct, answers when it comes to serious topics like medicine. We hope to scale to reach a broader audience by adding more accessibility features, like eye tracking movement or even direct brain neuron readings which will help users with speech limitations find the solutions that they need easily. 



