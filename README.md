import requests
import time
from flask import Flask, request

app = Flask(__name__)

TOKEN = 'YOUR_BOT_TOKEN'
URL = f'https://api.telegram.org/bot{TOKEN}/sendMessage'

@app.route('/', methods=['POST'])
def webhook():
    data = request.get_json()
    if "message" in data:
        chat_id = data["message"]["chat"]["id"]
        text = data["message"].get("text", "")

        if text == '/start':
            message = "Добро пожаловать! Товары:\n1. Футболка\n2. Кепка"
        elif text == "1":
            message = "Вы выбрали Футболку. Введите адрес."
        elif text == "2":
            message = "Вы выбрали Кепку. Введите адрес."
        else:
            message = "Спасибо! Мы с вами свяжемся."

        requests.post(URL, json={"chat_id": chat_id, "text": message})

    return {"ok": True}
