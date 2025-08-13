import discord
import random
import os
import requests
from discord.ext import commands

intents = discord.Intents.default()
intents.message_content = True

bot = commands.Bot(command_prefix='!', intents=intents)

@bot.event
async def on_ready():
    print(f'{bot.user} olarak giriş yaptık')

@bot.command()
async def hello(ctx):
    await ctx.send(f'Merhaba! Ben {bot.user}, bir Discord sohbet botuyum!')

@bot.command()
async def heh(ctx, count_heh = 5):
    await ctx.send("he" * count_heh)

@bot.command()
async def aboutyou(ctx):
    await ctx.send(f"Ben ne yapıcağı değiştirilebilir ve insanlara yardım etmek amacıyla yapılmış bir Discord botu olmak tayım ve ismim {bot.user}")

@bot.command()
async def mem(ctx):
    files = os.listdir('images')
    selected_file = random.choice(files)
    with open(f'images/{selected_file}', 'rb') as f:
        picture = discord.File(f)
    await ctx.send(file=picture)   

def get_duck_image_url():    
    url = 'https://random-d.uk/api/random'
    res = requests.get(url)
    data = res.json()
    return data['url']


@bot.command('duck')
async def duck(ctx):
    '''duck komutunu çağırdığımızda, program ordek_resmi_urlsi_al fonksiyonunu çağırır.'''
    image_url = get_duck_image_url()
    await ctx.send(image_url)

bot.run("")
