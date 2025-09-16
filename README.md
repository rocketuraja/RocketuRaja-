import telegram
import os
import random
import schedule
import time

# Replace with your actual Bot Token and Chat ID
BOT_TOKEN = "YOUR_BOT_TOKEN"
CHAT_ID = "YOUR_CHAT_ID"  # Can be a user ID, group ID, or channel username (e.g., '@your_channel_name')

# Initialize the bot
bot = telegram.Bot(token=BOT_TOKEN)

def post_random_image():
    """
    Selects a random image from a specified folder and posts it to the Telegram chat.
    """
    image_folder = "./images/"  # Make sure this folder exists and contains images
    
    # Check if the folder exists
    if not os.path.exists(image_folder):
        print(f"Error: The folder '{image_folder}' does not exist.")
        return

    # Get a list of image files
    image_files = [f for f in os.listdir(image_folder) if f.lower().endswith(('.png', '.jpg', '.jpeg', '.gif'))]

    if not image_files:
        print("No image files found in the specified folder.")
        return

    # Choose a random image
    random_image_name = random.choice(image_files)
    image_path = os.path.join(image_folder, random_image_name)

    # Send the image
    try:
        with open(image_path, 'rb') as photo_file:
            bot.send_photo(chat_id=CHAT_ID, photo=photo_file)
            print(f"Successfully posted: {random_image_name}")
    except Exception as e:
        print(f"Failed to post image. Error: {e}")

# --- Scheduling the post ---
# Schedule the job to run every day at 10:30 AM
schedule.every().day.at("10:30").do(post_random_image)

print("Bot is running and scheduled to post images.")

# Main loop to run the scheduler
while True:
    schedule.run_pending()
    time.sleep(1)
