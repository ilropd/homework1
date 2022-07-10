# homework1lite
Extra options for original code for currency info


import requests
import json
from flask import Flask
import datetime # new library


def get_valutes_list():
    url = 'https://www.cbr-xml-daily.ru/daily_json.js'
    response = requests.get(url)
    data = json.loads(response.text)
    valutes = list(data['Valute'].values())
    return valutes


app = Flask(__name__)


def create_html(valutes):
    actualdate = datetime.date.today().strftime("%d.%m.%Y") # new variable with current date in format 00.00.0000
    text = '<h1>Курс валют</h1>'
    text += f'<h3>Данные актульны на {actualdate}</h3>' # text with information about the relevance of data for the current date
    text += '<table>'
    text += '<tr>'
    for _ in valutes[0]:
        text += f'<th><th>'
    text += '</tr>'
    for valute in valutes:
        text += '<tr>'
        for v in valute.values():
            text += f'<td>{v}</td>'
        text += '</tr>'

    text += '</table>'
    return text


@app.route("/")
def index():
    valutes = get_valutes_list()
    html = create_html(valutes)
    return html


if __name__ == "__main__":
    app.run()
