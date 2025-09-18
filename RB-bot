import telebot      # Telegram Bot library
import requests     # To send HTTP requests
import time         # For delays
import os           # System interactions

# === FUNCTION TO READ BOT TOKEN ===
def read_token():
    try:
        with open("token.txt", "r") as file:
            return file.read().strip()
    except FileNotFoundError:
        print("❌ token.txt file not found. Please create token.txt and add your BOT TOKEN inside it.")
        exit()

# === CONFIGURATION ===
BOT_TOKEN = read_token()
REMOVE_BG_API_KEY = '3danqwQTBgDLxNFkD7hj2dSr'

bot = telebot.TeleBot(BOT_TOKEN)

# === TERMINAL BANNER ===
def show_banner():
    os.system("clear")  # Clears the terminal screen
    
    red = '\033[91m'
    dark = '\033[90m'
    bold = '\033[1m'
    reset = '\033[0m'

    banner = f"""{red}{bold}
  █████╗ ███╗   ███╗ █████╗ ██████╗      ██╗██╗████████╗
 ██╔══██╗████╗ ████║██╔══██╗██╔══██╗     ██║██║╚══██╔══╝
 ███████║██╔████╔██║███████║██████╔╝     ██║██║   ██║   
 ██╔══██║██║╚██╔╝██║██╔══██║██╔══██╗██   ██║██║   ██║   
 ██║  ██║██║ ╚═╝ ██║██║  ██║██║  ██║╚█████╔╝██║   ██║   
 ╚═╝  ╚═╝╚═╝     ╚═╝╚═╝  ╚═╝╚═╝  ╚═╝ ╚════╝ ╚═╝   ╚═╝   
    {reset}"""

    print(banner)
    print(f"{red}Create by Cyber Amarjit{reset}")
    print(f"{dark}Power by Black Devil 😈{reset}\n")
    print(f"{red}[INFO]{reset} Bot is starting...")
    time.sleep(1)
    print(f"{red}[INFO]{reset} Please wait...\n")
    time.sleep(1)

# === COMMAND HANDLERS ===

@bot.message_handler(commands=['start'])
def send_welcome(message):
    """
    Handles the /start command.
    """
    welcome_text = (
        "👋 *Welcome to @CyberAmarjit*\n\n"
        "🖼️ Send me an image, and I’ll remove its background for you instantly!"
    )
    bot.send_message(message.chat.id, welcome_text, parse_mode='Markdown')

@bot.message_handler(content_types=['photo'])
def handle_photo(message):
    """
    Processes incoming photos and removes background.
    """
    status = bot.send_message(message.chat.id, "⏳ Processing your image...")

    # Countdown animation
    for i in range(3, 0, -1):
        try:
            bot.edit_message_text(
                f"⏳ Removing background... {i}", 
                chat_id=message.chat.id, 
                message_id=status.message_id
            )
            time.sleep(1)
        except:
            pass  # Ignore errors in editing message

    # Fetch the photo
    try:
        file_id = message.photo[-1].file_id
        file_info = bot.get_file(file_id)
        file_url = f"https://api.telegram.org/file/bot{BOT_TOKEN}/{file_info.file_path}"
        photo_data = requests.get(file_url)

        if photo_data.status_code != 200:
            bot.reply_to(message, "❌ Could not download the image.")
            return

        # Send to remove.bg API
        remove_response = requests.post(
            'https://api.remove.bg/v1.0/removebg',
            files={'image_file': ('image.jpg', photo_data.content)},
            data={'size': 'auto'},
            headers={'X-Api-Key': REMOVE_BG_API_KEY}
        )

        if remove_response.status_code == 200:
            bot.send_document(
                message.chat.id,
                ("no_bg.png", remove_response.content),
                caption="✅ Background removed successfully.\n_Courtesy: @CyberAmarjit_",
                parse_mode='Markdown'
            )
        else:
            bot.reply_to(
                message,
                f"❌ API Error:\n{remove_response.status_code}\n{remove_response.text}"
            )

    except Exception as err:
        bot.reply_to(message, f"⚠️ Unexpected error:\n{str(err)}")

# === MAIN ===
if __name__ == "__main__":
    show_banner()
    bot.infinity_polling()
