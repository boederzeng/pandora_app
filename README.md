
# Pandora App

Pandora App is a Streamlit interface for the [pandora_ai](https://github.com/B4PT0R/pandora_ai) GPT4-console. It's both a fully working python console and a GPT4 assistant capable of running code, all wrapped up in a web interface thanks to Streamlit. It's main feature is to provide a special tool (a [st_stacker](https://github.com/B4PT0R/streamlit_stacker) object) declared as `st` in the console from which you may run streamlit commands interactively and render widgets in the chat dynamically (while it's running). The AI agent may as well interact with `st` to render any streamlit widget in the console, empowering it with super rich output capabilities (TTS and STT, markdown, latex, plotting tools, dataframes, image/audio/video players, gui elements with callbacks, html/javascript iframes, or even custom react components) which offer a wide multimodal channel of interaction with the user.

The App is designed to be user-friendly yet as powerful as an AI python console can be.

## Demo Screenshot

![Pandra screenshot](./pandora_app/app_images/pandora_demo.jpg)

## Main Features

- **Python Console**: Execute Python commands/scripts in real time as in a conventional Python console, the AI can help you in your workflow at any time thanks to its continuous observation of the session and its capacity to generate and run scripts. You can even mix natural language sentences with segments of python code in the same input script!

- **Multilingual**: Interact with the assistant in many languages thanks to its speech recognition and vocal synthesis capabilities (OpenAI).

- **Data/File/Image Analysis**: Transmit files or images for analysis. The Agent can observe text, files, data structures, python objects and modules (for documentation and inspection), as well as images thanks to its vision feature.

- **Dynamic Streamlit Interface and Interactive Widgets**: You or the AI can use the full range of Streamlit commands via the console to generate widgets in the chat interface in real time.

- **Image Generation**: Create images from textual descriptions with DALL-e 3.

- **LaTeX to PDF Conversion**: Generation of aesthetic documents via the conversion of .tex files into PDF documents and display them in a pdf reader.

- **Web search and scraping**: Perform web searches and read the content of web pages,or use a preimplemented selenium webdriver to interact, take webshots or extract data from webpages.

- **Personal folder/Cloud storage**: Acces your files and preferences from anywhere thanks to cloud storage of your user folder.

## Installation

Local installation:
```bash
$ pip install pandora-app
```

## Usage

open a terminal and run :

```
$ pandora
```

The app will start a local webserver and launch in your default webbrowser.

Alternatively you may use the web-app here:
[![Streamlit App](https://static.streamlit.io/badges/streamlit_badge_black_white.svg)](https://pandora-ai.streamlit.app/)

The installed app is local and runs python code locally on your system, but user connexion and cloud storage are managed via a cloud provider, so that your documents can be synchronized with the web application.

You will thus need to create an account and authenticate to use the app (free).

Pandora requires an OpenAI API key to enable the AI features. Your API key can be entered from the app, provided from your system as an environment variable or can be kept safely encrypted in your user profile within the database so that you may use pandora from anywhere, including from your smartphone thanks to the Streamlit cloud version of the app.

That's it, you can start typing your python commands or interact with Pandora in natural language via the input cell.

## A few explanations and tips:

- **Security** : Your account is managed via firebase authentication. Even as the admin, I don't have access to your password. Proper security rules are set in the firebase project to prevent anyone but you accessing your data.
- **API keys**: all API keys needed to enable the AI features or other tools are transmitted and stored safely encrypted with your password in firebase. All efforts are made to ensure safe transmission and storage of sensitive data.
- **Websearch**: In order to enable the websearch tool of the AI agent, you may provide your google custom search API key and CX in the Settings page.
- **Storage**: All files you create during a session are dumped to firebase storage when you log out. It's therefore recommended that you log out gracefully to avoid your work being lost. To prevent unwanted data loss you may call the `dump_workfolder()` function at any time in the console to upload your files to the cloud storage. When using the web app, your user folder will be wiped from the app's file system once you log out. Your files will be uploaded again from firebase storage when you sign in.
- **Stdin redirection**: When using a python command that wants to read from `stdin` such as the `input` command, the script will pause and a special input widget will render to let you enter a string. This string will be available immediately when your script resumes execution (without requiring a rerun). You can therefore use the `input` command in your scripts seamlessly. 
-  **Shortcuts**: Running `exit()` or `quit()` commands in the console will log you out gracefully. Running `clear()` in the console will clear the chat (This won't affect the python session and context memory of the AI agent).
- **Restart session**: You can restart the python session by clicking the 'Restart Session' button in the sidebar menu. This will also reinitialize the AI agent to its startup state. You can achieve a similar result by running `restart()` in the console.
- **Editor**: A text editor is provided within the app to enable opening and editing files. You may open it via the 'Open editor' button in the sidebar. Alternatively you may use the `edit` function directly from the console. `edit(file=your_file,text=your_text)` will open your file in the editor, prefilled with an optional string of text.
- **Pandora as a python object**: Pandora (the AI assistant) is declared as a python object in the console under the name `pandora`. You may thus interact with it programmatically.
- **Observation**: Pandora is equipped with a special `observe` tool enabling it to look at the content of any folder, file, data structure, image... When applied to a module, class or function this will inspect the object and access its documentation. You may thus ask Pandora to observe almost anything to get information about it, including the `pandora` object itself !
- **Hybrid scripting**: Pandora supports mixing both natural language and python code in the same input script. A parser determines which is which based on python syntactic correctness of each line. Natural language parts will be sent as prompts to the AI assistant and rendered as chat messages, the python code parts will be executed directly in the interpreter without triggering an assistant response. Beware that single word inputs like "Hello" will be interpreted as an attempt to access a variable by the parser and will most likely result in a "not defined" exception. To avoid this, you should use punctuation to help the parser understand your word is meant to be a message to the assistant: "Hello!" 
- **Web scraping**: Pandora comes equipped with a headless firefox webdriver in its toolkit. Useful to make it navigate through webpages, take webshots, interact with the DOM object, scrap data... You can use this web-driver yourself by running `driver=get_webdriver()`. In order to use it from the local client you should have firefox installed on you computer.
- **LaTeX and PDF**: The web-app comes equipped with a minimal LaTeX distribution enabling Pandora to use pdflatex to generate pdf documents from .tex files. A dedicated tool is declared in the console as `tex_to_pdf(tex_file,pdf_file)`. To be able to use it via the local app, you should have a LaTeX distribution and pdflatex installed on your computer. A custom streamlit widget enables displaying pdf files in the chat, you can use it via the `show_pdf(file_or_url)` shortcut.
- **Memory**: Pandora uses a `memory.json` file associated to your user profile for storing and remembering any kind of information across sessions. The assistant has permanent contextual visibility on the memory content. You can use it to guide the assistant towards the desired behavior, save user information, preferences or memos. Just ask Pandora and it will remember something durably.
- **Startup**: A `startup.py` file specific to your user profile will be executed whenever a new session starts. You may use it to pre-declare your favorite functions/tools to avoid having to declare them manually every new session. You may also use it to declare custom tools the agent will be able to use via the `pandora.add_tool` method. You will find the `memory.json` and `startup.py` files in the `config` folder of your workfolder. Feel free to edit them with the built-in editor.

## Use cases

- **Python Programming Learning**: Use Pandora to learn Python with interactive examples and real-time explanations.

- **Data Analysis, Python Development, or Research Assistance**: Take advantage of Pandora's expertise to analyze data, write scripts, or perform complex research.

- **Automatic Document/Image Generation**: Ask Pandora to generate documents or images based on textual descriptions.

- **Web Content Search and Extraction**: Use Pandora to navigate, find an extract information on the Internet thanks to its headless firefox webriver.

- **Modular use of custom tools you provide to the AI**: Pass the AI any new tool to play with (custom functions, python objects, APIs,...)

- **Productivity assistant**: Benefit from the AI's vast technical knowledge, data analysis capabilities, and long lasting memory to manage and speed up complex projects.

- **Interactive Content Creation**: Ask the assistant to help you create code, documentation, tutorials, demonstrations, beautiful logos or images, or intractive presentations using Streamlit widgets.


## License

This project is licensed. Please see the LICENSE file for more details.

## Contributions

Contributions are welcome. Please open an issue or a pull request to suggest changes or additions.

## Contact

For any questions or support requests, please contact Baptiste Ferrand at the following address: bferrand.maths@gmail.com.
