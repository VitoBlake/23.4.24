"# 23.4.24" 
import requests
from bs4 import BeautifulSoup
import os

url = "https://example.com"
save_folder = "images"

response = requests.get(url)
soup = BeautifulSoup(response.text, "html.parser")
img_tags = soup.find_all("img")

if not os.path.exists(save_folder):
    os.makedirs(save_folder)

for img in img_tags:
    img_url = img.attrs.get("src")
    if not img_url:
        continue

    try:
        response = requests.get(img_url)
        img_name = os.path.join(save_folder, os.path.basename(img_url))
        with open(img_name, "wb") as f:
            f.write(response.content)
    except:
        pass
