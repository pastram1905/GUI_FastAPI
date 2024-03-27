# Fast API pro mobilní aplikace
## Konfigurace projektu
*  Vytvořime si virtualní prostředí: `py -3 -m venv venv`
*   Aktivujeme ho: `venv/Scripts/activate`
*   Budeme potřebovat frameworky FastAPI, Uvicorn a SQLAlchemy.<br>Nainstalujeme si to: `pip install fastapi uvicorn sqlalchemy`.
*   Uděláme soubor `requirements.txt`: `pip freeze > requirements.txt`
<br>
Máme mít takovou strukturu projektu:<br>
```
.
├── backend
│   ├── venv
│   ├── database.py
│   ├── main.py
│   ├── models.py
│   ├── music_reviews_app.db
│   └── requirements.txt
└── frontend

## Připojení ke databázi a další nastavení
Pro jednoduchost budeme použivat SQLite. V souboru `database.py` naimportujeme metody z baličku SQLAlchemy.
```
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker
from sqlalchemy.ext.declarative import declarative_base
```
Definujeme adresu databáze<br>
```DATABASE_URL = "sqlite:///./music_reviews_app.db"```
