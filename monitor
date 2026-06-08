import requests
from bs4 import BeautifulSoup
import json
from pathlib import Path

URL = "https://prospera.se/rankings"
STATE_FILE = "reports.json"

html = requests.get(URL).text

soup = BeautifulSoup(html, "html.parser")
text = soup.get_text("\n")

lines = [x.strip() for x in text.splitlines() if x.strip()]

reports = []

for i, line in enumerate(lines):
    if line == "Recent Reports":
        section = lines[i+1:i+50]

        for j in range(len(section)-1):
            if section[j].startswith("20"):
                reports.append({
                    "date": section[j],
                    "title": section[j+1]
                })
        break

path = Path(STATE_FILE)

old = []

if path.exists():
    old = json.loads(path.read_text())

new_reports = [
    r for r in reports
    if r not in old
]

if new_reports:
    print("NEW REPORTS FOUND:")
    print(new_reports)

path.write_text(json.dumps(reports, indent=2))
