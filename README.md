# Discord Music Bot

This is a simple Discord music bot that can play music from YouTube, send random welcome messages, respond to various commands, and provide inspirational quotes.

## Features

* Play music from YouTube using the ?play command.
* Stop playing music using the ?stop command.
* Respond to the ?ping command with the bot's latency.
* Send a random welcome message with the ?hello command.
* Respond with a random last word message with the ?die command.
* Credit the creator with the ?creditz command.
* Send an inspirational quote with the ?inspire command.
* Automatically changes its status every 20 seconds.

## Requirements

* Python 3.6 or higher
* discord.py library
* youtube_dl library
* requests library
* A Discord bot token

## Installation

1. Install the required Python libraries:
   ```
   pip install discord youtube_dl requests
   ```

2. Clone this repository:
   ```
   git clone https://github.com/tarunsai31302/DiscordMusicBot.git
   cd discord-music-bot
   ```
3. Create a .env file in the root directory and add your Discord bot token:

   ```
   DISCORD_TOKEN=your_bot_token_here
   ```

4. Run the bot:

   ```
   python bot.py
   ```

## Usage 

* Use the ?play [YouTube URL] command in a voice channel to start playing music.
* Use the ?stop command to stop the music and disconnect the bot from the voice channel.
* Use the ?ping command to check the bot's latency.
* Use the ?hello, ?die, and ?creditz commands for fun responses.
* Use the ?inspire command to get an inspirational quote.
