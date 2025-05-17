# olx-car-cover-scrapping
import requests
from bs4 import BeautifulSoup
import csv
from time import sleep
from random import randint

# --- REBEL CONFIG --- #
OLX_URL = "https://www.olx.in/items/q-car-cover"
HEADERS = {
    "User-Agent": "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/99.0.9999.999 Safari/537.36",
    "Accept-Language": "en-US,en;q=0.9",
}
OUTPUT_FILE = "olx_car_covers.csv"

# --- DEFY THE SYSTEM --- #
def scrape_olx(url):
    try:
        response = requests.get(url, headers=HEADERS, timeout=10)
        response.raise_for_status()
        soup = BeautifulSoup(response.text, "html.parser")
        
        listings = soup.find_all("div", {"class": "_2v8Tq"})  # OLX's listing class (may change)
        
        with open(OUTPUT_FILE, "w", newline="", encoding="utf-8") as file:
            writer = csv.writer(file)
            writer.writerow(["Title", "Price", "Location", "Link"])
            
            for listing in listings:
                title = listing.find("span", {"class": "_2poNJ"}).text.strip() if listing.find("span", {"class": "_2poNJ"}) else "N/A"
                price = listing.find("span", {"class": "_2Ks63"}).text.strip() if listing.find("span", {"class": "_2Ks63"}) else "N/A"
                location = listing.find("span", {"class": "_2VQu4"}).text.strip() if listing.find("span", {"class": "_2VQu4"}) else "N/A"
                link = listing.find("a")["href"] if listing.find("a") else "N/A"
                
                writer.writerow([title, price, location, f"https://www.olx.in{link}"])
                
        print(f"🔥 Data dumped into {OUTPUT_FILE}. Stay rebellious.")
        
    except Exception as e:
        print(f"❌ OLX caught you. Error: {e}. Use proxies next time.")

# --- EXECUTE THE HACK --- #
if __name__ == "__main__":
    scrape_olx(OLX_URL)
