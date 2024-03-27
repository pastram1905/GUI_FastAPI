# Fast API pro mobilní aplikace
## Konfigurace projektu
*  Vytvořime si virtualní prostředí: `py -3 -m venv venv`
*   Aktivujeme ho: `venv/Scripts/activate`
*   Budeme potřebovat frameworky FastAPI, Uvicorn a SQLAlchemy. Nainstalujeme si to: `pip install fastapi uvicorn sqlalchemy`.
*   Uděláme soubor `requirements.txt`: `pip freeze > requirements.txt`
<br>
Máme mít takovou strukturu projektu:

. <br>
├── backend <br>
        ├── __pycache__ <br>
        ├── venv <br>
        ├── database.py <br>
        ├── main.py <br>
        ├── models.py <br>
        ├── music_reviews_app.db <br>
└── frontend <br>
