from typing import Final
from telegram import Update
from telegram.ext import Application, CommandHandler, MessageHandler, filters, ContextTypes

TOKEN: Final = '6773603590:AAHKW5yfVD1eUk1uqbwFFvOcf5ULNXxe4sY'
BOT_USERNAME: Final = '@William_bot'

# Commands

async def start_command(update: Update, context: ContextTypes.DEFAULT_TYPE):
    await update.message.reply_text("Oi eu sou William!")


async def help_command(update: Update, context: ContextTypes.DEFAULT_TYPE):
    await update.message.reply_text("Oi eu sou William!")


async def custom_command(update: Update, context: ContextTypes.DEFAULT_TYPE):
    await update.message.reply_text("Oi eu sou William!")

# Responses

def handler_response(text: str) -> str:
    processed: str = text.lower()
    if 'hello' in processed:
        return "Hey there"
    elif 'how are you' in processed:
        return "I am good"
    elif 'i love python' in processed:
        return "Remember to subscribe"
    else:
        return "I don't understand what you wrote..."

async def handler_message(update: Update, context: ContextTypes.DEFAULT_TYPE):
    message_type: str = update.message.chat.type
    text: str = update.message.text
    print(f"User ({update.message.chat.id}) in {message_type}: {text}")
    
    if message_type == 'group':
        if BOT_USERNAME in text:
            new_text: str = text.replace(BOT_USERNAME, '').strip()
            response: str = handler_response(new_text)
        else:
            return
    else:
        response: str = handler_response(text)
    
    print(f"Bot: {response}")
    await update.message.reply_text(response)

async def error(update: Update, context: ContextTypes.DEFAULT_TYPE):
    print(f"Update {update} caused error {context.error}")

if __name__ == '__main__':
    print("Starting bot...")
    app = Application.builder().token(TOKEN).build()

    # Commands
    app.add_handler(CommandHandler("start", start_command))
    app.add_handler(CommandHandler("help", help_command))
    app.add_handler(CommandHandler("custom", custom_command))

    # Messages
    app.add_handler(MessageHandler(filters.TEXT & (~filters.COMMAND), handler_message))

    # Errors
    app.add_error_handler(error)

    # Poll the bot
    print("Polling...")
    app.run_polling(poll_interval=3)



