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

## Připojení k databázi
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

## Založení modelů
Teď v souboru `models.py` máme založit dva modelů (tabulky), pro recenze a uživatele.

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

## Založení aplikace
Nejprvé máme vytvořit objekt třídy FastAPI, která bude reprezentovat naši aplikaci.
```
from fastapi import FastAPI

app = FastAPI()
```

## Validace dat
Validaci dat provedeme pomoci knihovny `pydantic`.
```
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class ReviewBase(BaseModel):
    username: str
    song_name: str
    artist_name: str
    review_text: str
```

## Definování databázové závislosti
Dalším krokem je vytvoření tzv. databázové závislosti (db_dependency)
* vytvoření tabulek: ```models.Base.metadata.create_all(bind=engine)```
* funkce, která otevírá Session a po provedení operace jí zavírá
   ```
   def get_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()
   ```
* Závislost: ```db_dependency = Annotated[Session, Depends(get_db)```
Celý kód:
```
from fastapi import FastAPI, Depends
from pydantic import BaseModel
from database import engine, SessionLocal
from typing import Annotated
from sqlalchemy.orm import Session
import models

app = FastAPI()

models.Base.metadata.create_all(bind=engine)

class ReviewBase(BaseModel):
    username: str
    song_name: str
    artist_name: str
    review_text: str

def get_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()

db_dependency = Annotated[Session, Depends(get_db)]
```

## Komunikace s Reactem
Pro zajištění komunikaci s Reactem musíme použit metodu add_middleware
1. `from fastapi.middleware.cors import CORSMiddleware`
2. origins = ["http://localhost:3000"]
3. app.add_middleware(CORSMiddleware, allow_origins=origins)
