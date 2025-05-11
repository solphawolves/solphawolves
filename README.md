

---

bot.py

import os
from telegram import Update, InlineKeyboardButton, InlineKeyboardMarkup
from telegram.ext import ApplicationBuilder, CommandHandler, ContextTypes, CallbackQueryHandler

ADMIN_USERNAME = "@solphaawolves"
WALLET_ADDRESS = "EGocDz6tJc4dzTrUgRyCYKag6azww2LKhtHDGbspsM4b"

WELCOME_MSG = """مرحباً بك في نادي SolphaWolves VIP 💎
خطوة واحدة فقط تفصلك عن الوصول إلى أقوى بوت ذكاء اصطناعي على شبكة سولانا!

🔹 مميزات العضوية:
• إشارات تداول حصرية 3x أكثر
• نسبة نجاح بين 70% - 90%
• وصول مبكر 20 ثانية قبل الجميع
• إشارات العملات تحت 40K
• استراتيجيات دخول وخروج خاصة
• وصول مبكر لإطلاقات التوكنات

اضغط /subscribe للاطلاع على خطط الاشتراك المتاحة.
"""

SUBSCRIBE_MSG = f"""اختر خطة الاشتراك VIP:

1️⃣ 0.7 SOL — تجربة ليوم واحد  
2️⃣ 1 SOL — اشتراك لمدة 15 يوم  
3️⃣ 3 SOL — اشتراك لمدة 30 يوم  

يرجى الإيداع إلى المحفظة التالية:  
{WALLET_ADDRESS}

بعد الإيداع، أرسل لقطة الشاشة لتأكيد الدفع إلى {ADMIN_USERNAME}.
"""

async def start(update: Update, context: ContextTypes.DEFAULT_TYPE):
    await update.message.reply_text(WELCOME_MSG)

async def subscribe(update: Update, context: ContextTypes.DEFAULT_TYPE):
    keyboard = [
        [InlineKeyboardButton("0.7 SOL - يوم", callback_data='plan1')],
        [InlineKeyboardButton("1 SOL - 15 يوم", callback_data='plan2')],
        [InlineKeyboardButton("3 SOL - 30 يوم", callback_data='plan3')],
    ]
    reply_markup = InlineKeyboardMarkup(keyboard)
    await update.message.reply_text(SUBSCRIBE_MSG, reply_markup=reply_markup)

async def button(update: Update, context: ContextTypes.DEFAULT_TYPE):
    query = update.callback_query
    await query.answer()
    await query.edit_message_text(text=f"يرجى إرسال إثبات الدفع إلى {ADMIN_USERNAME} لتفعيل اشتراكك.")

def main():
    TOKEN = os.getenv("BOT_TOKEN")  # قم بتحديده في بيئة Render
    app = ApplicationBuilder().token(TOKEN).build()

    app.add_handler(CommandHandler("start", start))
    app.add_handler(CommandHandler("subscribe", subscribe))
    app.add_handler(CallbackQueryHandler(button))

    print("البوت يعمل الآن...")
    app.run_polling()

if __name__ == "__main__":
    main()


