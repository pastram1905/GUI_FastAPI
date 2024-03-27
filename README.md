# Fast API pro mobilní aplikace
## Konfigurace projektu
*  Vytvořime si virtualní prostředí: `py -3 -m venv venv`
*   Aktivujeme ho: `venv/Scripts/activate`
*   Budeme potřebovat frameworky FastAPI, Uvicorn a SQLAlchemy. Nainstalujeme si to: `pip install fastapi uvicorn sqlalchemy`.
*   Uděláme soubor `requirements.txt`: `pip freeze > requirements.txt`
<br>
Máme mít takovou strukturu projektu:

-.
        -backend
                -_pycache_
                -venv
                -database.py
                -main.py
                -models.py
                -music_reviews_app.db
        -frontend
