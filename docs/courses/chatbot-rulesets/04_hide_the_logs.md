# Hiding the Logs

<iframe src="https://www.youtube.com/embed/GTG1YfKudRA" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

In the previous section, we had a blast engaging with our chatbot, but there was one tiny detail that interrupted the flow of our conversations - those verbose logs cluttering our output. Fear not!  In this section, we'll show you how to turn off the logs and let your chatbot's brilliance shine without unnecessary distractions.


### Goal
After completing this section, you'll be able to enjoy clean and clutter-free conversations with your chatbot by disabling the logs.

## Logging Utility
### Import

We'll begin by importing the logging library, which will give us the power to control the verbosity of our chatbot's output. Add the following import statement to your code:

```python
import logging
```

Now we're ready to silence those logs and enjoy the tranquility of clean output.

### Add to Agent

It's time to modify our agent to quiet those logs and allow our chatbot's brilliance to shine through. Adjust the code where the agent is created, like so:

```python hl_lines="3"
# Create the agent
agent = Agent(
   logger_level=logging.ERROR
   )
```

By specifying `logger_level=logging.ERROR`, we indicate that we only want to receive logs of the highest priority, suppressing the informational logs and leaving us with a cleaner output.


Here is the code with the new lines highlighted:

``` py linenums="1" hl_lines="2 13" 
from dotenv import load_dotenv
import logging                     

# Griptape Items
from griptape.structures import Agent
from griptape.utils import Chat

# Load environment variables
load_dotenv()

# Create the agent
agent = Agent(
    logger_level=logging.ERROR      
)

# Run the agent
Chat(agent).start()
```

### Give it a try
Go ahead and execute the script and have a chat.

```
Q: Give me a haiku about python skateboarders
processing...
A: Python skateboarders
Glide on wheels, swift and free
Thrilling tricks they show
Q: 
```

!!! Success
    Ahh, isn't it refreshing? Now our conversations will flow seamlessly, without any distracting logs cluttering our chatbot's responses.

---

## Code Review

We've made valuable progress in this stage. Before proceeding, let's verify your code.

```python linenums="1" title="app.py"
from dotenv import load_dotenv
import logging                     

# Griptape Items
from griptape.structures import Agent
from griptape.utils import Chat

# Load environment variables
load_dotenv()

# Create the agent
agent = Agent(
    logger_level=logging.ERROR      
)

# Run the agent
Chat(agent).start()
```


## Next Steps

In the next section: [Personality With Rulesets](05_personality_with_rulesets.md), we'll unlock the true potential of your chatbot by giving it a vibrant personality with the help of **Rulesets**. Prepare to witness your chatbot's transformation as it takes on unique traits, behaviors, and even multiple personas. 
