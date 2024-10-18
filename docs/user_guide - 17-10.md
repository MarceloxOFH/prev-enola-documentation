# Enola-AI Python Library Documentation

## Table of Contents

1. [Introduction](#introduction)
2. [Features](#features)
3. [Requirements](#requirements)
4. [Installation](#installation)
5. [Getting Started](#getting-started)
   - [Initializing Tracking](#initializing-tracking)
     - [Example: Basic Tracking Initialization](#example-basic-tracking-initialization)
   - [Understanding Steps in Enola-AI](#understanding-steps-in-enola-ai)
     - [Generic Steps](#generic-steps)
     - [LLM Steps](#llm-steps)
6. [Sending Online Chat Data](#sending-online-chat-data)
7. [Sending Online Score Data](#sending-online-score-data)
8. [Sending Multiple Tasks](#sending-multiple-tasks)
9. [Sending File Information](#sending-file-information)
10. [Sending API Information](#sending-api-information)
11. [Sending Cost Information](#sending-cost-information)
12. [Sending Batch Score Data](#sending-batch-score-data)
13. [Summary](#summary)
14. [Contributing](#contributing)
15. [License](#license)
16. [Contact](#contact)

---

## Introduction

Enola AI is an advanced GenAI platform designed to validate and monitor the robustness of artificial intelligence models in highly regulated industries such as finance, healthcare, and education. Our solution ensures that AI implementations comply with strict regulatory standards through continuous assessments, seamless integrations, and real-time monitoring.

This documentation provides a comprehensive guide on how to use the Enola-AI Python library to interact with the platform. You will learn how to install the library, understand the types of steps used in tracking, send different types of data, and utilize various features to enhance your AI model's performance and compliance.

---

## Features

- **Multilevel Evaluation**: Collect feedback from users, automated assessments, and reviews from internal experts.
- **Real-Time Monitoring**: Continuous monitoring capabilities to detect deviations in AI model behavior.
- **Seamless Integration**: Compatible with existing infrastructures such as ERP systems, CRM platforms, and data analytics tools.
- **Customizable Configuration**: Adapt the evaluation methodology according to the specific needs of the client.
- **Security and Compliance**: Advanced security measures and compliance with data protection regulations.

---

## Requirements

- **Python 3.7+**
- **Enola API Token**

---

## Installation

Before installing the Enola-AI Python library, ensure that you have Python 3.7 or higher installed.

**Install the SDK via pip**

In your Command Prompt (Windows) or Terminal (Linux/macOS) type:

   ```bash
   pip install enola
   ```

By doing a pip install, you have the Enola-AI Python library and its dependencies automatically installed in your device.
```Note 
Note: Advanced users may prefer to use a virtual environment to manage dependencies and 
isolate the Enola-AI library from other Python packages
```

---

## Getting Started

To start using the Enola-AI Python library, follow the steps below to initialize tracking in your application.

### Initializing Tracking
To connect to Enola and initialize tracking you will need:
- A token provided by Enola. This token is essential for authentication and authorization purposes.
- A Python script, you can start creating an empty Python file with .py extension. (eg. enola_sample.py, enola_script.py)

#### **1. Loading Enola API Token**
**You can load the token from a `.env` file (recommended).**
This method is recommended due to better security for token management.
- Go to the same directory where your Python file is located
- To load the token, you have to create a .env file, with name ".env"

- Then open the file with a text editor and add this line:

    ```plaintext
    ENOLA_TOKEN=your_api_token
    ```
- Replace `your_api_token` with your Enola API token.
- Assuming the Python file and the `.env` file are in the same directory, your token should be loaded

**Alternatively, you can set it directly in your script:**
This method is easier and fine for testing purposes, but it is not the recommended method due to the token is exposed in the script. It is advisable to always keep your token information secure.

To set it directly in your Python script:
```python
token = 'your_api_token'
```
**You can also load the token using Environment Variables**
Another method to load the token is by setting your Enola API token as an Environment Variable. This method offers more flexibility but it requires more complex configurations, depending on your operating system.
For convenience, the method for loading the token from a `.env` file will be covered instead in this guide.


#### **2. Import the Necessary Libraries**
Assuming you are loading the token from the `.env` file,
In your Python script, start importing the necessary libraries:
```python
# Import necessary libraries
from enola.tracking import Tracking     # Enola Tracking module
from dotenv import load_dotenv          # .env Loader
import os                               # Operating System functionalities
```

#### **3. Initialize the Tracking Agent**
Load the Enola API token and initialize the tracking agent.
```python
# Load .env file
load_dotenv()
# Set up your token
token = os.getenv('ENOLA_TOKEN')

# Initialize the tracking agent
monitor = Tracking(
    token=token,			  # Your Enola API token
    name="My Enola Project",  # Name of your tracking session
    is_test=True,             # Set to True if this is a test session
    app_id="my_app_id_01",    # Application ID
    user_id="user_123",       # User ID
    session_id="session_456", # Session ID
    channel_id="console",     # Channel ID (e.g., 'console', 'web', 'mobile')
    ip="192.168.1.1",         # IP address of the client
    message_input="Hello, what can you do?"  # Input message from the user
)
```
---

#### **Complete Example: Basic Tracking Initialization**

```python
# Import necessary libraries
from enola.tracking import Tracking
from dotenv import load_dotenv
import os

# Load .env file and set up your token
load_dotenv()
token = os.getenv('ENOLA_TOKEN')

# Initialize the tracking agent
monitor = Tracking(
    token=token,
    name="My Enola Project",  # Name of your tracking session
    is_test=True,             # Set to True if this is a test session
    app_id="my_app_id_01",    # Application ID
    user_id="user_123",       # User ID
    session_id="session_456", # Session ID
    channel_id="console",     # Channel ID (e.g., 'console', 'web', 'mobile')
    ip="192.168.1.1",         # IP address of the client
    message_input="Hello, what can you do?"  # Input message from the user
)
```
After executing the script and initializing the tracking agent, you should get a console output like this:
```plaintext
2024-10-17 09:31:23,539 WELCOME to Enola...
2024-10-17 09:31:23,540 authorized...
2024-10-17 09:31:23,540 STARTED!!!
```
This means you have successfully connected to Enola and initialized your first tracking agent!

### Understanding Steps in Enola-AI

In Enola-AI, the concept of **steps** is fundamental for tracking the execution flow of your AI agents. Each step represents a significant action or event in your agent's processing pipeline.

There are two main types of steps:

- **Generic Steps**: Used for general-purpose tracking of actions that are not specific to language models, such as data retrieval, preprocessing, or any custom logic.

- **LLM Steps**: Specifically designed for tracking interactions with Language Models (e.g., GPT-4, Ollama, BERT), where token usage and costs are relevant.

Understanding the difference between these step types is crucial for accurate tracking and cost analysis.

#### Generic Steps

Generic steps are used to track any action or process in your agent that doesn't involve a language model. This could include data retrieval, preprocessing, API calls, or custom computations.

**When to use Generic Steps:**

- Data fetching from databases or APIs.
- Data preprocessing and evaluation.
- Custom computations or business logic.
- Any step where token usage is not applicable.

#### LLM Steps

LLM Steps are used to track interactions with language models where token usage, input/output messages, and costs are important.

**When to use LLM Steps:**

- Sending prompts to a language model.
- Receiving responses from a language model.
- Tracking token counts and associated costs.

---

## Sending Online Chat Data

When working with conversational AI agents, it's essential to track user interactions and model responses. Enola-AI allows you to send online chat data for monitoring and evaluation.

### Step-by-Step Guide

1. **Import libraries**

   ```python
   # Import necessary libraries
   from enola.tracking import Tracking
   from dotenv import load_dotenv
   import os
   ```

2. **Load the Token**

   ```python
   # Load .env file and set up your token
   load_dotenv()
   token = os.getenv('ENOLA_TOKEN')
   ```
   
3. **Initialize the Tracking Object**

   Create an instance of the `Tracking` class to start tracking an execution.

   ```python
   # Initialize the tracking object
   monitor = Tracking(
       token=token,
       name="Chat Session Tracking V1",
       is_test=True,
       app_id="ChatApp",
       user_id="User123",
       session_id="Session456",
       channel_id="console",
       ip="192.168.1.1",
       message_input="What car offers good performance for a low cost?"
   )
   ```

4. **Create a New LLM Step**

   Since we're interacting with a language model, we'll create an LLM Step.

   ```python
   # Create an LLM Step
   step_chat = monitor.new_step("User Inquiry")
   ```

5. **Add Extra Information**

   You can add any additional information relevant to the step. In this case the user question.

   ```python
   # Add user's question
   step_chat.add_extra_info("UserQuestion", "What car offers good performance for a low cost?")
   ```

6. **Process the User Input with the Language Model**

   Suppose your model generates a response to the user's question.

   ```python
   # Simulate model response
   model_response = "The Honda Civic offers great performance at a reasonable price."
   ```

7. **Add the Model's Response to the Step**

   ```python
   # Add model's response
   step_chat.add_extra_info("ModelResponse", model_response)
   ```

8. **Close the LLM Step**

   Indicate that the step has completed successfully, and include token usage and costs.

   ```python
   # Close the LLM Step
   monitor.close_step_token(
       step=step_chat,
       successfull=True,
       message_output=model_response,
       token_input_num=12,       # Number of input tokens (estimated)
       token_output_num=15,      # Number of output tokens (estimated)
       token_total_cost=0.0025,  # Total cost (example)
       token_input_cost=0.001,   # Cost for input tokens
       token_output_cost=0.0015  # Cost for output tokens
   )
   ```

9. **Execute the Tracking**

    Send the tracking data to the Enola-AI server.

      ```python
      # Execute the tracking and send the data to Enola-AI server
      monitor.execute(
          successfull=True,
          message_output=model_response,
          num_iteratons=1
      )
      ```
    After executing the Tracking, you have successfully completed all the necessary steps in sending online chat data. You can view the data in the Enola web interface by logging in.
### Complete Example: Sending Online Chat Data

```python
# Import necessary libraries
from enola.tracking import Tracking
from dotenv import load_dotenv
import os

# Load .env file and set up your token
load_dotenv()
token = os.getenv('ENOLA_TOKEN')

# Initialize the tracking object
monitor = Tracking(
    token=token,
    name="Chat Session Tracking V1",
    is_test=True,
    app_id="ChatApp",
    user_id="User123",
    session_id="Session456",
    channel_id="console",
    ip="192.168.1.1",
    message_input="What car offers good performance for a low cost?"
)

# Create an LLM Step
step_chat = monitor.new_step("User Inquiry")

# Add user's question
step_chat.add_extra_info("UserQuestion", "What car offers good performance for a low cost?")

# Simulate model response
model_response = "The Honda Civic offers great performance at a reasonable price."

# Add model's response
step_chat.add_extra_info("ModelResponse", model_response)

# Close the LLM Step
monitor.close_step_token(
    step=step_chat,
    successfull=True,
    message_output=model_response,
    token_input_num=12,
    token_output_num=15,
    token_total_cost=0.0025,
    token_input_cost=0.001,
    token_output_cost=0.0015
)

# Execute the tracking and send the data to Enola-AI server
monitor.execute(
    successfull=True,
    message_output=model_response,
    num_iteratons=1
)
```
After executing the script and the tracking, you should get a console output like this:
```console
Chat Session Tracking V1: sending to server... 
Chat Session Tracking V1: finish OK! 
```
This means you have successfully sent the data to the Enola-AI servers and you will be able to monitor and evaluate your data.
```Note
Note: It is recommended to keep the user input stored in a variable to fill the message_input from the function Tracking.
```

- First you define the variable user_input and assign the message from the user
    ```Python
    user_input="What car offers good performance for a low cost?"
    ```
- Then replace the message from the user with the variable user_input inside Tracking:
    ```Python
    monitor = Tracking(
        token=token,
        name="Chat Session Tracking V1",
        is_test=True,
        app_id="ChatApp",
        user_id="User123",
        session_id="Session456",
        channel_id="console",
        ip="192.168.1.1",
        message_input=user_input
    )
    ```
```Note
This approach will be taken in the following examples for better message input management.
```
