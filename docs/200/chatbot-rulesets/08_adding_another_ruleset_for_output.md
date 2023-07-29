## Stage 8. Adding Output Rulesets - Controlling the Response Format

Consider a situation where we have integrated the LLM (Language Learning Module) into our code. It becomes crucial for us to receive the output in a specific format that aligns with our requirements, like JSON. By employing an output ruleset, we can precisely control the structure and format of the chatbot's responses.

### Goal:
After completing this section, you'll be able use output rulesets to get responses from the LLM in the way most useful for your application.

### 8.1 Crafting the JSON Ruleset - Structuring Responses

To achieve our goal of formatting the response as JSON, we'll create a ruleset called "json_ruleset." This ruleset will contain a single rule that tells the chatbot to use JSON when formulating it's response. Place it after the kiwi_rulesest:

```python
json_ruleset = Ruleset(
    name="json_ruleset",
    rules=[
        Rule("Use JSON when formulating your response.")
    ]
)
```

### 8.2 Enhancing the Agent - Integrating the json_ruleset

With the json_ruleset in hand, it's time to integrate it into our Agent. By including it in the list of rulesets available to the Agent, we can harness its power to control the response format.

```python
# Create the agent
agent = MyAgent(
    rulesets=[kiwi_ruleset, json_ruleset],
    logger_level=logging.ERROR
)
```

Here, we modify the `MyAgent` instantiation to include both the `kiwi_ruleset` and `json_ruleset` in the `rulesets=[]` argument. This ensures that our chatbot possesses the kiwi personality traits while _also_ adhering to the desired response format specified by the json_ruleset.

### 8.3 Trying it out

Prepare for an exciting conversation as we engage our chatbot in a quest for knowledge about Wellington's top tourist destinations. Let's dive in:

```json
Q: "Hey chatbot, what are the top three tourist destinations in Wellington? Can you give me a name and a description?"

Kiwi: {
  "message": "Absolutely, mate! Here are the top three tourist destinations in Wellington with a brief description:",
  "destinations": [
    {
      "name": "Te Papa Museum",
      "description": "New Zealand's national museum, known for its interactive and innovative exhibits."
    },
    {
      "name": "Wellington Cable Car",
      "description": "An iconic Wellington attraction, offering stunning views of the city and harbour."
    },
    {
      "name": "Zealandia Ecosanctuary",
      "description": "A unique protected natural area where you can see New Zealand's wildlife up close."
    }
  ]
}
```

Enjoy the beauty of Wellington's top tourist destinations, neatly presented in a JSON format, as our chatbot provides you with insightful reasons to visit each destination.

### 8.4 Utilizing the Ruleset Effectively

While this is an interesting example, let's use the ruleset in a way that helps control the way our application works.

Currently, the user has to know to type "exit" to leave the chat. This is not a great user experience, as it's a hidden command. We are using a chat-interface... wouldn't it be great if we could simply _tell_ the chatbot when we were done chatting and it would quit on it's own?

Turns out, we can do just that - by using the `json_ruleset`.

Modify the json ruleset to look like the following:
```python
json_ruleset = Ruleset(
    name='json_ruleset',
    rules = [
        Rule("Respond in plain text only with JSON objects that have the following keys: response, continue_chatting."),
        Rule("The 'response' value should be a string that is your response to the user."),
        Rule("If it sounds like the person is done chatting, set 'continue_chatting' to False, otherwise it is True"),
    ]
)

```

The **first** rule tells the chatbot to respond in json, and specifies the keys. The **second** and **third** rules explain what the values for those keys should be. Notice the third one specifically says that if it sounds like the person is done chatting, set `continue_chatting` to `False`.

Go ahead and run the example and notice the response.


```json
Kiwi: {
  "response": "G'day mate! I'm a bot from New Zealand, speaking with a strong kiwi accent. How can I assist you today?",
  "continue_chatting": true
}

Chat with Kiwi: see ya later

Kiwi: {
  "response": "No worries, mate! Catch ya later!",
  "continue_chatting": false
}
```

See how `continue_chatting` returns false when it sounds like we're done talking?

Let's now use this json output!

### 8.5 Using JSON in all it's glory

First, we'll have to import the json library. To do that, add the following at the beginning of your script:

```python
import json
```

Next, we'll use the `json.loads()` function to take the output from the agent's response and convert it into json data.

Modify the start of the `respond` method of the `MyAgent` class, to look like:

```python
    # ... truncated for brevity
    def respond (self, user_input):
        agent_response = agent.run(user_input)
        data = json.loads(agent_response.output.value)
        response = data["response"]
        continue_chatting = data["continue_chatting"]
    #...

```
This creates two variables - `response` which will be the normal response from the chatobt, and `continue_chatting` which should be `True` or `False`.

Modify the print statement where we get the response from the chatbot to look like:

```print
        print(f"Kiwi: {response}")
```

And then return the `continue_chatting` at the end of the method.  
```python
    # ...
    def respond (self, user_input):
      # ...
      return continue_chatting
    #...
```

The whole class should look like:

```python
# Create a subclass for the Agent
class MyAgent(Agent):
        
    def respond (self, user_input):
        agent_response = agent.run(user_input)
        data = json.loads(agent_response.output.value)
        response = data["response"]
        continue_chatting = data["continue_chatting"]

        print("")
        print(f"Kiwi: {response}")
        print("")

        return continue_chatting
```

Since we're returning `True` or `False` from the `agent.respond()` method, the entire `chat` function can now be simplified as:
```python
# Chat function
def chat(agent):
    is_chatting = True
    while is_chatting:
        user_input = input("Chat with Kiwi: ")
        is_chatting = agent.respond(user_input)
```

Give it a try and see how you can quit the chat simply by holding the conversation:

```
Kiwi: G'day mate! I'm a bot from New Zealand, speaking with a strong kiwi accent. How can I assist you today?

Chat with Kiwi: I'm good, how are you?

Kiwi: I'm doing great, thanks for asking! Anything else you'd like to chat about, mate?

Chat with Kiwi: Nah, I'm done for today.

Kiwi: No worries, mate! Have a good one. Don't hesitate to reach out if you need anything else.

```

--- 

### Code Checkpoint

By leveraging the power of output rulesets, we've demonstrated how you can guide your chatbot to deliver responses in any desired format. Take a moment to check your code against the [Stage 8 Code Checkpoint](../assets/examples/08_app.py) on GitHub.

### Next Steps

In the next stage, [Formatting Chat Output](09_formatting_chat_output.md), 
 we'll make the chat interface more visually appealing and chat-like using the `rich` library. Get ready to add some style and flair to your conversations!