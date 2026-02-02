# main.py
# التثبيت الأول: pip install discord.py flask
import discord
import re
import os
from flask import Flask
from threading import Thread

# ========== سيرفر بسيط لتشغيل البوت 24/7 (Replit أو Render) ==========
app = Flask('')

@app.route('/')
def home():
    return "I'm alive!"

def run():
    app.run(host='0.0.0.0', port=8080)

def keep_alive():
    t = Thread(target=run)
    t.start()

keep_alive()
# ===============================================================

# إعداد الـ intents للسماح بقراءة محتوى الرسائل
intents = discord.Intents.default()
intents.message_content = True

# إنشاء عميل البوت
client = discord.Client(intents=intents)

# دالة لتنقية النص العربي من التشكيل والاختلافات
def normalize_arabic(text):
    text = text.lower()
    text = re.sub(r"[ًٌٍَُِّْـ]", "", text)  # إزالة التشكيل
    text = text.replace("ة", "ه")            # توحيد التاء المربوطة
    text = text.strip()
    return text

# نمط شامل لأنواع التحية (السلام)
greet_pattern = re.compile(r"\b(السلام|سلام)\s*عليكم", re.IGNORECASE)

@client.event
async def on_ready():
    print(f"✅ البوت اشتغل بنجاح: {client.user}")

@client.event
async def on_message(message):
    if message.author.bot:
        return  # تجاهل البوتات الأخرى

    content = normalize_arabic(message.content)
    if greet_pattern.search(content):
        reply_text = "**وعليكم السلام ورحمة الله وبركاتة <:8albMinecraft:965605504351080509>**"
        try:
            await message.reply(reply_text, mention_author=True)
        except discord.Forbidden:
            await message.channel.send(reply_text)

# تشغيل البوت من متغير البيئة
TOKEN = os.getenv("DISCORD_TOKEN")
if not TOKEN:
    print("❌ خطأ: لم يتم العثور على التوكن! أضفه في Secrets باسم DISCORD_TOKEN")
else:
    client.run(TOKEN)