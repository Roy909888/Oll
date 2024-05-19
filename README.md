import telebot
import requests
from bs4 import BeautifulSoup

bot = telebot.TeleBot("7059752053:AAHUQLXmpVZAwRJd_O2yn_WS1O7OkkaR7dE")

@bot.message_handler(commands=['start'])
def start(message):
    bot.reply_to(message, '¡Hola! Soy un bot de pronósticos deportivos. ¿Qué deporte te interesa?')
    bot.send_message(message.chat.id, 'Deportes:', reply_markup=generate_sports_buttons())

def generate_sports_buttons():
    keyboard = telebot.types.ReplyKeyboardMarkup(one_time_keyboard=True, resize_keyboard=True)
    keyboard.add('Fútbol', 'Tenis', 'Baloncesto')
    return keyboard

@bot.message_handler(func=lambda message: True)
def handle_sport_selection(message):
    sport = message.text
    predictions = get_predictions(sport)
    bot.send_message(message.chat.id, f'Pronósticos para {sport}:')
    for prediction in predictions:
        bot.send_message(message.chat.id, prediction)

def get_predictions(sport):
    if sport == 'Fútbol':
        url = 'https://www.resultados-futbol.com/pronosticos/'
    elif sport == 'Tenis':
        url = 'https://www.oddschecker.com/tennis/match-odds'
    elif sport == 'Baloncesto':
        url = 'https://www.apuestasdeportivas.com/pronosticos/baloncesto'

    page = requests.get(url).text
    soup = BeautifulSoup(page, 'html.parser')
    predictions = []
    for prediction in soup.find_all('div', class_='prediction'):
        predictions.append(prediction.text)

    return predictions

bot.polling()
bot.set_webhook(https://github.com/Roy909888/Oll/edit/main/README.py)
