
import asyncio
import json
import discord
from discord.ext import commands, tasks
import youtube_dl
from random import choice
import requests




youtube_dl.utils.bug_reports_message = lambda: ''

ytdl_format_options = {
    'format': 'bestaudio/best',
    'outtmpl': '%(extractor)s-%(id)s-%(title)s.%(ext)s',
    'restrictfilenames': True,
    'nonplaylist' : True,
    'noncheckcertificate': True,
    'ignoreerrors': False,
    'logtostderr': False,
    'quiet': True,
    'no_warnings' : True,
    'default_search' : 'auto',
    'source_address' : '0.0.0.0'
}

ffmpeg_options = {
    'options' : '-vn'
}
ytdl = youtube_dl.YoutubeDL(ytdl_format_options)

class YTDLSource(discord.PCMVolumeTransformer):
    def __init__(self, source, *, data, volume=0.5):
        super().__init__(source,volume)

        self.data = data
        self.title = data.get('title')
        self.url = data.get('url')

    @classmethod
    async def from_url(cls, url, *, loop=None, stream=False):
        loop = loop or asyncio.get_event_loop()
        data = await loop.run_in_executor(None, lambda: ytdl.extract_info(url, download=not stream ))

        if 'entries' in data:
            data = data['entries'][0]

        filename = data['url'] if stream else ytdl.prepare_filename(data)
        return cls(discord.FFmpegPCMAudio(filename, **ffmpeg_options), data=data)

client = commands.Bot(command_prefix='?')

status = ['Jamming out to music!', 'Eating ', 'Sleeping']

@client.event
async def on_ready():
    change_status.start()
    print('bot is online!')

@client.event
async def on_member_join(member):
    channel = discord.utils.get(member.guild.channels, name='general')
    await channel.send(f'Welcome {member.mention}! Ready to jam out? see `?help` command for more information')



@client.command(name='ping', help='This command returns the latency')
async def ping(ctx):
    await ctx.send(f'**pong!** Latency: {round(client.latency*1000)}ms')

@client.command(name='hello', help='This command returns a random welcome message')
async def hello(ctx):
    responses = ['*** Grumble *** why did you wake me up?', 'Top of the morning to you lad!', 'Hii']
    await ctx.send(choice(responses))

@client.command(name='die', help='This command returns a random last words')
async def die(ctx):
    responses = ['Why have you brought my short life to an end?', 'I could try to do so much for you if i can be alive for long', 'I have a family please don’t kill me’]
    await ctx.send(choice(responses))
@client.command(name='creditz', help= 'This command returns the true credits' )
async def creditz(ctx):
    await ctx.send('***No one but me, lozer!***')

@client.command(name='play', help= 'This command plays the music' )
async def play(ctx, url=None):
    if not ctx.message.author.voice:
        await ctx.send('You are not connected to a voice channel')
        return
    else:
        channel = ctx.message.author.voice.channel

    await channel.connect()

    server = ctx.message.guild
    voice_channel = server.voice_client
    async with ctx.typing():
        player = await YTDLSource.from_url(url, loop=client.loop)
        voice_channel.play(player, after=lambda e: print('Player error: %s' % e) if e else None)


@client.command(name='stop', help= 'This command stops the music' )
async def stop(ctx):
    voice_client = ctx.message.guild.voice_client
    await voice_client.disconnect()


@tasks.loop(seconds=20)

async def change_status():
    await client.change_presence(activity=discord.Game(choice(status)))
def get_quote():
  response = requests.get("https://zenquotes.io/api/random")
  json_data = json.loads(response.text)
  quote = '```' + json_data[0]['q'] + " -" + json_data[0]['a'] + '```'

  return(quote)

@client.command(name='inspire', help= 'This command sends the inspirational quotes' )
async def inspire(ctx):
    await ctx.send(get_quote())



client.run(‘client-id’)
