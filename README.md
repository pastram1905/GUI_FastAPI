# Fast API pro mobilní aplikace
## Konfigurace projektu
*  Vytvořime si virtualní prostředí: `py -3 -m venv venv`
*   Aktivujeme ho: `venv/Scripts/activate`
*   Budeme potřebovat frameworky FastAPI, Uvicorn a SQLAlchemy.<br>Nainstalujeme si to: `pip install fastapi uvicorn sqlalchemy`.
*   Uděláme soubor `requirements.txt`: `pip freeze > requirements.txt`
<br>
Máme mít takovou strukturu projektu:
```
. <br>
├── backend <br>
│   ├── venv <br>
│   ├── database.py <br>
│   ├── main.py <br>
│   ├── models.py <br>
│   ├── music_reviews_app.db <br>
│   └── requirements.txt <br>
└── frontend <br>
```

## Připojení ke databázi a další nastavení
Pro jednoduchost budeme použivat SQLite. V souboru `database.py` naimportujeme metody z baličku SQLAlchemy.

```
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker
from sqlalchemy.ext.declarative import declarative_base
```
