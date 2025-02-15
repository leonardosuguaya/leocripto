import os
from telegram import Update
from telegram.ext import Updater, CommandHandler, CallbackContext
import requests

# Coloque o seu token do Telegram aqui
TOKEN = '7730209368:AAFrXnaOrGRmNBp8WzS1ktwBe1jqxZNAhOY'

def start(update: Update, context: CallbackContext) -> None:
    update.message.reply_text('Olá! Eu sou o seu bot de criptomoedas.')

def get_crypto(update: Update, context: CallbackContext) -> None:
    # Exemplo de consulta à API CoinGecko para pegar dados de Bitcoin
    response = requests.get('https://api.coingecko.com/api/v3/simple/price?ids=bitcoin&vs_currencies=usd')
    data = response.json()
    price = data['bitcoin']['usd']
    update.message.reply_text(f'O preço do Bitcoin é ${price}.')

def main() -> None:
    # Inicializa o bot
    updater = Updater(TOKEN)

    # Obtém o despachante do bot
    dispatcher = updater.dispatcher

    # Adiciona os handlers para os comandos
    dispatcher.add_handler(CommandHandler("start", start))
    dispatcher.add_handler(CommandHandler("crypto", get_crypto))

    # Começa a escutar os comandos
    updater.start_polling()

    # Para o bot quando o processo é interrompido
    updater.idle()

if __name__ == '__main__':
    main()
