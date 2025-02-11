# Microsoft Teams Conversational Bot: DevOps Bot

This is a conversational bot for Microsoft Teams that demonstrates how you could build a DevOps bot. The bot uses the gpt-3.5-turbo model to chat with Teams users and perform DevOps action such as create, update, triage and summarize work items.

<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=6 orderedList=false} -->

<!-- code_chunk_output -->

- [Microsoft Teams Conversational Bot: DevOps Bot](#microsoft-teams-conversational-bot-devops-bot)
    - [Interacting with the bot](#interacting-with-the-bot)
    - [Setting up the sample](#setting-up-the-sample)
    - [Testing the sample](#testing-the-sample)
        - [Using Teams Toolkit for Visual Studio Code](#using-teams-toolkit-for-visual-studio-code)

<!-- /code_chunk_output -->

This sample illustrates basic conversational bot behavior in Microsoft Teams. The bot is built to allow GPT to facilitate the conversation on its behalf, using only a natural language prompt file to guide it.

## Interacting with the bot

You can interact with this bot by sending it a message. The bot will respond to the following strings.

1. **create work item to track new functionality in Adaptive card and assign it to John**

- **Result:** The bot will create a tracking item in Azure DevOps and assign it to John
- **Valid Scopes:** personal, group chat, team chat

2. **update title of work item 1 to create a new bot in azure**

- **Result:** The bot will update the title of work item 1 to create a new bot in azure.
- **Valid Scopes:** personal, group chat, team chat

3. **triage work item 1 as "in progress"**

- **Result:** The bot will update the state of work item 1 to "in progress".
- **Valid Scopes:** personal, group chat, team chat

4. **summarize work items"**

- **Result:** The bot will summarize the work items and respond back with an adaptive card.
- **Valid Scopes:** personal, group chat, team chat

You can select an option from the command list by typing `@TeamsConversationBot` into the compose message area and `What can I do?` text above the compose area.

## Setting up the sample

1. Clone the repository

    ```bash
    git clone https://github.com/Microsoft/teams-ai.git
    ```

> [!IMPORTANT]
> To prevent issues when installing dependencies after cloning the repo, copy or move the sample directory to it's own location first.
> If you opened this sample from the Sample Gallery in Teams Toolkit, you can skip to step 3.

1. If you do not have `yarn` installed, and want to run local bits, install it globally

    ```bash
    npm install -g yarn@1.21.1
    ```

1. In the root **JavaScript folder**, install and build all dependencies

    ```bash
    cd teams-ai/js
    # This will use the latest changes from teams-ai in the sample.
    yarn install #only needs to be run once, after clone or remote pull
    yarn build
    # To run using latest published version of teams-ai, do the following instead:
    cd teams-ai/js/samples/<this-sample-folder>
    npm install --workspaces=false
    npm run build
    ```

1. In a terminal, navigate to the sample root.

    ```bash
    cd samples/<this-sample-folder>/
    yarn start
    # If running the sample on published version of teams-ai
    npm start
    ```

1. Update any prompt `config.json` and `/src/index.ts` with your model deployment name.

1. If developing without Teams Toolkit, add your OpenAI or Azure OpenAI key to the `OPENAI_KEY` or `AZURE_OPENAI_KEY` and `AZURE_OPENAI_ENDPOINT` variable(s) in `.env` file, which you can copy from `sample.env`. If using TTK, continue following the directions below.

## Testing the sample

The easiest and fastest way to get up and running is with Teams Toolkit as your development guide.

Otherwise, if want to learn about the other ways to test a sample, use Teams Toolkit or Teams Toolkit CLI, and more, please see our documentation on [different ways to run samples](https://github.com/microsoft/teams-ai/tree/main/getting-started/OTHER#different-ways-to-run-the-samples).

To use Teams Toolkit, continue following the directions below.

### Using Teams Toolkit for Visual Studio Code

1. Add your OpenAI key to the `SECRET_OPENAI_KEY` variable in the `./env/.env.local.user` file.

If you are using Azure OpenAI then follow these steps:

- Comment the `SECRET_OPENAI_KEY` variable in the `./env/.env.local.user` file.
- Add your Azure OpenAI key and endpoint values to the `SECRET_AZURE_OPENAI_KEY` and `SECRET_AZURE_OPENAI_ENDPOINT` variables
- Open the `teamsapp.local.yml` file and modify the last step to use Azure OpenAI variables instead:

```yml
- uses: file/createOrUpdateEnvironmentFile
    with:
      target: ./.env
      envs:
        BOT_ID: ${{BOT_ID}}
        BOT_PASSWORD: ${{SECRET_BOT_PASSWORD}}
        #OPENAI_KEY: ${{SECRET_OPENAI_KEY}}
        AZURE_OPENAI_KEY: ${{SECRET_AZURE_OPENAI_KEY}}
        AZURE_OPENAI_ENDPOINT: ${{SECRET_AZURE_OPENAI_ENDPOINT}}
```

- Open `./infra/azure.bicep` and comment out lines 72-75 and uncomment lines 76-83.
- Open `./infra/azure.parameters.json` and replace lines 20-22 with:

```json
      "azureOpenAIKey": {
        "value": "${{SECRET_AZURE_OPENAI_KEY}}"
      },
      "azureOpenAIEndpoint": {
        "value": "${{SECRET_AZURE_OPENAI_ENDPOINT}}"
      }
```

1. Ensure you have downloaded and installed [Visual Studio Code](https://code.visualstudio.com/docs/setup/setup-overview)
1. Install the [Teams Toolkit extension](https://marketplace.visualstudio.com/items?itemName=TeamsDevApp.ms-teams-vscode-extension)
1. Copy this sample into a new folder outside of teams-ai
1. Select File > Open Folder in VS Code and choose this sample's directory
1. Using the extension, sign in with your Microsoft 365 account where you have permissions to upload custom apps
1. Verify that the Teams Toolkit extension is connected to your Teams account from the above step.
1. Select **Debug > Start Debugging** or **F5** to run the app in a Teams web client.
1. In the browser that launches, select the **Add** button to install the app to Teams.

> If you do not have permission to upload custom apps (sideloading), Teams Toolkit will recommend creating and using a Microsoft 365 Developer Program account - a free program to get your own dev environment sandbox that includes Teams.
