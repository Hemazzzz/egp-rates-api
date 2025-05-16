from fastapi import FastAPI
from fastapi.responses import JSONResponse
import requests
from bs4 import BeautifulSoup

app = FastAPI()

currencies = {
    "USD": "الدولار الأمريكي",
    "EUR": "اليورو",
    "GBP": "الجنيه الاسترليني",
    "SAR": "الريال السعودي",
    "AED": "الدرهم الإماراتي",
    "KWD": "الدينار الكويتي",
    "QAR": "الريال القطري",
    "BHD": "الدينار البحريني",
    "OMR": "الريال العماني",
    "JOD": "الدينار الأردني",
    "SDG": "الجنيه السوداني",
    "DZD": "الدينار الجزائري",
    "MAD": "الدرهم المغربي",
    "TND": "الدينار التونسي",
    "LYD": "الدينار الليبي",
    "LBP": "الليرة اللبنانية",
    "SYP": "الليرة السورية",
    "YER": "الريال اليمني",
    "IQD": "الدينار العراقي",
    "MRU": "الأوقية الموريتانية",
    "SOS": "الشلن الصومالي"
}

@app.get("/exchange-rates")
def get_rates():
    headers = {"User-Agent": "Mozilla/5.0"}
    result = {}
    for code in currencies:
        try:
            query = f"1 EGP to {code}"
            url = f"https://www.google.com/search?q={query}"
            res = requests.get(url, headers=headers)
            soup = BeautifulSoup(res.text, "html.parser")
            rate = soup.find("span", class_="DFlfde").text
            result[code] = float(rate.replace(",", ""))
        except:
            result[code] = None
    return JSONResponse(content=result)
