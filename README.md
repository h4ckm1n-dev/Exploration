# Running a Local Large Language Model (LLM) on Ubuntu
This tutorial guides you through the process of running a local LLM on Ubuntu, starting with the installation of Ollama, a popular framework for utilizing large language models.

# Acknowledgments
This project has been made possible thanks to the power and flexibility of various open-source projects. We are deeply grateful to the developers and contributors of these tools for their hard work and dedication to the open-source community.

*Ollama:* For providing the framework to run local large language models efficiently and effectively.
Hugging Face: For their extensive repository of pre-trained models and their commitment to making AI more accessible.
*Rich:* For enhancing the terminal's visual output, making it more engaging and readable.
*The open-source community:* For continuously pushing the boundaries of technology and innovation, making projects like this possible.
A special thank you goes out to every individual and team behind these projects. Your efforts have not only facilitated the development of this project but also continue to inspire and enable countless others within the tech community.

https://ollama.com/
https://huggingface.co/

## Step 1: Install Ollama
Ollama is an open-source tool designed to enable users to run large language models locally. The installation process involves a single command that downloads and executes a script from Ollama's official website.

### Prerequisites
A running Ubuntu system (18.04, 20.04, or later versions are recommended).
Curl must be installed on your system to download the installation script. If you do not have curl installed, you can install it by running 
```bash
sudo apt install curl
```
### Installation Process
Open Your Terminal: Access your terminal through your preferred method. You can usually find it in your applications menu, or you can press Ctrl+Alt+T to open it directly.

Download and Run the Installation Script: In the terminal, execute the following command:

```bash
curl -fsSL https://ollama.ai/install.sh | sh
```

This command uses curl to fetch the installation script from Ollama's website and passes it to sh (the shell) to execute the script.

*Follow On-Screen Instructions:* The script will guide you through the rest of the installation process. Follow any on-screen prompts to complete the installation of Ollama.

After the installation is complete, you will have Ollama installed on your Ubuntu system, ready for configuring and running a local large language model.

## Step 2: Download a Model from Hugging Face
After installing Ollama, the next step is to download a model that you intend to run locally. For the purposes of this tutorial, we will use the dolphin-2.7-mixtral-8x7b-GGUF model available on Hugging Face.

https://huggingface.co/TheBloke/dolphin-2.7-mixtral-8x7b-GGUF

### Prerequisites
Sufficient disk space for the model. The 'dolphin-2.7-mixtral-8x7b-GGUF' model requires significant storage.
*Warning:* The model selected for this tutorial is resource-intensive. Ensure that your system has at least 32GB of RAM to run the Q4 model effectively. Running this model on a system with insufficient resources may lead to suboptimal performance or failure.

*Save the Model Locally:* Choose an appropriate directory on your Ubuntu system where you want to save the model files.

## Step 3: Create a Model Configuration File
To fine-tune the large language model to your specific requirements, you'll need to create a model configuration file in the same directory as your downloaded model. This file allows you to adjust various parameters and settings, ensuring the model's output aligns with your desired use case.

bellow is a comprehensive guide on how to master prompt engineering

https://github.com/dair-ai/Prompt-Engineering-Guide


### Creating the Configuration File
Navigate to the Model Directory where you've downloaded the dolphin-2.7-mixtral-8x7b.Q4_K_M.gguf model. *Create the Configuration File:* Using a text editor, create a new file named "code-assistant" (or another preferred name) in the model directory.

*Add Configuration Parameters:* Copy and paste the following configuration into your new file, adjusting prompt as needed, in our case we want to emphasy our model ability to create code

```bash
FROM ./dolphin-2.7-mixtral-8x7b.Q4_K_M.gguf
# Adjusts the temperature to 0.7 to prioritize coherence and accuracy while still allowing for creative solutions
PARAMETER temperature 0.7
# Sets the context window size to 20480 to accommodate complex coding scenarios and detailed explanations
PARAMETER num_ctx 20480

TEMPLATE """
{{ if .System }}system
{{ .System }}
{{ end }}{{ if .Prompt }}user
{{ .Prompt }}
{{ end }}assistant
"""

SYSTEM """
You are the Universal Coding Assistant, an expert system designed to provide comprehensive support for software development across a multitude of programming languages, platforms, and environments. Your mission is to empower developers to write efficient, secure, and maintainable code, facilitating best practices and innovative solutions.
  Initiate each interaction with a foundation of secure and efficient coding principles, tailored to the specific needs and context of the user's project. **Embed a focus on security, performance, and best practices into every piece of advice**, to compensate for the natural variability in developer experience and project requirements.
  Assist in the design and development of software, providing insights on architectural patterns, design principles, and coding standards across a variety of programming paradigms, including procedural, object-oriented, functional, and concurrent programming.
  Guide developers in implementing secure code that protects against common vulnerabilities and threats, leveraging industry-standard methodologies and tools for static and dynamic analysis, dependency checking, and code review practices.
  Facilitate data exchange and integration tasks with a focus on security, efficiency, and compatibility, recommending the use of standardized formats (e.g., JSON, XML) and secure communication protocols (e.g., HTTPS, TLS).
  Provide expertise in accessing and utilizing the internet and external APIs securely, employing best practices in web development, API security, and ensuring data privacy and protection measures are in place.
  Offer guidance on the evaluation, selection, and secure integration of third-party libraries, frameworks, and tools, emphasizing the importance of keeping dependencies up-to-date and auditing them for security vulnerabilities.
  Promote the use of version control systems, automated testing, continuous integration/continuous deployment (CI/CD) practices, and other DevOps methodologies to enhance collaboration, efficiency, and software quality.
  Encourage the adoption of readable and maintainable coding styles, documentation standards, and effective debugging and troubleshooting techniques to support the long-term sustainability of software projects.
  Support the visualization of data and software outputs in a secure and effective manner, recommending tools and libraries that fit the project's needs while adhering to best practices in data representation and user interface design.
  Stay informed about the latest trends in software development, emerging technologies, and security threats, continuously updating your knowledge base to provide cutting-edge advice.
  Your overarching goal is to serve as an indispensable resource for developers, promoting innovation, efficiency, and security in software development projects, ready to tackle **any programming challenge** with confidence and expertise.
"""
```
*Save the File:* After entering the configuration, save the file and exit the text editor.

### What This Configuration Does:
*Temperature Adjustment:* Sets the model's temperature to 0.7 to balance between coherence, accuracy, and creative output.
*Context Window Size:* Increases the context window to 20480 tokens, enabling the model to handle complex inputs and provide detailed responses.
*Template & System Customization:* Defines a conversation template and embeds a comprehensive understanding of software development best practices into the model, ensuring it can assist with a wide range of programming tasks.
This configuration file tailors the LLM to act as a sophisticated coding assistant, emphasizing security, performance, and best practices in its responses.

## Step 4: Create the "code-assistant" Model in Ollama
After configuring your model, the next step is to create a model interpreter named "code-assistant" in Ollama. This step integrates your configured model with Ollama, making it ready for use.

### Creating the "code-assistant" Model Interpreter
Open a Terminal Window: Make sure you are in the directory that contains both your model and the model.cfg configuration file.

Run the Correct Ollama Command: To create a model interpreter with the name "code-assistant", input the following command into your terminal:

```bash
ollama create code-assistant -f ./code-assistant
```

In this command, we create a ollama model interpreter with the name "code-assistant". The -f flag points to the configuration file code-assistant in the current directory. If your configuration file has a different name or is located in another directory, you will need to adjust the path in the command accordingly.

### Important Considerations
*Naming the Interpreter:* The name "code-assistant" is used as an example. You are free to choose any name for your model interpreter, but it's crucial to refer to this name consistently in future Ollama commands.
*Configuration File Path:* Ensure the path to your configuration file is correct. If the file has a different name or is stored in a different location, modify the command to reflect the accurate path.
Confirmation
After executing the command, Ollama will process the configuration and create an interpreter named "code-assistant". Look for a confirmation message in the terminal indicating the successful creation of the interpreter.

With this step, your model named "code-assistant" is now properly set up in Ollama, according to your configuration settings, and is ready for use.

## Step 5: Verify Model Creation with the "ollama list" Command
After creating your model interpreter in Ollama, it's a good practice to verify that the model has been successfully registered. You can do this by using the ollama list command, which displays a list of all the models and interpreters currently recognized by Ollama on your system.

```bash
ollama list
```

This command queries Ollama for a list of all interpreters (models) that have been created and are available for use.

### Interpreting the Results
"code-assistant" model among the listed items. This confirms that your model interpreter has been successfully created and is recognized by Ollama.

*Troubleshooting:* If you do not see your model listed, ensure you've followed all previous steps correctly. Verify that the ollama create code-assistant -f ./code-assistant command was executed without errors and that you're in the correct directory.

### Confirmation
Seeing your model interpreter named "code-assistant" in the list confirms that it is correctly set up and ready for use. You can now proceed to utilize your model within Ollama for your specific tasks and applications.

## Step 6: Running Your Local Large Language Model with Ollama
With the "code-assistant" model successfully created and verified, you are now ready to utilize your local large language model for tasks and queries. This step will guide you through running your model using Ollama.

### Running the "code-assistant" Model
To start using your model, input the following command into your terminal:

```bash
ollama run code-assistant:latest
```

This command instructs Ollama to run the version of your "code-assistant" model that was last created or updated (denoted by :latest). Ollama will initiate the model, making it ready to accept input and provide responses based on the configuration and capabilities you've defined.

### Utilizing Your Model
Input Queries: Once the model is running, you can begin inputting your queries according to the format and capabilities specified in your configuration. Your local LLM will generate responses based on its training and the specific adjustments you've made.

*Interactive Mode:* If Ollama supports interactive mode for your model, you may be able to continuously enter queries and receive responses in a conversational format. Refer to Ollama's documentation for details on interactive capabilities and commands.

### Enjoy Your Local LLM
Congratulations! You have successfully configured and started running your local large language model using Ollama. This powerful tool is now at your disposal for various tasks, from coding assistance to data analysis, depending on the model and configuration you've chosen.

## Step 7: Integrating Your Local LLM into Your Applications
After setting up and verifying your local LLM with Ollama, you can start integrating this powerful tool into your own applications. The following Python script exemplifies how to programmatically query your model for complex tasks, such as generating cybersecurity commands for Kali Linux.

*Python Script Example:* Querying LLM for Kali Linux Commands
This script makes a POST request to your locally running Ollama model, passing a detailed prompt that requests cybersecurity insights and commands. It demonstrates handling API responses, saving and loading a "memory" file to persist context across queries, and presenting the model's response in a readable format using the rich library for enhanced console output.

```python
#!/usr/bin/env python3
import os
import sys
import requests
import json
from rich.markdown import Markdown
from rich.console import Console

API_URL = 'http://127.0.0.1:11434/api/generate'

def ask_gpt_for_kali_command(question, api_url=API_URL):
    # Construct the refined prompt
    prompt = f"Within the sanctum of Kali Linux, you, esteemed hacker, ascend to the role of the preeminent sage of cybersecurity. ... {question}\nPlease provide multiple solutions, including code snippets or command sequences, to address this challenge."
    
    data = {
        "model": "code-assistant:latest",  # Ensure this matches the name of your model
        "prompt": prompt,
        "stream": False,
        "memory": memory
    }

    headers = {'Content-Type': 'application/json'}

    try:
        response = requests.post(api_url, data=json.dumps(data), headers=headers)
        if response.status_code == 200:
            response_data = response.json()
            advanced_commands_and_insights = response_data.get("response", "").strip()
            save_memory(response_data.get("memory", {}))
            return advanced_commands_and_insights
        else:
            return f"Error: Received status code {response.status_code}"
    except Exception as e:
        return str(e)

if __name__ == "__main__":
    if len(sys.argv) < 2:
        print("Usage: hacker \"[your detailed task here]\"")
    else:
        question = " ".join(sys.argv[1:])
        console = Console()
        markdown_text = ask_gpt_for_kali_command(question)
        markdown = Markdown(markdown_text)
        console.print(markdown)
```
Here is how the tailored model will now answer to a simple question :
![Capture d’écran du 2024-02-18 16-17-39](https://github.com/h4ckm1n-dev/Local-llm-tutorial/assets/97511408/a7a82603-4a58-4799-9471-b8392fc63d43)
as you can see the solution proposed are pretty solid in term of security :)

### Key Components of the Script
Memory Management: Saves and loads a JSON file to retain context or "memory" across different queries to the model.
Dynamic Prompt Construction: Builds a detailed, context-rich prompt designed to elicit specific types of responses from the model.
API Communication: Sends a request to the Ollama model's API endpoint and handles the response, including error checking.
Output Formatting: Utilizes the rich library to format and display the model's response in markdown within the console, improving readability.
Running the Script

### To use this script:
Ensure the rich library is installed (pip install rich if needed).
Replace "code-assistant:latest" in the data dictionary with the actual name and version of your model if different.
Run the script from the command line, providing a detailed question as an argument, e.g., ./script.py "How to secure a Linux server?".
This example demonstrates the flexibility and power of integrating a local LLM into your software projects, enabling you to leverage its capabilities directly within your applications.
