import os
from telegram import Update, InlineKeyboardButton, InlineKeyboardMarkup
from telegram.ext import Application, CommandHandler, CallbackQueryHandler, ContextTypes

TELEGRAM_CHANNEL = @easyincomebd1999_bot
TELEGRAM_CHANNEL_LINK = https://t.me/taskearanbd_bot
FACEBOOK_LINK_1 = https://www.facebook.com/share/1Gbb6jfftJ/
FACEBOOK_LINK_2 = https://www.facebook.com/share/1Hx5TEJ8pW/
YOUTUBE_LINK = "https://www.youtube.com/@Mymirza-h9b"

BOT_TOKEN =8448580097:AAGYySHI8jPt2U4QU9cVVAVjbH3d3U6mIbQ os.environ.get("BOT_TOKEN")

# ইউজারদের ব্যালেন্স জমা রাখার জন্য
user_data_store = {}

def get_user_data(user_id):
    if user_id not in user_data_store:
        user_data_store[user_id] = {"balance": 0, "fb1": False, "fb2": False, "yt": False}
    return user_data_store[user_id]

async def check_joined(update: Update, context: ContextTypes.DEFAULT_TYPE):
    user_id = update.effective_user.id
    try:
        member = await context.bot.get_chat_member(chat_id=TELEGRAM_CHANNEL, user_id=user_id)
        if member.status in ['member', 'administrator', 'creator']:
            return True
        return False
    except:
        return True

async def start(update: Update, context: ContextTypes.DEFAULT_TYPE):
    user = update.effective_user
    if not await check_joined(update, context):
        keyboard = [
            [InlineKeyboardButton("📢 চ্যানেলে জয়েন করুন", url=TELEGRAM_CHANNEL_LINK)],
            [InlineKeyboardButton("✅ জয়েন করেছি", callback_data="check_join")]
        ]
        await update.message.reply_text(f"হ্যালো {user.first_name} 👋\n\nআগে চ্যানেলে জয়েন করুন।", reply_markup=InlineKeyboardMarkup(keyboard))
        return
    await show_tasks(update, context)

async def show_tasks(update: Update, context: ContextTypes.DEFAULT_TYPE):
    user_id = update.effective_user.id
    data = get_user_data(user_id)
    balance = data["balance"]
    
    text = (
        f"💰 আপনার বর্তমান ব্যালেন্স: {balance} টাকা\n\n"
        f"নিচের কাজ গুলো করুন:\n\n"
        f"{'✅' if data['fb1'] else '❌'} ফেসবুক টাস্ক ১ - ২ টাকা {'(Done)' if data['fb1'] else ''}\n"
        f"{'✅' if data['fb2'] else '❌'} ফেসবুক টাস্ক ২ - ৩ টাকা {'(Done)' if data['fb2'] else ''}\n"
        f"{'✅' if data['yt'] else '❌'} ইউটিউব টাস্ক - ৬ টাকা {'(Done)' if data['yt'] else ''}\n\n"
        f"লিংকে ক্লিক করে কাজ করুন, তারপর 'Done' বাটনে ক্লিক করুন।"
    )

    keyboard = [
        [InlineKeyboardButton("📘 ফেসবুক ১ লিংক", url=FACEBOOK_LINK_1)],
        [InlineKeyboardButton("✅ ফেসবুক ১ Done (২৳)", callback_data="fb1_done")],
        [InlineKeyboardButton("📘 ফেসবুক ২ লিংক", url=FACEBOOK_LINK_2)],
        [InlineKeyboardButton("✅ ফেসবুক ২ Done (৩৳)", callback_data="fb2_done")],
        [InlineKeyboardButton("▶️ ইউটিউব লিংক", url=YOUTUBE_LINK)],
        [InlineKeyboardButton("✅ ইউটিউব Done (৬৳)", callback_data="yt_done")],
        [InlineKeyboardButton("💰 ব্যালেন্স রিফ্রেশ", callback_data="refresh")]
    ]
    
    markup = InlineKeyboardMarkup(keyboard)
    if update.message:
        await update.message.reply_text(text, reply_markup=markup)
    else:
        try:
            await update.callback_query.edit_message_text(text, reply_markup=markup)
        except:
            await update.callback_query.message.reply_text(text, reply_markup=markup)

async def button_handler(update: Update, context: ContextTypes.DEFAULT_TYPE):
    query = update.callback_query
    user_id = query.from_user.id
    data = get_user_data(user_id)

    if query.data == "check_join":
        if await check_joined(update, context):
            await show_tasks(update, context)
        else:
            await query.answer("❌ এখনো জয়েন করেননি!", show_alert=True)
        return

    if query.data == "fb1_done":
        if data["fb1"]:
            await query.answer("এই কাজটা আগেই করেছেন!", show_alert=True)
        else:
            data["fb1"] = True
            data["balance"] += 2
            await query.answer("✅ ২ টাকা যোগ হয়েছে!", show_alert=True)
    
    elif query.data == "fb2_done":
        if data["fb2"]:
            await query.answer("এই কাজটা আগেই করেছেন!", show_alert=True)
        else:
            data["fb2"] = True
            data["balance"] += 3
            await query.answer("✅ ৩ টাকা যোগ হয়েছে!", show_alert=True)

    elif query.data == "yt_done":
        if data["yt"]:
            await query.answer("এই কাজটা আগেই করেছেন!", show_alert=True)
        else:
            data["yt"] = True
            data["balance"] += 6
            await query.answer("✅ ৬ টাকা যোগ হয়েছে!", show_alert=True)

    elif query.data == "refresh":
        await query.answer()

    await show_tasks(update, context)

def main():
    app = Application.builder().token(BOT_TOKEN).build()
    app.add_handler(CommandHandler("start", start))
    app.add_handler(CallbackQueryHandler(button_handler))
    print("Bot Started...")
    app.run_polling()

if __name__ == "__main__":
    main()
