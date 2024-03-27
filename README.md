# Fast API pro mobilní aplikace
## Konfigurace projektu
*  Vytvořime si virtualní prostředí: `py -3 -m venv venv`
*   Aktivujeme ho: `venv/Scripts/activate`
*   Budeme potřebovat frameworky FastAPI, Uvicorn a SQLAlchemy.<br>Nainstalujeme si to: `pip install fastapi uvicorn sqlalchemy`.
*   Uděláme soubor `requirements.txt`: `pip freeze > requirements.txt`

Máme mít takovou strukturu projektu:
```.
├── backend
│   ├── venv
│   ├── database.py
│   ├── main.py
│   ├── models.py
│   ├── music_reviews_app.db
│   └── requirements.txt
└── frontend
```

## Připojení ke databázi
* Pro jednoduchost budeme použivat SQLite. V souboru `database.py` naimportujeme metody z baličku SQLAlchemy.
```
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker
from sqlalchemy.ext.declarative import declarative_base
```

* Definujeme adresu databáze<br>
```DATABASE_URL = "sqlite:///./music_reviews_app.db"```

* Vytvaříme instance tříd Engine, Session a Base pro práci s databází
```
engine = create_engine(DATABASE_URL, connect_args={"check_same_thread": False})

SessionLocal = sessionmaker(autocommit=False, autoflush=False, bind=engine)

Base = declarative_base()
```

## Definování modelů
Teď máme založit dvě tabulky (modelů), pro recenze a uživatele.

```
from sqlalchemy import Column, Integer, String
from database import Base
from datetime import datetime

class Review(Base):
    __tablename__ = "reviews"
    id = Column(Integer, primary_key=True, index=True)
    username = Column(String(50))
    song_name = Column(String(50))
    artist_name = Column(String(50))
    review_text = Column(String(1000))
    date_time = Column(String, default=datetime.now())

class User(Base):
    __tablename__ = "users"
    id = Column(Integer, primary_key=True, index=True)
    username = Column(String, unique=True)
    hashed_password = Column(String)
```
