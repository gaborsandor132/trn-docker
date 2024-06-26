# Kód 3 - Webalkalmazás adatbázissal

## Projekt leírása

Adatbázis szerver és webalkalmazás konténerek futtatása Docker környezetben.

## Kiegészítő alkalmazások

DBEaver adatbázis kliens: https://dbeaver.com/download/lite/

## Projekt struktúra

```bash
.
├── backend
│   └── Dockerfile
│
├── frontend
│   ├── Dockerfile
│   ├── app.py
├── └── requirements.txt
└── docker-compose.yaml

```

## Hálózat

```bash
docker network create mt-network
```

## Adatbázis

- Építés

```bash
docker build --tag mt-db:latest .
```

### Adattárolás a konténeren belül (nem elérhető a hoston)

```bash
docker run --name adatbazis --network mt-network -d mt-db:latest
```

### Adattárolás a konténeren belül (elérhető a hoston)

```bash
docker run --name adatbazis -p 3306:3306 --network mt-network -d mt-db:latest
```

### Adattárolás a konténeren kívül (persistent volume)

## Perzisztens lemez

```bash
docker volume create mariadb_data
```

```bash
docker run --name adatbazis -p 3306:3306 --network mt-network -d -v mariadb_data:/var/lib/mysql mt-db:latest
```

## Webalkalmazás

- Építés

````bash
docker build --tag mt-web:latest .

```bash
docker run --name web --network mt-network -p 8000:5000 -d -e DB_HOST='adatbazis' -e DB_USER='root' -e DB_PASS='2NUW-a5QdH-8fAXy' -e DB_NAME='adatbazis' mt-web:latest
````

- Használat:

  Böngészőben: http://localhost:8000

## Docker Compose

### Indítás építéssel

```bash
docker-compose up --build
```

### Indítás

```bash
docker-compose up -d
```

_Megjegyzés: `-d` kapcsolóval a konténerek a háttérben futnak._

### Leállítás

```bash
docker-compose down
```