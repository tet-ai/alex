# Alex: Practice Chatbot for Tet.ai

Alex is a first attempt to create a patient chatbot, allowing medical students - and other would-be doctors - to practice patient-doctor interactions and their diagnostic skills.

There are two main parts: the dialogue model and the NLU model. The NLU model can take natural language and convert it into structured data that a computer can understand. The dialogue model takes this structured data and chooses a suitable reply to it.

## Setup

### The All-in-One

1. #### Run this command in terminal

    ```bash
    pip install rasa_core
    pip install rasa_nlu[tensorflow]
    cd ~/
    git clone https://github.com/tet-ai/alex
    cd ~/alex/
    python -m rasa_core.train -d domain.yml -s stories.md -o models/dialogue
    python -m rasa_nlu.train -c nlu_config.yml --data nlu.md -o models --fixed_model_name nlu --project current --verbose
    python -m rasa_core.run -d models/dialogue -u models/current/nlu
    ```

    and you should be able to talk to the chatbot.

### In Baby Steps

1. #### Install Rasa

    ```bash
    pip install rasa_core
    pip install rasa_nlu[tensorflow]
    ```

    For more info or alternative installation methods, go [here](https://rasa.com/docs/core/installation/).

1. #### Clone the repository

    ```bash
    cd ~/
    git clone https://github.com/tet-ai/alex
    ```

1. #### Move to the repository

    ```bash
    cd ~/alex/
    ```

1. #### Train the dialogue model

    Run
    ```bash
    python -m rasa_core.train -d domain.yml -s stories.md -o models/dialogue
    ```

    This will train the dialogue model and store it into `models/dialogue`.

1. #### (Optional) Talk to bot with structured data

    We have only trained the dialogue model, not the NLU. But we can send structured data to the bot now.

    Run
    ```bash
    python -m rasa_core.run -d models/dialogue
    ```
    Then send it a message such as `/greet`.

1. #### Train NLU

    Run
    ```bash
    python -m rasa_nlu.train -c nlu_config.yml --data nlu.md -o models --fixed_model_name nlu --project current --verbose
    ```

    This creates the NLU model in `models/current/nlu`

1. #### Talk to Bot

    Now the bot is all trained up. To speak to it, run
    ```bash
    python -m rasa_core.run -d models/dialogue -u models/current/nlu
    ```

## Retraining the chatbot

If you make changes to the chatbot, you will need to retrain it.

```bash
python3 -m rasa_core.train -d domain.yml -s stories.md -o models/dialogue
python3 -m rasa_nlu.train -c nlu_config.yml --data nlu.md -o models --fixed_model_name nlu --project current --verbose
python3 -m rasa_core.run -d models/dialogue -u models/current/nlu
```