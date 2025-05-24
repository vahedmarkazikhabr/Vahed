import os
import asyncio
from telethon import TelegramClient
from telethon.tl.functions.messages import GetHistoryRequest
from aiogram import Bot
from aiogram.types import ParseMode

API_ID = 1234567  # از my.telegram.org بگیر
API_HASH = 'your_api_hash'  # از my.telegram.org بگیر
BOT_TOKEN = os.getenv('BOT_TOKEN')
CHANNEL_ID = os.getenv('CHANNEL_ID')

client = TelegramClient('session', API_ID, API_HASH)
bot = Bot(token=BOT_TOKEN)

sources = ['@TeleTabnak', '@akhbarefori', '@Khabar_Fouri']

async def fetch_and_post():
    await client.start()
    for channel in sources:
        history = await client(GetHistoryRequest(
            peer=channel,
            limit=5,
            offset_date=None,
            offset_id=0,
            max_id=0,
            min_id=0,
            add_offset=0,
            hash=0
        ))
        for message in reversed(history.messages):
            text = message.message
            # TODO: بازنویسی و ترجمه اضافه کن اینجا
            await bot.send_message(CHANNEL_ID, text, parse_mode=ParseMode.HTML)

async def main():
    while True:
        try:
            await fetch_and_post()
        except Exception as e:
            print('Error:', e)
        await asyncio.sleep(300)

if __name__ == '__main__':
    asyncio.run(main())# Vahed
