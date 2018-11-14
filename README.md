# Alex: Prototype Chatbot for [Tet.ai](https://www.tet.ai)

## Introduction

Alex is a prototype patient chatbot, allowing medical students - and other would-be doctors - to practice patient-doctor interactions and their diagnostic skills.

There are two main parts: the dialogue model and the NLU model. The NLU model can take natural language and convert it into structured data that a computer can understand. The dialogue model takes this structured data and chooses a suitable reply to it.

It can be run either locally, using the terminal, or running on Facebook Messenger using a websocket hosted on Heroku.

## Setup

### The All-in-One

1. #### Run this command in terminal

    ```bash
    pip install rasa_core
    pip install rasa_nlu[spacy]
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
    pip install rasa_nlu[spacy]
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


## Running for facebook

If setting up with facebook, follow [these instructions](https://rasa.com/docs/core/0.9.8/tutorial_basics/).

```bash
python -m rasa_core.run -d models/dialogue -u models/current/nlu \
   --port 5002 --connector facebook --credentials credentials.yml
   ```

```
python -m rasa_core.run -d models/dialogue -u models/current/nlu \
   --port 5002 --credentials credentials.yml
```

### To run locally for facebook

1. Expose local link
    ```bash
    ngrok http 5002
    ```
    see [ngrok](https://ngrok.com/).

    This makes random_name.ngrok.io point to localhost:5002

1.  - Run virtual environment
        ```bash
        source .env/bin/activate
        ```
    
    - Run Rasa
        ```bash
        python -m rasa_core.run -d models/dialogue -u models/current/nlu \
        --port 5002 --credentials credentials.yml
        ```

1. Create webhook
    ```bash
    node index.js
    ```

1. Point facebook in the right direction
    1. Go to application page on 'Facebook for developers'. Mine is [here](https://developers.facebook.com/apps/2161616560765921).
    1. Webhooks
    1. Edit subscription
    1. Callback URL: 'random_name.ngrok.io/webhooks/facebook/webhook'

1. Talk on messenger!