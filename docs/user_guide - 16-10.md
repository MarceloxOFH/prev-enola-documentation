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
- **Dependencies**: Specified in `requirements.txt`

---

## Installation

Before installing the Enola-AI Python library, ensure that you have Python 3.7 or higher installed.

1. **Install the SDK via pip**

   ```bash
   pip install enola
   ```

2. **Optional: Create a Virtual Environment**

   It's recommended to use a virtual environment to manage your dependencies.
   If you are located in the Enola project folder, you can create a virtual environment using the following command:

   ```bash
   python3 -m venv enola-env        # On Linux/macOS
   python -m venv enola-env         # On Windows
   ```
    This command creates a directory called enola-env, which contains your isolated Python installation and any libraries you install.
    
    To use the virtual environment, you need to "activate" it. Depending on your operating system:
    ```bash
   source enola-env/bin/activate    # On Linux/macOS
   enola-env\Scripts\activate       # On Windows
   ```
   When the environment is activated, your terminal prompt should change to indicate that you are now working inside the virtual environment. Any pip install commands you run will install packages in this environment, not globally.
   
   Tip: To exit the virtual environment and return to your global environment, use the deactivate command:
    ```bash
   deactivate
   ```
   After deactivation, your terminal prompt will revert to its normal state.


3. **Install Dependencies**

   If you are installing from the source code, install the dependencies:

   ```bash
   pip install -r requirements.txt
   ```

---

## Getting Started

To start using the Enola-AI Python library, follow the steps below to initialize tracking in your application.

### Initializing Tracking

#### **1. Set Up Environment Variables**

You can set your Enola API token as an environment variable. Replace `'your_api_token'` with your actual token.
```bash
export ENOLA_TOKEN='your_api_token'     # On Linux/macOS
set ENOLA_TOKEN=your_api_token          # On Windows
```

Alternatively, you can load the token from a `.env` file (recommended) or 
set it directly in your script (not recommended for production):

To set it directly in your script:
```plaintext
token = 'your_api_token'
```
To load it from a `.env` file, you have to create the file name ".env" in the same directory as your Python script and set it like this:

```plaintext
ENOLA_TOKEN=your_api_token
```

Then you can load the .env file in your script
```python
from dotenv import load_dotenv
load_dotenv()
token = os.getenv("ENOLA_TOKEN")
```

#### **2. Import the Necessary Libraries**
Assuming you are loading the .env file:
```python
from enola.tracking import Tracking
from enola.enola_types import ErrOrWarnKind, DataType
from dotenv import load_dotenv
import os
```

#### **3. Initialize the Tracking Agent**

```python
# Load .env file
load_dotenv()
# Load the token from environment variables
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

---

#### **Example: Basic Tracking Initialization**

```python
import os
from enola.tracking import Tracking
from dotenv import load_dotenv

# Load .env file with token inside
load_dotenv()
# Set up your token
token = os.getenv('ENOLA_TOKEN')

# Initialize the tracking agent
monitor = Tracking(
    token=token,
    name="Customer Support Session",
    is_test=True,
    app_id="support_app_01",
    user_id="customer_001",
    session_id="Session_123",
    channel_id="console",
    ip="192.168.1.1",
    message_input="I need help with my order."
)
```
After executing the script and initializing the tracking agent, you should get a console output like this:
```plaintext
2024-10-02 09:31:23,539 WELCOME to Enola...
2024-10-02 09:31:23,540 authorized...
2024-10-02 09:31:23,540 STARTED!!!
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

LLM steps are used to track interactions with language models where token usage, input/output messages, and costs are important.

**When to use LLM Steps:**

- Sending prompts to a language model.
- Receiving responses from a language model.
- Tracking token counts and associated costs.

---

## Sending Online Chat Data

When working with conversational AI agents, it's essential to track user interactions and model responses. Enola-AI allows you to send online chat data for monitoring and evaluation.

### Step-by-Step Guide

1. **Import the Required Classes**

   ```python
   from enola.tracking import Tracking
   from enola.enola_types import ErrOrWarnKind
   from dotenv import load_dotenv
   import os
   ```

2. **Set Up the Authentication Token**

   You need a JWT token provided by Enola-AI. This token is essential for authentication and authorization purposes.

   ```python
   load_dotenv()
   token = os.getenv('ENOLA_TOKEN')  # Load your token from an environment variable
   ```
3. **Create variable storing User Input**

   For convenience, let's assume the variable user_input contains the User Input, which represents the question you are asking to the Model.
   
   ```python
   # Create variable representing the question you are asking to the Model
   user_input = "What car offers good performance for a low cost?"
   ```
    We will use this variable to fill the 'message_input' and initialize our Tracking Object in the next step.
    
4. **Initialize the Tracking Object**

   Create an instance of the `Tracking` class to start tracking an execution.

   ```python
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

5. **Create a New LLM Step**

   Since we're interacting with a language model, we'll create an LLM step.

   ```python
   step_chat = monitor.new_step("User Inquiry")
   ```

6. **Add Extra Information**

   You can add any additional information relevant to the step.

   ```python
   # User's question
   step_chat.add_extra_info("UserQuestion", user_input)
   ```

7. **Process the User Input with the Language Model**

   Suppose your model generates a response to the user's question.

   ```python
   # Model's response (simulated)
   model_response = "The Honda Civic offers great performance at a reasonable price."
   ```

8. **Add the Model's Response to the Step**

   ```python
   step_chat.add_extra_info("ModelResponse", model_response)
   ```

9. **Close the LLM Step**

   Indicate that the step has completed successfully, and include token usage and costs.

   ```python
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

10. **Execute the Tracking**

    Send the tracking data to the Enola-AI server.

      ```python
      monitor.execute(
          successfull=True,
          message_output=model_response,
          num_iteratons=1
      )
      ```
    After executing the Tracking, you have successfully completed all the necessary steps in sending online chat data. You can view the data in the Enola web interface by logging in.
### Complete Example

```python
from enola.tracking import Tracking
from enola.enola_types import ErrOrWarnKind
from dotenv import load_dotenv
import os

# Set up your token
load_dotenv()
token = os.getenv('ENOLA_TOKEN')

# Create variable representing the question you are asking to the Model
user_input = "What car offers good performance for a low cost?"

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
    message_input=user_input
)

# Create an LLM step
step_chat = monitor.new_step("User Inquiry")

# Add user's question
step_chat.add_extra_info("UserQuestion", user_input)

# Simulate model response
model_response = "The Honda Civic offers great performance at a reasonable price."

# Add model's response
step_chat.add_extra_info("ModelResponse", model_response)

# Close the LLM step
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

# Execute the tracking
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
This means you have successfully sent the data to the Enola servers and you will be able to monitor and evaluate your data.

---

## Sending Online Score Data

For AI models that generate scores (e.g., risk assessments, credit scores), you can send these scores for tracking and evaluation.

### Step-by-Step Guide

1. **Initialize the Tracking Object**

   ```python
   monitor = Tracking(
       token=token,
       name="Score Generation",
       is_test=False,
       app_id="ScoreApp",
       user_id="User789",
       message_input="Calculate credit score for customer ID 12345."
   )
   ```

2. **Create a Generic Step**

   Since we're performing a score calculation that doesn't involve token usage, we'll create a generic step.

   ```python
   step_score = monitor.new_step("Credit Score Calculation")
   ```

3. **Add Extra Information**

   ```python
   step_score.add_extra_info("CustomerID", "12345")
   step_score.add_extra_info("InputData", {"income": 50000, "debt": 10000})
   ```

4. **Set Score Values**

   Use the `set_score` method to assign the score.

   ```python
   step_score.set_score(
       value=750.0,
       group="Good",
       cluster="ClusterA",
       date="2023-10-01T12:00:00Z"
   )
   ```

5. **Close the Generic Step**

   ```python
   monitor.close_step_score(
       step=step_score,
       successfull=True,
       message_output="Credit score calculated successfully."
   )
   ```

6. **Execute the Tracking**

   ```python
   monitor.execute(
       successfull=True,
       message_output="Credit score is 750.",
       num_iteratons=1
   )
   ```

### Complete Example

```python
from enola.tracking import Tracking
import os

# Set up your token
token = os.getenv('ENOLA_TOKEN')

monitor = Tracking(
    token=token,
    name="Score Generation",
    is_test=False,
    app_id="ScoreApp",
    user_id="User789",
    message_input="Calculate credit score for customer ID 12345."
)

# Create a generic step for score calculation
step_score = monitor.new_step("Credit Score Calculation")
step_score.add_extra_info("CustomerID", "12345")
step_score.add_extra_info("InputData", {"income": 50000, "debt": 10000})

# Set the score
step_score.set_score(
    value=750.0,
    group="Good",
    cluster="ClusterA",
    date="2023-10-01T12:00:00Z"
)

# Close the score step
monitor.close_step_score(
    step=step_score,
    successfull=True,
    message_output="Credit score calculated successfully."
)

# Execute the tracking
monitor.execute(
    successfull=True,
    message_output="Credit score is 750.",
    num_iteratons=1
)
```

---

## Sending Multiple Tasks

If your agent performs multiple tasks or steps within a single execution, you can track each step individually.

### Step-by-Step Guide

1. **Initialize the Tracking Object**

   ```python
   monitor = Tracking(
       token=token,
       name="Multi-Step Execution",
       is_test=False,
       app_id="MultiTaskApp",
       user_id="User456",
       message_input="Process data and generate report."
   )
   ```

2. **Create and Close Multiple Steps**

   - **Step 1: Data Retrieval (Generic Step)**

     ```python
     step1 = monitor.new_step("Data Retrieval")
     step1.add_extra_info("DataSource", "Database XYZ")
     monitor.close_step_others(
         step=step1,
         successfull=True,
         message_output="Data retrieved successfully."
     )
     ```

   - **Step 2: Data Processing (Generic Step)**

     ```python
     step2 = monitor.new_step("Data Processing")
     step2.add_extra_info("RecordsProcessed", 1000)
     monitor.close_step_others(
         step=step2,
         successfull=True,
         message_output="Data processed successfully."
     )
     ```

   - **Step 3: Report Generation (LLM Step)**

     Suppose you're using a language model to generate a report.

     ```python
     step3 = monitor.new_step("Report Generation")
     report_content = "The processed data indicates a positive trend in sales."
     step3.add_extra_info("ReportContent", report_content)

     monitor.close_step_token(
         step=step3,
         successfull=True,
         message_output="Report generated successfully.",
         token_input_num=200,
         token_output_num=150,
         token_total_cost=0.01,
         token_input_cost=0.005,
         token_output_cost=0.005
     )
     ```

3. **Execute the Tracking**

   ```python
   monitor.execute(
       successfull=True,
       message_output="Multi-task execution completed.",
       num_iteratons=3
   )
   ```

### Complete Example

```python
from enola.tracking import Tracking
import os

# Set up your token
token = os.getenv('ENOLA_TOKEN')

monitor = Tracking(
    token=token,
    name="Multi-Step Execution",
    is_test=False,
    app_id="MultiTaskApp",
    user_id="User456",
    message_input="Process data and generate report."
)

# Step 1: Data Retrieval (Generic Step)
step1 = monitor.new_step("Data Retrieval")
step1.add_extra_info("DataSource", "Database XYZ")
monitor.close_step_others(
    step=step1,
    successfull=True,
    message_output="Data retrieved successfully."
)

# Step 2: Data Processing (Generic Step)
step2 = monitor.new_step("Data Processing")
step2.add_extra_info("RecordsProcessed", 1000)
monitor.close_step_others(
    step=step2,
    successfull=True,
    message_output="Data processed successfully."
)

# Step 3: Report Generation (LLM Step)
step3 = monitor.new_step("Report Generation")
report_content = "The processed data indicates a positive trend in sales."
step3.add_extra_info("ReportContent", report_content)

monitor.close_step_token(
    step=step3,
    successfull=True,
    message_output="Report generated successfully.",
    token_input_num=200,
    token_output_num=150,
    token_total_cost=0.01,
    token_input_cost=0.005,
    token_output_cost=0.005
)

# Execute the tracking
monitor.execute(
    successfull=True,
    message_output="Multi-task execution completed.",
    num_iteratons=3
)
```

---

## Sending File Information

If your agent processes files (e.g., images, documents), you can send file-related information for tracking.

### Step-by-Step Guide

1. **Create a New Step**

   ```python
   step_file = monitor.new_step("File Processing")
   ```

2. **Add File Information**

   Use the `add_file_link` method to include file details.

   ```python
   step_file.add_file_link(
       name="UserDocument",
       url="https://example.com/documents/userdoc.pdf",
       type="application/pdf",
       size_kb=2048,
       description="User uploaded document."
   )
   ```

3. **Close the Generic Step**

   Since processing files may not involve token usage, you can close it as a generic step.

   ```python
   monitor.close_step_doc(
       step=step_file,
       successfull=True,
       message_output="File processed successfully.",
       doc_num=1,
       doc_pages=10,
       doc_size=2048,
       doc_char=5000,
       doc_cost=0.005
   )
   ```

### Complete Example

```python
# Step: File Processing
step_file = monitor.new_step("File Processing")

# Add file information
step_file.add_file_link(
    name="UserDocument",
    url="https://example.com/documents/userdoc.pdf",
    type="application/pdf",
    size_kb=2048,
    description="User uploaded document."
)

# Close the file processing step
monitor.close_step_doc(
    step=step_file,
    successfull=True,
    message_output="File processed successfully.",
    doc_num=1,
    doc_pages=10,
    doc_size=2048,
    doc_char=5000,
    doc_cost=0.005
)
```

---

## Sending API Information

When your agent makes API calls, you can log the details of these calls for monitoring and debugging.

### Step-by-Step Guide

1. **Create a New Step**

   ```python
   step_api = monitor.new_step("External API Call")
   ```

2. **Add API Data**

   Use the `add_api_data` method to include API call details.

   ```python
   step_api.add_api_data(
       name="Weather API",
       method="GET",
       url="https://api.weather.com/v3/wx/forecast",
       bodyToSend="",
       headerToSend="{'Authorization': 'Bearer token'}",
       payloadReceived="{'temperature': '25°C'}",
       description="Fetching weather data for location X."
   )
   ```

3. **Close the Generic Step**

   ```python
   monitor.close_step_others(
       step=step_api,
       successfull=True,
       message_output="API call successful."
   )
   ```

### Complete Example

```python
# Step: External API Call
step_api = monitor.new_step("External API Call")

# Add API data
step_api.add_api_data(
    name="Weather API",
    method="GET",
    url="https://api.weather.com/v3/wx/forecast",
    bodyToSend="",
    headerToSend="{'Authorization': 'Bearer token'}",
    payloadReceived="{'temperature': '25°C'}",
    description="Fetching weather data for location X."
)

# Close the API step
monitor.close_step_others(
    step=step_api,
    successfull=True,
    message_output="API call successful."
)
```

---

## Sending Cost Information

Tracking the cost associated with different steps can help in monitoring and optimizing resource usage.

### Step-by-Step Guide

1. **Include Cost Information When Closing Steps**

   - **LLM Steps**

     When closing an LLM step, you can include token costs.

     ```python
     monitor.close_step_token(
         step=step_llm,
         successfull=True,
         message_output="LLM processing completed.",
         token_input_num=100,
         token_output_num=150,
         token_input_cost=0.0001,
         token_output_cost=0.00015,
         token_total_cost=0.00025
     )
     ```

   - **Generic Steps with Other Costs**

     For steps that don't involve tokens but have other costs (e.g., API calls, storage), use `others_cost`.

     ```python
     monitor.close_step_others(
         step=step_api,
         successfull=True,
         message_output="API call successful.",
         others_cost=0.002  # Cost of the API call
     )
     ```

   - **Image, Audio, or Video Steps**

     For steps involving images, audio, or video, use the respective closing methods and include cost parameters.

     ```python
     monitor.close_step_image(
         step=step_image,
         successfull=True,
         message_output="Image processed.",
         image_num=2,
         image_size=2048,
         image_cost=0.01
     )
     ```

### Complete Example

```python
# Close LLM step with token costs
monitor.close_step_token(
    step=step_llm,
    successfull=True,
    message_output="LLM processing completed.",
    token_input_num=100,
    token_output_num=150,
    token_input_cost=0.0001,
    token_output_cost=0.00015,
    token_total_cost=0.00025
)

# Close API step with other costs
monitor.close_step_others(
    step=step_api,
    successfull=True,
    message_output="API call successful.",
    others_cost=0.002
)

# Close image processing step with costs
monitor.close_step_image(
    step=step_image,
    successfull=True,
    message_output="Image processed.",
    image_num=2,
    image_size=2048,
    image_cost=0.01
)
```

---

## Sending Batch Score Data

For scenarios where you need to process and send a large number of scores (e.g., batch processing), you can use the `TrackingBatch` class.

### Step-by-Step Guide

1. **Import Required Classes**

   ```python
   from enola.tracking_batch import TrackingBatch
   import pandas as pd
   import os
   ```

2. **Prepare Your DataFrame**

   Your data should be in a Pandas DataFrame. Ensure that it contains the necessary columns.

   ```python
   data = {
       'client_id': ['C001', 'C002', 'C003'],
       'product_id': ['P001', 'P002', 'P003'],
       'score_value': [700, 650, 720],
       'score_group': ['Good', 'Average', 'Excellent'],
       'score_cluster': ['Cluster1', 'Cluster2', 'Cluster1']
   }
   df = pd.DataFrame(data)
   ```

3. **Initialize the TrackingBatch Object**

   ```python
   # Set up your token
   token = os.getenv('ENOLA_TOKEN')

   batch_monitor = TrackingBatch(
       token=token,
       name="Batch Score Processing",
       dataframe=df,
       period="2023-10-01T00:00:00Z",
       client_id_column_name="client_id",
       product_id_column_name="product_id",
       score_value_column_name="score_value",
       score_group_column_name="score_group",
       score_cluster_column_name="score_cluster",
       app_id="BatchApp",
       user_id="UserBatch",
       is_test=False
   )
   ```

4. **Execute the Batch Tracking**

   ```python
   batch_results = batch_monitor.execute(batch_size=100)
   ```

   The `batch_size` parameter controls how many records are sent per batch.

5. **Handle the Results**

   ```python
   for result in batch_results:
       if result.successfull:
           print(f"Record {result.enola_id} sent successfully.")
       else:
           print(f"Error sending record: {result.message}")
   ```

### Complete Example

```python
from enola.tracking_batch import TrackingBatch
import pandas as pd
import os

# Prepare the data
data = {
    'client_id': ['C001', 'C002', 'C003'],
    'product_id': ['P001', 'P002', 'P003'],
    'score_value': [700, 650, 720],
    'score_group': ['Good', 'Average', 'Excellent'],
    'score_cluster': ['Cluster1', 'Cluster2', 'Cluster1']
}
df = pd.DataFrame(data)

# Set up your token
token = os.getenv('ENOLA_TOKEN')

# Initialize TrackingBatch
batch_monitor = TrackingBatch(
    token=token,
    name="Batch Score Processing",
    dataframe=df,
    period="2023-10-01T00:00:00Z",
    client_id_column_name="client_id",
    product_id_column_name="product_id",
    score_value_column_name="score_value",
    score_group_column_name="score_group",
    score_cluster_column_name="score_cluster",
    app_id="BatchApp",
    user_id="UserBatch",
    is_test=False
)

# Execute batch tracking
batch_results = batch_monitor.execute(batch_size=100)

# Process results
for result in batch_results:
    if result.successfull:
        print(f"Record {result.enola_id} sent successfully.")
    else:
        print(f"Error sending record: {result.message}")
```

---

## Summary

This documentation provided a comprehensive guide on using the Enola-AI Python library to send various types of data for tracking and evaluation. By understanding and following the step-by-step instructions with examples, you can effectively monitor your AI models, ensure compliance with regulatory standards, and optimize performance.

---

## Contributing

Contributions are welcome! Please open an issue or submit a pull request for any improvements or corrections.

When contributing, please ensure to:

- Follow the existing coding style.
- Write clear commit messages.
- Update documentation as necessary.
- Ensure that any code changes are covered by tests.

---

## License

This project is licensed under the **BSD 3-Clause License**. See the [LICENSE](LICENSE) file for more details.

---

## Contact

For any inquiries or support, please contact us at [help@huemulsolutions.com](mailto:help@huemulsolutions.com).

