# 🚀 Szkolenie API dla Początkujących - Przewodnik z Ćwiczeniami

## 📋 Spis Treści
1. [Informacje o szkoleniu](#informacje-o-szkoleniu)
2. [Dzień 1: Fundamenty API i HTTP](#dzień-1-fundamenty-api-i-http)
3. [Dzień 2: Praktyka z REST API](#dzień-2-praktyka-z-rest-api)
4. [Dzień 3: Dobre praktyki i projekt](#dzień-3-dobre-praktyki-i-projekt)

---

## Informacje o szkoleniu

### 🎯 Cele szkolenia
Po ukończeniu szkolenia uczestnik będzie potrafił:
- Wyjaśnić, czym jest API i jak działa komunikacja HTTP
- Samodzielnie tworzyć proste REST API w Pythonie
- Testować API przy użyciu Postmana
- Dokumentować API zgodnie z najlepszymi praktykami
- Stosować bezpieczne metody autoryzacji
- Debugować i rozwiązywać problemy z API

### 📚 Metodologia
- **70% praktyki, 30% teorii** - nauka przez działanie
- **Interaktywne ćwiczenia** - każdy koncept wzmocniony praktyką
- **Praca w parach** - peer programming i code review
- **Projekt końcowy** - stworzenie własnego API od podstaw

### 🛠️ Wymagania techniczne
- Python 3.8+ zainstalowany
- Postman Desktop Agent
- Edytor kodu (VS Code, PyCharm)
- Dostęp do internetu

---

## Dzień 1: Fundamenty API i HTTP

### 🎓 Moduł 1.1: Czym jest API? (45 min)

#### Teoria z przykładami (15 min)

**API (Application Programming Interface)** to interfejs, który pozwala różnym aplikacjom komunikować się ze sobą.

**💡 Analogia do zrozumienia:**
Wyobraź sobie API jak **menu w restauracji**:
- **Menu (API)** pokazuje, co możesz zamówić (dostępne funkcje)
- **Kelner (HTTP)** przynosi Twoje zamówienie do kuchni (serwera)
- **Kuchnia (serwer)** przygotowuje danie (przetwarza dane)
- **Kelner** przynosi gotowe danie (odpowiedź API)

**NIE musisz wiedzieć**, jak dokładnie kuchnia przygotowuje danie (implementacja wewnętrzna) - wystarczy, że wiesz, co zamówić!

#### 🎯 Ćwiczenie 1.1: "Rozpoznaj API w codziennym życiu" (15 min)

**Cel:** Zrozumienie wszechobecności API w życiu codziennym

**Instrukcje:**
1. Podzielcie się na grupy 3-4 osobowe
2. Każda grupa ma 5 minut na wymyślenie **5 sytuacji z codziennego życia**, gdzie używamy API (nawet nie wiedząc o tym!)
3. Dla każdej sytuacji określcie:
   - Co robi użytkownik?
   - Jakie API jest prawdopodobnie używane?
   - Jakie dane są wymieniane?

**Przykład:**
- **Sytuacja:** Sprawdzanie pogody w smartfonie
- **API:** OpenWeatherMap API lub Weather.com API
- **Dane:** Lokalizacja → prognoza pogody, temperatura, opady

**Prezentacja:** Każda grupa prezentuje swoje 2 najciekawsze przykłady (5 min)

**Omówienie:** Prowadzący podsumowuje i uzupełnia o przykłady: płatności online, logowanie przez Google/Facebook, mapy w Uber, integracje w aplikacjach (10 min)

---

#### 🎯 Ćwiczenie 1.2: "REST vs SOAP vs GraphQL - Gra decyzyjna" (15 min)

**Cel:** Zrozumienie różnic między stylami API

**Materiały:** Karty scenariuszy (przygotowane przez prowadzącego)

**Instrukcje:**
Dla każdego scenariusza zdecyduj, który typ API jest najbardziej odpowiedni i **dlaczego**:

**Scenariusz A:**
Tworzysz prostą aplikację mobilną do zarządzania listą zakupów. Potrzebujesz szybko pobrać i zapisać dane. Aplikacja będzie używana przez tysiące użytkowników.
- [ ] REST
- [ ] SOAP
- [ ] GraphQL

**Scenariusz B:**
Pracujesz dla banku, który wymaga najwyższych standardów bezpieczeństwa i potrzebuje ACID transactions dla transferów pieniężnych między systemami legacy.
- [ ] REST
- [ ] SOAP
- [ ] GraphQL

**Scenariusz C:**
Tworzysz frontend dla aplikacji e-commerce, gdzie na jednym ekranie potrzebujesz: danych produktu, recenzji, powiązanych produktów, statusu dostępności i 3 zdjęć. Chcesz uniknąć 5 osobnych zapytań.
- [ ] REST
- [ ] SOAP
- [ ] GraphQL

**Odpowiedzi i wyjaśnienie:**
- **Scenariusz A: REST** - prosty, szybki, skalowalny, idealny dla mobilnych aplikacji
- **Scenariusz B: SOAP** - wysoka bezpieczeństwo (WS-Security), transakcyjność, standard w systemach finansowych
- **Scenariusz C: GraphQL** - jedno zapytanie, brak over-fetching, klient kontroluje strukturę danych

---

### 🎓 Moduł 1.2: Podstawy HTTP (90 min)

#### Teoria interaktywna (20 min)

**Metody HTTP = Czasowniki akcji**

| Metoda | Akcja CRUD | Przykład | Idempotentna? |
|--------|------------|----------|---------------|
| GET | Read (Czytaj) | Pobierz listę użytkowników | ✅ Tak |
| POST | Create (Utwórz) | Dodaj nowego użytkownika | ❌ Nie |
| PUT | Update (Aktualizuj) | Zaktualizuj cały profil użytkownika | ✅ Tak |
| PATCH | Update (Aktualizuj częściowo) | Zmień tylko email użytkownika | ⚠️ Może być |
| DELETE | Delete (Usuń) | Usuń użytkownika | ✅ Tak |

**💡 Kluczowe pojęcie: Idempotencja**
Operacja idempotentna = wywołanie jej wiele razy ma taki sam efekt jak wywołanie raz.
- Usunięcie tego samego użytkownika 5 razy = użytkownik i tak już nie istnieje po pierwszym razie
- Dodanie użytkownika 5 razy (POST) = 5 nowych użytkowników!

#### 🎯 Ćwiczenie 1.3: "HTTP Methods Matching Game" (20 min)

**Cel:** Utrwalenie wyboru odpowiedniej metody HTTP

**Instrukcje:** Dopasuj akcję do odpowiedniej metody HTTP:

1. Chcę zobaczyć profil użytkownika o ID 42
   - **Odpowiedź:** `GET /users/42`

2. Chcę utworzyć nowe konto w systemie
   - **Odpowiedź:** `POST /users`

3. Chcę zmienić hasło użytkownika
   - **Odpowiedź:** `PATCH /users/42/password` lub `PUT /users/42` (cały zasób)

4. Chcę usunąć swoje konto
   - **Odpowiedź:** `DELETE /users/42`

5. Chcę zobaczyć wszystkie posty na blogu
   - **Odpowiedź:** `GET /posts`

6. Chcę zastąpić całkowicie dane adresowe użytkownika
   - **Odpowiedź:** `PUT /users/42/address`

**Zadanie dodatkowe:** 
Wymyśl 3 własne scenariusze i określ odpowiednią metodę HTTP oraz endpoint!

---

#### 🎯 Ćwiczenie 1.4: "Kody statusu HTTP - Detektyw API" (25 min)

**Cel:** Nauczenie się interpretacji kodów statusu HTTP

**Scenariusz:** Jesteś detektywem API i musisz rozwikłać, co się stało na podstawie kodu statusu!

**Instrukcje:** Dla każdego kodu statusu:
1. Określ, czy to sukces czy błąd
2. Po czyjej stronie jest problem (klient/serwer/brak problemu)
3. Co prawdopodobnie się stało?
4. Co powinien zrobić klient?

**Przypadek 1: Status 200**
- Sukces czy błąd? **SUKCES ✅**
- Czyja strona? **Brak problemu**
- Co się stało? **Zapytanie GET wykonane poprawnie, dane zwrócone**
- Co dalej? **Przetwórz otrzymane dane**

**Przypadek 2: Status 404**
- Sukces czy błąd? **BŁĄD ❌**
- Czyja strona? **Klient (4xx)**
- Co się stało? **Zasób nie został znaleziony - zły URL lub zasób nie istnieje**
- Co dalej? **Sprawdź URL, sprawdź czy zasób istnieje**

**Przypadek 3: Status 500**
- Sukces czy błąd? **BŁĄD ❌**
- Czyja strona? **Serwer (5xx)**
- Co się stało? **Wewnętrzny błąd serwera - coś poszło nie tak w kodzie**
- Co dalej? **Spróbuj ponownie później, zgłoś problem administratorowi**

**Przypadek 4: Status 201**
- Sukces czy błąd? **SUKCES ✅**
- Czyja strona? **Brak problemu**
- Co się stało? **Nowy zasób został utworzony (POST)**
- Co dalej? **Sprawdź header `Location` - tam jest URL nowego zasobu**

**Zadanie praktyczne:**
Stwórz "cheat sheet" (ściągawkę) z 10 najważniejszymi kodami statusu i przykładami sytuacji. Możecie użyć rysunków, emotikon, kolorów!

**Przykład ściągawki:**
```
😊 2xx - SUKCES
  200 OK - Wszystko działa
  201 Created - Stworzyłeś coś nowego!
  204 No Content - Sukces, ale brak danych do zwrócenia

😕 4xx - TO TWÓJ BŁĄD (klient)
  400 Bad Request - Coś źle wysłałeś
  401 Unauthorized - Zapomniałeś się zalogować
  403 Forbidden - Jesteś zalogowany, ale nie masz uprawnień
  404 Not Found - Tego nie ma!

😱 5xx - TO BŁĄD SERWERA
  500 Internal Server Error - Serwer się zepsuł
  503 Service Unavailable - Serwer na przerwie
```

---

#### 🎯 Ćwiczenie 1.5: "JSON - Build Your Own API Response" (25 min)

**Cel:** Praktyczne tworzenie i czytanie formatu JSON

**Część A: Czytanie JSON (10 min)**

Przeanalizuj poniższy JSON i odpowiedz na pytania:

```json
{
  "user": {
    "id": 42,
    "username": "jankowalski",
    "email": "jan@example.com",
    "is_active": true,
    "roles": ["user", "moderator"],
    "profile": {
      "first_name": "Jan",
      "last_name": "Kowalski",
      "age": 28
    },
    "posts_count": 156
  }
}
```

**Pytania:**
1. Jaki typ danych ma pole `is_active`? **boolean**
2. Ile ról ma użytkownik i jakie? **2 role: user i moderator**
3. Jak się dostać do wieku użytkownika? **user.profile.age**
4. Czy `posts_count` to liczba czy string? **liczba**
5. Co będzie jeśli spróbujesz dodać komentarz `// to jest komentarz` w JSON? **BŁĄD! JSON nie obsługuje komentarzy**

**Część B: Tworzenie JSON (15 min)**

**Zadanie:** Zaprojektuj JSON reprezentujący:

**Scenariusz 1:** Produkt w sklepie internetowym
Powinien zawierać: nazwę, cenę, walutę, czy jest dostępny, kategorie (lista), ocenę (1-5), opis, rozmiary dostępne (lista)

**Twoje rozwiązanie:**
```json
{
  "product": {
    "id": 101,
    "name": "Smartfon XYZ",
    "price": 2499.99,
    "currency": "PLN",
    "in_stock": true,
    "categories": ["Elektronika", "Telefony", "Smartfony"],
    "rating": 4.5,
    "description": "Nowoczesny smartfon z aparatem 108MP",
    "available_colors": ["czarny", "biały", "niebieski"]
  }
}
```

**Scenariusz 2:** Odpowiedź API z listą zadań (TODO list)
Każde zadanie ma: ID, tytuł, opis, status (ukończone/nieukończone), priorytet, datę utworzenia

**Wspólne omówienie:** Prowadzący omawia różne podejścia uczestników i wskazuje najlepsze praktyki (np. konsystentne nazewnictwo, camelCase vs snake_case)

---

### 🎓 Moduł 1.3: Praktyka z Postmanem (90 min)

#### 🎯 Ćwiczenie 1.6: "Pierwsze kroki z Postmanem" (30 min)

**Cel:** Zapoznanie z interfejsem Postmana i wykonanie pierwszych zapytań

**Krok 1: Instalacja i setup (10 min)**
1. Pobierz i zainstaluj Postman Desktop
2. Utwórz darmowe konto (opcjonalne, ale zalecane - pozwala synchronizować kolekcje)
3. Utwórz nowy Workspace: "Szkolenie API"

**Krok 2: Pierwsze zapytanie GET (10 min)**

Użyjemy publicznego API do testowania: **JSONPlaceholder** (https://jsonplaceholder.typicode.com/)

**Zadanie:**
1. Utwórz nowe zapytanie
2. Wybierz metodę: `GET`
3. Wpisz URL: `https://jsonplaceholder.typicode.com/users`
4. Kliknij **Send**

**Obserwacja:**
- Jaki status otrzymałeś? *(Oczekiwana odpowiedź: 200 OK)*
- Ile użytkowników jest w odpowiedzi? *(10 użytkowników)*
- Przejdź do zakładki **Headers** w odpowiedzi - znajdź `Content-Type`. Jaka jest wartość? *(application/json)*

**Krok 3: Parametry zapytania (Query Parameters) (10 min)**

**Zadanie:**
1. Nowe zapytanie GET: `https://jsonplaceholder.typicode.com/posts`
2. W sekcji **Params** dodaj:
   - Key: `userId`, Value: `1`
3. Wyślij zapytanie

**Pytania:**
- Ile postów otrzymałeś? *(10 postów użytkownika o ID 1)*
- Jaki jest pełny URL po dodaniu parametru? *(https://jsonplaceholder.typicode.com/posts?userId=1)*
- Dodaj kolejny parametr: `_limit=5` - co się stało? *(Otrzymasz tylko 5 pierwszych postów)*

---

#### 🎯 Ćwiczenie 1.7: "Postman - Wszystkie metody HTTP" (40 min)

**Cel:** Praktyczne użycie wszystkich głównych metod HTTP

Będziemy używać API: **https://jsonplaceholder.typicode.com/**

**Zadanie 1: GET - Pobierz konkretny post (5 min)**
- URL: `https://jsonplaceholder.typicode.com/posts/1`
- Metoda: `GET`
- **Pytanie:** Jaki jest tytuł posta?

**Zadanie 2: POST - Utwórz nowy post (10 min)**
1. URL: `https://jsonplaceholder.typicode.com/posts`
2. Metoda: `POST`
3. W zakładce **Headers** dodaj:
   - Key: `Content-Type`, Value: `application/json`
4. W zakładce **Body** wybierz **raw** i **JSON**, następnie wpisz:
```json
{
  "title": "Moje pierwsze API",
  "body": "Uczę się używać Postmana!",
  "userId": 1
}
```
5. Kliknij **Send**

**Pytania:**
- Jaki status otrzymałeś? *(201 Created)*
- Jakie ID dostał nowy post? *(Zazwyczaj 101)*

**Zadanie 3: PUT - Zaktualizuj cały zasób (10 min)**
1. URL: `https://jsonplaceholder.typicode.com/posts/1`
2. Metoda: `PUT`
3. Body (raw JSON):
```json
{
  "id": 1,
  "title": "Zaktualizowany tytuł",
  "body": "Całkowicie nowa treść posta",
  "userId": 1
}
```
4. Send

**Obserwacja:** Porównaj odpowiedź - wszystkie pola zostały zastąpione

**Zadanie 4: PATCH - Zaktualizuj częściowo (10 min)**
1. URL: `https://jsonplaceholder.typicode.com/posts/1`
2. Metoda: `PATCH`
3. Body (raw JSON):
```json
{
  "title": "Tylko tytuł został zmieniony"
}
```
4. Send

**Obserwacja:** Tylko pole `title` się zmieniło, reszta bez zmian!

**Zadanie 5: DELETE - Usuń zasób (5 min)**
1. URL: `https://jsonplaceholder.typicode.com/posts/1`
2. Metoda: `DELETE`
3. Send

**Pytanie:** Jaki status otrzymałeś? *(200 OK lub 204 No Content)*

---

#### 🎯 Ćwiczenie 1.8: "Organizacja w Postmanie - Kolekcje" (20 min)

**Cel:** Nauczenie się organizować zapytania w kolekcje

**Zadanie:**
Stwórz kolekcję "JSONPlaceholder API" i uporządkuj wszystkie poprzednie zapytania w foldery:

```
📁 JSONPlaceholder API
  📂 Users
    ↳ GET All Users
    ↳ GET User by ID
  📂 Posts
    ↳ GET All Posts
    ↳ GET Posts by User
    ↳ GET Single Post
    ↳ POST Create Post
    ↳ PUT Update Post
    ↳ PATCH Update Post Partially
    ↳ DELETE Post
```

**Instrukcje:**
1. Kliknij **New Collection**
2. Nazwij: "JSONPlaceholder API"
3. Kliknij prawym przyciskiem na kolekcję → **Add Folder** → "Users"
4. Przeciągnij odpowiednie zapytania do folderów
5. Zmień nazwy zapytań, aby były opisowe

**Best Practices:**
- ✅ Używaj jasnych, opisowych nazw
- ✅ Grupuj zapytania logicznie
- ✅ Dodawaj opisy do zapytań (pole Description)
- ✅ Używaj zmiennych środowiskowych (zobaczymy w następnych ćwiczeniach)

---

## Dzień 2: Praktyka z REST API

### 🎓 Moduł 2.1: Budowanie własnego API (120 min)

#### 🎯 Ćwiczenie 2.1: "Flask API od podstaw" (60 min)

**Cel:** Stworzenie prostego działającego API w Pythonie

**Przygotowanie środowiska (10 min):**

```bash
# Utwórz folder projektu
mkdir moje-api
cd moje-api

# Utwórz wirtualne środowisko
python -m venv venv

# Aktywuj środowisko
# Windows:
venv\Scripts\activate
# Mac/Linux:
source venv/bin/activate

# Zainstaluj Flask
pip install flask
```

**Krok 1: Hello World API (15 min)**

Utwórz plik `app.py`:

```python
from flask import Flask, jsonify

app = Flask(__name__)

@app.route('/')
def home():
    return jsonify({"message": "Witaj w moim pierwszym API!"})

@app.route('/hello/<name>')
def hello(name):
    return jsonify({"message": f"Cześć, {name}!"})

if __name__ == '__main__':
    app.run(debug=True, port=5000)
```

**Uruchom:**
```bash
python app.py
```

**Testuj w Postmanie:**
- `GET http://localhost:5000/`
- `GET http://localhost:5000/hello/Jan`

**Pytania do uczestników:**
- Co robi dekorator `@app.route()`?
- Dlaczego używamy `jsonify()`?
- Co oznacza `<name>` w ścieżce?

**Krok 2: CRUD API dla zadań (TODO) (35 min)**

Rozbuduj `app.py`:

```python
from flask import Flask, jsonify, request

app = Flask(__name__)

# "Baza danych" w pamięci
tasks = [
    {"id": 1, "title": "Nauczyć się API", "done": False},
    {"id": 2, "title": "Stworzyć projekt", "done": False}
]

# GET - Pobierz wszystkie zadania
@app.route('/api/tasks', methods=['GET'])
def get_tasks():
    return jsonify({"tasks": tasks})

# GET - Pobierz jedno zadanie
@app.route('/api/tasks/<int:task_id>', methods=['GET'])
def get_task(task_id):
    task = next((t for t in tasks if t['id'] == task_id), None)
    if task is None:
        return jsonify({"error": "Task not found"}), 404
    return jsonify({"task": task})

# POST - Utwórz nowe zadanie
@app.route('/api/tasks', methods=['POST'])
def create_task():
    if not request.json or 'title' not in request.json:
        return jsonify({"error": "Title is required"}), 400
    
    new_task = {
        "id": tasks[-1]['id'] + 1 if tasks else 1,
        "title": request.json['title'],
        "done": False
    }
    tasks.append(new_task)
    return jsonify({"task": new_task}), 201

# PUT - Zaktualizuj zadanie
@app.route('/api/tasks/<int:task_id>', methods=['PUT'])
def update_task(task_id):
    task = next((t for t in tasks if t['id'] == task_id), None)
    if task is None:
        return jsonify({"error": "Task not found"}), 404
    
    task['title'] = request.json.get('title', task['title'])
    task['done'] = request.json.get('done', task['done'])
    return jsonify({"task": task})

# DELETE - Usuń zadanie
@app.route('/api/tasks/<int:task_id>', methods=['DELETE'])
def delete_task(task_id):
    global tasks
    tasks = [t for t in tasks if t['id'] != task_id]
    return jsonify({"message": "Task deleted"}), 200

if __name__ == '__main__':
    app.run(debug=True, port=5000)
```

**Testowanie (użyj Postmana):**

✅ **Test 1:** GET wszystkich zadań
- `GET http://localhost:5000/api/tasks`

✅ **Test 2:** GET jednego zadania
- `GET http://localhost:5000/api/tasks/1`

✅ **Test 3:** POST nowe zadanie
- `POST http://localhost:5000/api/tasks`
- Body (JSON):
```json
{
  "title": "Nauczyć się testować API"
}
```

✅ **Test 4:** PUT aktualizacja
- `PUT http://localhost:5000/api/tasks/1`
- Body (JSON):
```json
{
  "title": "Nauczyć się API - ZROBIONE!",
  "done": true
}
```

✅ **Test 5:** DELETE zadanie
- `DELETE http://localhost:5000/api/tasks/2`

**Zadanie dla uczestników:**
Dodaj endpoint `PATCH /api/tasks/<id>/toggle`, który przełącza status `done` zadania (z false na true i odwrotnie)

---

#### 🎯 Ćwiczenie 2.2: "Obsługa błędów jak profesjonalista" (30 min)

**Cel:** Nauczenie się prawidłowego raportowania błędów w API

**Część A: Analiza złych praktyk (10 min)**

**Zła praktyka #1:**
```python
@app.route('/api/users/<int:user_id>')
def get_user(user_id):
    user = db.get_user(user_id)
    if not user:
        return "Not found"  # ❌ Zwraca HTML zamiast JSON, brak kodu statusu
    return jsonify(user)
```

**Dobra praktyka:**
```python
@app.route('/api/users/<int:user_id>')
def get_user(user_id):
    user = db.get_user(user_id)
    if not user:
        return jsonify({
            "error": "User not found",
            "status": 404
        }), 404  # ✅ JSON + odpowiedni kod statusu
    return jsonify(user), 200
```

**Część B: Implementacja (20 min)**

**Zadanie:** Dodaj do swojego API z zadaniami profesjonalną obsługę błędów

```python
from flask import Flask, jsonify, request

# Globalna obsługa błędów
@app.errorhandler(404)
def not_found(error):
    return jsonify({
        "error": "Resource not found",
        "status": 404,
        "message": "The requested URL was not found on the server."
    }), 404

@app.errorhandler(400)
def bad_request(error):
    return jsonify({
        "error": "Bad request",
        "status": 400,
        "message": str(error)
    }), 400

@app.errorhandler(500)
def internal_error(error):
    return jsonify({
        "error": "Internal server error",
        "status": 500,
        "message": "Something went wrong on our end."
    }), 500

# Funkcja pomocnicza do walidacji
def validate_task_data(data):
    """Zwraca (is_valid, error_message)"""
    if not data:
        return False, "No data provided"
    if 'title' not in data:
        return False, "Title is required"
    if not isinstance(data['title'], str):
        return False, "Title must be a string"
    if len(data['title']) < 3:
        return False, "Title must be at least 3 characters long"
    return True, None

# Użycie w endpoincie
@app.route('/api/tasks', methods=['POST'])
def create_task():
    data = request.get_json()
    
    is_valid, error_msg = validate_task_data(data)
    if not is_valid:
        return jsonify({
            "error": "Validation failed",
            "message": error_msg
        }), 400
    
    new_task = {
        "id": tasks[-1]['id'] + 1 if tasks else 1,
        "title": data['title'],
        "done": False
    }
    tasks.append(new_task)
    return jsonify({"task": new_task}), 201
```

**Testuj różne błędy:**
1. Wyślij POST bez body → powinno zwrócić 400 z opisem błędu
2. Wyślij POST bez pola "title" → 400 + komunikat
3. Wyślij POST z tytułem "ab" (za krótki) → 400 + komunikat
4. Wyślij GET na nieistniejący endpoint → 404
5. Wyślij GET na nieistniejące zadanie → 404

---

#### 🎯 Ćwiczenie 2.3: "Dokumentacja API - OpenAPI/Swagger" (30 min)

**Cel:** Nauka tworzenia dokumentacji API

**Krok 1: Instalacja narzędzi (5 min)**

```bash
pip install flask-swagger-ui
```

**Krok 2: Stworzenie specyfikacji OpenAPI (15 min)**

Utwórz plik `swagger.yaml`:

```yaml
openapi: 3.0.0
info:
  title: Tasks API
  description: Proste API do zarządzania zadaniami
  version: 1.0.0
servers:
  - url: http://localhost:5000
    description: Development server

paths:
  /api/tasks:
    get:
      summary: Pobierz wszystkie zadania
      description: Zwraca listę wszystkich zadań
      responses:
        '200':
          description: Lista zadań
          content:
            application/json:
              schema:
                type: object
                properties:
                  tasks:
                    type: array
                    items:
                      $ref: '#/components/schemas/Task'
    
    post:
      summary: Utwórz nowe zadanie
      description: Dodaje nowe zadanie do listy
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              required:
                - title
              properties:
                title:
                  type: string
                  minLength: 3
                  example: "Nauczyć się API"
      responses:
        '201':
          description: Zadanie utworzone
          content:
            application/json:
              schema:
                type: object
                properties:
                  task:
                    $ref: '#/components/schemas/Task'
        '400':
          description: Błędne dane wejściowe
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'

  /api/tasks/{taskId}:
    get:
      summary: Pobierz zadanie po ID
      parameters:
        - name: taskId
          in: path
          required: true
          schema:
            type: integer
          description: ID zadania
      responses:
        '200':
          description: Szczegóły zadania
        '404':
          description: Zadanie nie znalezione

components:
  schemas:
    Task:
      type: object
      properties:
        id:
          type: integer
          example: 1
        title:
          type: string
          example: "Nauczyć się API"
        done:
          type: boolean
          example: false
    
    Error:
      type: object
      properties:
        error:
          type: string
        message:
          type: string
        status:
          type: integer
```

**Krok 3: Integracja ze Swagger UI (10 min)**

Dodaj do `app.py`:

```python
from flask_swagger_ui import get_swaggerui_blueprint

# Konfiguracja Swagger UI
SWAGGER_URL = '/api/docs'
API_URL = '/static/swagger.yaml'

swaggerui_blueprint = get_swaggerui_blueprint(
    SWAGGER_URL,
    API_URL,
    config={'app_name': "Tasks API"}
)

app.register_blueprint(swaggerui_blueprint, url_prefix=SWAGGER_URL)

# Serwuj plik swagger.yaml
@app.route('/static/swagger.yaml')
def swagger_spec():
    with open('swagger.yaml', 'r') as f:
        return f.read(), 200, {'Content-Type': 'text/yaml'}
```

**Testuj:** Otwórz w przeglądarce `http://localhost:5000/api/docs`

---

### 🎓 Moduł 2.2: Autoryzacja i bezpieczeństwo (90 min)

#### 🎯 Ćwiczenie 2.4: "API Keys - Podstawowa autoryzacja" (40 min)

**Cel:** Implementacja prostej autoryzacji za pomocą API Keys

**Krok 1: Teoria (10 min)**

**Dlaczego potrzebujemy autoryzacji?**
- 🔒 Ochrona zasobów przed nieautoryzowanym dostępem
- 📊 Monitorowanie użycia API (rate limiting)
- 💰 Rozliczenia za użycie (w płatnych API)
- 🎯 Personalizacja (każdy użytkownik widzi swoje dane)

**Metody autoryzacji:**
1. **API Key** - prosty klucz w nagłówku lub URL (najprostsze)
2. **Bearer Token (JWT)** - token z informacjami o użytkowniku (popularne)
3. **OAuth 2.0** - delegacja uprawnień (np. "Zaloguj przez Google")
4. **Basic Auth** - username + password w Base64 (przestarzałe dla API)

**Krok 2: Implementacja API Keys (30 min)**

Rozbuduj `app.py`:

```python
from flask import Flask, jsonify, request
from functools import wraps

app = Flask(__name__)

# Symulowana baza API keys
API_KEYS = {
    "sk_test_1234567890": {"user": "jan", "tier": "free"},
    "sk_live_abcdefghij": {"user": "anna", "tier": "premium"}
}

# Dekorator do sprawdzania API key
def require_api_key(f):
    @wraps(f)
    def decorated_function(*args, **kwargs):
        # Sprawdź API key w nagłówku
        api_key = request.headers.get('X-API-Key')
        
        if not api_key:
            return jsonify({
                "error": "Unauthorized",
                "message": "API key is missing"
            }), 401
        
        if api_key not in API_KEYS:
            return jsonify({
                "error": "Unauthorized",
                "message": "Invalid API key"
            }), 401
        
        # Przekaż informacje o użytkowniku do funkcji
        kwargs['user_info'] = API_KEYS[api_key]
        return f(*args, **kwargs)
    
    return decorated_function

# Endpoint publiczny (bez autoryzacji)
@app.route('/api/public')
def public():
    return jsonify({"message": "To jest publiczny endpoint"})

# Endpoint chroniony
@app.route('/api/protected')
@require_api_key
def protected(user_info):
    return jsonify({
        "message": "Witaj w chronionym endpoincie!",
        "user": user_info['user'],
        "tier": user_info['tier']
    })

# CRUD z autoryzacją - każdy użytkownik widzi tylko swoje zadania
user_tasks = {
    "jan": [
        {"id": 1, "title": "Zadanie Jana", "done": False}
    ],
    "anna": [
        {"id": 1, "title": "Zadanie Anny", "done": False}
    ]
}

@app.route('/api/my/tasks', methods=['GET'])
@require_api_key
def get_my_tasks(user_info):
    user = user_info['user']
    tasks = user_tasks.get(user, [])
    return jsonify({"tasks": tasks, "owner": user})

@app.route('/api/my/tasks', methods=['POST'])
@require_api_key
def create_my_task(user_info):
    user = user_info['user']
    data = request.get_json()
    
    if 'title' not in data:
        return jsonify({"error": "Title is required"}), 400
    
    if user not in user_tasks:
        user_tasks[user] = []
    
    new_task = {
        "id": len(user_tasks[user]) + 1,
        "title": data['title'],
        "done": False
    }
    user_tasks[user].append(new_task)
    
    return jsonify({"task": new_task}), 201

if __name__ == '__main__':
    app.run(debug=True, port=5000)
```

**Testuj w Postmanie:**

✅ **Test 1:** Endpoint publiczny (bez API key)
- `GET http://localhost:5000/api/public`
- Powinno działać bez problemu

✅ **Test 2:** Endpoint chroniony bez API key
- `GET http://localhost:5000/api/protected`
- Oczekiwany wynik: 401 Unauthorized

✅ **Test 3:** Endpoint chroniony z API key
- `GET http://localhost:5000/api/protected`
- W zakładce **Headers** dodaj:
  - Key: `X-API-Key`
  - Value: `sk_test_1234567890`
- Powinno zwrócić dane użytkownika "jan"

✅ **Test 4:** Różni użytkownicy widzą różne zadania
- `GET http://localhost:5000/api/my/tasks` z kluczem Jana
- `GET http://localhost:5000/api/my/tasks` z kluczem Anny
- Porównaj wyniki!

**💡 Best Practices:**
- ❌ NIE umieszczaj API key w URL (będzie w logach!)
- ✅ Używaj nagłówków (X-API-Key lub Authorization)
- ✅ Używaj HTTPS w produkcji (szyfrowanie)
- ✅ Implementuj rate limiting (limit zapytań na godzinę)
- ✅ Rotuj klucze regularnie

---

#### 🎯 Ćwiczenie 2.5: "JWT Tokens - Nowoczesna autoryzacja" (50 min)

**Cel:** Zrozumienie i implementacja JSON Web Tokens

**Krok 1: Co to jest JWT? (15 min)**

**JWT (JSON Web Token)** to sposób na bezpieczne przesyłanie informacji między stronami jako obiekt JSON.

**Struktura JWT:**
```
xxxxx.yyyyy.zzzzz
Header.Payload.Signature
```

**Przykład zdekodowanego JWT:**

**Header (czerwony):**
```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

**Payload (fioletowy):**
```json
{
  "sub": "1234567890",
  "name": "Jan Kowalski",
  "admin": true,
  "iat": 1516239022
}
```

**Signature (niebieski):**
```
HMACSHA256(
  base64UrlEncode(header) + "." + base64UrlEncode(payload),
  secret
)
```

**Zalety JWT:**
- ✅ Bezstanowe (serwer nie musi przechowywać sesji)
- ✅ Skalowalne (działa w systemach rozproszonych)
- ✅ Zawiera dane użytkownika (nie trzeba pytać bazy za każdym razem)
- ✅ Ma wygaśnięcie (automatyczne wylogowanie)

**Krok 2: Instalacja i implementacja (25 min)**

```bash
pip install pyjwt
```

Utwórz `auth_jwt.py`:

```python
from flask import Flask, jsonify, request
from functools import wraps
import jwt
import datetime

app = Flask(__name__)

# WAŻNE: W produkcji użyj zmiennej środowiskowej!
app.config['SECRET_KEY'] = 'super-secret-key-change-in-production'

# Symulowana baza użytkowników
users = {
    "jan": {"password": "haslo123", "role": "admin"},
    "anna": {"password": "haslo456", "role": "user"}
}

# Dekorator do wymagania tokenu
def token_required(f):
    @wraps(f)
    def decorated(*args, **kwargs):
        token = request.headers.get('Authorization')
        
        if not token:
            return jsonify({
                'error': 'Token is missing!'
            }), 401
        
        # Usuń 'Bearer ' z początku
        if token.startswith('Bearer '):
            token = token[7:]
        
        try:
            # Dekoduj token
            data = jwt.decode(token, app.config['SECRET_KEY'], algorithms=["HS256"])
            current_user = data['username']
        except jwt.ExpiredSignatureError:
            return jsonify({
                'error': 'Token has expired!'
            }), 401
        except jwt.InvalidTokenError:
            return jsonify({
                'error': 'Invalid token!'
            }), 401
        
        return f(current_user=current_user, *args, **kwargs)
    
    return decorated

# Endpoint logowania - generuje token
@app.route('/api/login', methods=['POST'])
def login():
    data = request.get_json()
    
    username = data.get('username')
    password = data.get('password')
    
    # Sprawdź dane logowania
    if username not in users or users[username]['password'] != password:
        return jsonify({
            'error': 'Invalid credentials!'
        }), 401
    
    # Utwórz token JWT
    token = jwt.encode({
        'username': username,
        'role': users[username]['role'],
        'exp': datetime.datetime.utcnow() + datetime.timedelta(hours=24)
    }, app.config['SECRET_KEY'], algorithm="HS256")
    
    return jsonify({
        'token': token,
        'message': 'Login successful!',
        'expires_in': '24 hours'
    })

# Endpoint chroniony tokenem
@app.route('/api/dashboard')
@token_required
def dashboard(current_user):
    return jsonify({
        'message': f'Welcome to your dashboard, {current_user}!',
        'user': current_user
    })

# Endpoint tylko dla adminów
@app.route('/api/admin')
@token_required
def admin_only(current_user):
    # Sprawdź rolę
    if users[current_user]['role'] != 'admin':
        return jsonify({
            'error': 'Admin access required!'
        }), 403
    
    return jsonify({
        'message': 'Welcome to admin panel!',
        'users': list(users.keys())
    })

if __name__ == '__main__':
    app.run(debug=True, port=5001)
```

**Krok 3: Testowanie (10 min)**

**Workflow w Postmanie:**

1️⃣ **Zaloguj się:**
- `POST http://localhost:5001/api/login`
- Body (JSON):
```json
{
  "username": "jan",
  "password": "haslo123"
}
```
- **Skopiuj otrzymany token!**

2️⃣ **Użyj tokenu:**
- `GET http://localhost:5001/api/dashboard`
- W Headers dodaj:
  - Key: `Authorization`
  - Value: `Bearer <twój-token>`

3️⃣ **Test uprawnień admina:**
- `GET http://localhost:5001/api/admin` jako "jan" (admin) → powinno działać
- Zaloguj się jako "anna" (user) i spróbuj ponownie → 403 Forbidden

4️⃣ **Test wygasłego tokenu:**
- Zmień w kodzie `timedelta(hours=24)` na `timedelta(seconds=5)`
- Zaloguj się i otrzymaj token
- Poczekaj 10 sekund
- Spróbuj użyć tokenu → 401 Token expired

---

## Dzień 3: Dobre praktyki i projekt

### 🎓 Moduł 3.1: Dobre praktyki REST API (60 min)

#### 🎯 Ćwiczenie 3.1: "Design Your API - Warsztat projektowy" (40 min)

**Cel:** Nauczenie się projektowania intuicyjnych i spójnych endpointów

**Część A: Analiza złych praktyk (15 min)**

Oceń poniższe endpointy i zaproponuj poprawki:

**❌ Źle zaprojektowane API:**
```
GET  /getUserData?id=123
POST /user/create
POST /deleteUser
GET  /users/123/posts/get
PUT  /update-user-profile
GET  /api/v1/users/getAllActiveUsers
```

**✅ Dobrze zaprojektowane API:**
```
GET    /api/v1/users/123
POST   /api/v1/users
DELETE /api/v1/users/123
GET    /api/v1/users/123/posts
PUT    /api/v1/users/123/profile
GET    /api/v1/users?status=active
```

**Zasady REST API Design:**

1. **Używaj rzeczowników, nie czasowników**
   - ❌ `/getUsers`, `/createUser`
   - ✅ `/users` (metoda HTTP określa akcję)

2. **Używaj liczby mnogiej dla kolekcji**
   - ❌ `/user/123`
   - ✅ `/users/123`

3. **Hierarchia zasobów**
   - ✅ `/users/123/posts/456/comments`
   - (posty użytkownika → komentarze do posta)

4. **Używaj query parameters do filtrowania**
   - ✅ `/users?role=admin&status=active`
   - ✅ `/products?category=electronics&price_min=100`

5. **Wersjonowanie API**
   - ✅ `/api/v1/users`
   - ✅ `/api/v2/users`

6. **Spójne nazewnictwo**
   - Używaj snake_case lub camelCase, ale konsekwentnie!
   - ✅ `user_id` lub `userId` (wybierz jedno!)

**Część B: Zaprojektuj API (25 min)**

**Scenariusz:** Tworzysz API dla systemu biblioteki.

**Wymagania:**
- Zarządzanie książkami (tytuł, autor, ISBN, rok, dostępność)
- Zarządzanie użytkownikami (imię, nazwisko, email, rola)
- Wypożyczenia (która książka, który użytkownik, daty)
- Recenzje książek (ocena 1-5, tekst, autor recenzji)

**Zadanie:**
Zaprojektuj RESTful API z następującymi funkcjonalnościami:

1. CRUD dla książek
2. CRUD dla użytkowników
3. Wypożycz książkę
4. Zwróć książkę
5. Zobacz historię wypożyczeń użytkownika
6. Zobacz wszystkie recenzje książki
7. Dodaj recenzję
8. Wyszukaj książki po tytule/autorze

**Przykładowe rozwiązanie:**

```
# Książki
GET    /api/v1/books                    # Lista wszystkich książek
GET    /api/v1/books?available=true     # Tylko dostępne
GET    /api/v1/books?author=Tolkien     # Filtrowanie po autorze
GET    /api/v1/books/search?q=hobbit    # Wyszukiwanie
GET    /api/v1/books/{isbn}             # Szczegóły książki
POST   /api/v1/books                    # Dodaj książkę
PUT    /api/v1/books/{isbn}             # Aktualizuj książkę
DELETE /api/v1/books/{isbn}             # Usuń książkę

# Użytkownicy
GET    /api/v1/users                    # Lista użytkowników
GET    /api/v1/users/{id}               # Profil użytkownika
POST   /api/v1/users                    # Rejestracja
PUT    /api/v1/users/{id}               # Aktualizacja profilu
DELETE /api/v1/users/{id}               # Usuń konto

# Wypożyczenia
GET    /api/v1/users/{id}/loans         # Historia wypożyczeń użytkownika
GET    /api/v1/users/{id}/loans/active  # Aktywne wypożyczenia
POST   /api/v1/loans                    # Wypożycz książkę
PUT    /api/v1/loans/{id}/return        # Zwróć książkę

# Recenzje
GET    /api/v1/books/{isbn}/reviews     # Wszystkie recenzje książki
POST   /api/v1/books/{isbn}/reviews     # Dodaj recenzję
GET    /api/v1/reviews/{id}             # Konkretna recenzja
PUT    /api/v1/reviews/{id}             # Edytuj recenzję
DELETE /api/v1/reviews/{id}             # Usuń recenzję
```

**Dyskusja:** Prowadzący omawia różne podejścia uczestników

---

#### 🎯 Ćwiczenie 3.2: "Rate Limiting & Error Handling" (20 min)

**Cel:** Implementacja ograniczenia liczby zapytań

**Instalacja:**
```bash
pip install flask-limiter
```

**Implementacja:**

```python
from flask import Flask, jsonify
from flask_limiter import Limiter
from flask_limiter.util import get_remote_address

app = Flask(__name__)

# Konfiguracja rate limitera
limiter = Limiter(
    app=app,
    key_func=get_remote_address,  # Limit per IP address
    default_limits=["200 per day", "50 per hour"]
)

# Endpoint z custom limitem
@app.route('/api/search')
@limiter.limit("10 per minute")
def search():
    return jsonify({"message": "Search results..."})

# Endpoint bez limitu (np. health check)
@app.route('/health')
@limiter.exempt
def health():
    return jsonify({"status": "healthy"})

# Custom obsługa przekroczenia limitu
@app.errorhandler(429)
def ratelimit_handler(e):
    return jsonify({
        "error": "Rate limit exceeded",
        "message": "Too many requests. Please try again later.",
        "retry_after": str(e.description)
    }), 429

if __name__ == '__main__':
    app.run(debug=True)
```

**Testowanie:**
Wyślij 15 zapytań do `/api/search` w ciągu minuty - po 10 powinno zwrócić 429!

---

### 🎓 Moduł 3.2: Projekt Końcowy (180 min)

#### 🎯 PROJEKT KOŃCOWY: "Blog API" (150 min + 30 min prezentacje)

**Cel:** Samodzielne stworzenie kompletnego, profesjonalnego API

### Specyfikacja projektu:

**Temat:** RESTful API dla prostego bloga

**Wymagania funkcjonalne:**

**1. Zarządzanie postami:**
- Każdy post ma: `id`, `title`, `content`, `author_id`, `created_at`, `updated_at`, `published`
- Tworzenie nowego posta
- Edycja posta (tylko autor)
- Usuwanie posta (tylko autor)
- Publikowanie/ukrywanie posta
- Wyświetlanie wszystkich opublikowanych postów
- Wyświetlanie postów konkretnego autora

**2. Zarządzanie użytkownikami:**
- Każdy użytkownik ma: `id`, `username`, `email`, `password_hash`, `bio`, `created_at`
- Rejestracja
- Logowanie (zwraca JWT token)
- Wyświetlanie profilu
- Edycja własnego profilu

**3. Komentarze:**
- Każdy komentarz ma: `id`, `post_id`, `author_id`, `content`, `created_at`
- Dodawanie komentarza do posta
- Wyświetlanie wszystkich komentarzy do posta
- Usuwanie komentarza (tylko autor komentarza lub autor posta)

**4. System "Polubień" (Likes):**
- Użytkownik może polubić post
- Użytkownik może usunąć polubienie
- Wyświetlanie liczby polubień posta

**Wymagania techniczne:**

1. **Framework:** Python + Flask
2. **Autoryzacja:** JWT Tokens
3. **Walidacja:** Wszystkie endpointy muszą walidować dane wejściowe
4. **Obsługa błędów:** Profesjonalne komunikaty błędów (400, 401, 403, 404, 500)
5. **Rate limiting:** 100 zapytań na godzinę per użytkownik
6. **Dokumentacja:** Plik README.md + specyfikacja OpenAPI/Swagger
7. **Testy:** Kolekcja w Postmanie z testami automatycznymi (min. 3 testy per endpoint)

**Endpointy do zaimplementowania:**

```
# Autoryzacja
POST   /api/v1/auth/register
POST   /api/v1/auth/login

# Użytkownicy
GET    /api/v1/users/{id}
PUT    /api/v1/users/{id}            # Tylko własny profil

# Posty
GET    /api/v1/posts                 # Tylko opublikowane
GET    /api/v1/posts/{id}
POST   /api/v1/posts                 # Wymaga autoryzacji
PUT    /api/v1/posts/{id}            # Tylko autor
DELETE /api/v1/posts/{id}            # Tylko autor
PUT    /api/v1/posts/{id}/publish    # Tylko autor
GET    /api/v1/users/{id}/posts      # Posty użytkownika

# Komentarze
GET    /api/v1/posts/{id}/comments
POST   /api/v1/posts/{id}/comments   # Wymaga autoryzacji
DELETE /api/v1/comments/{id}         # Autor komentarza lub posta

# Polubienia
POST   /api/v1/posts/{id}/like       # Wymaga autoryzacji
DELETE /api/v1/posts/{id}/like       # Wymaga autoryzacji
GET    /api/v1/posts/{id}/likes      # Liczba i lista użytkowników
```

**Kryteria oceny:**

| Kryterium | Punkty |
|-----------|--------|
| Wszystkie endpointy działają poprawnie | 30 |
| Autoryzacja JWT zaimplementowana | 15 |
| Walidacja danych wejściowych | 10 |
| Profesjonalna obsługa błędów | 10 |
| Czytelny i dobrze zorganizowany kod | 10 |
| Dokumentacja (README + Swagger) | 15 |
| Testy w Postmanie | 10 |
| **Bonus:** Rate limiting | +5 |
| **Bonus:** Paginacja dla listy postów | +5 |

**Czas na realizację:** 150 minut (2.5 godziny)

**Wskazówki:**
- Zacznij od prostej struktury i stopniowo dodawaj funkcjonalności
- Testuj każdy endpoint zaraz po implementacji
- Używaj pomocniczych funkcji (DRY principle)
- Commit często do Git
- Współpracuj z kolegami - możecie się wzajemnie wspierać!

---

#### 🎤 Prezentacje projektów (30 min)

Każdy uczestnik/zespół ma 5 minut na prezentację:

**Struktura prezentacji:**
1. **Demo (2 min):** Pokaż działanie API w Postmanie
2. **Wyzwania (1 min):** Jakie napotkałeś problemy i jak je rozwiązałeś?
3. **Code highlight (1 min):** Pokaż fragment kodu, z którego jesteś najbardziej dumny
4. **Dokumentacja (1 min):** Pokaż swoją dokumentację

**Feedback:** Prowadzący i inni uczestnicy zadają pytania i dzielą się spostrzeżeniami

---

## 📊 Podsumowanie i Certyfikaty

### Co osiągnęliśmy:
✅ Zrozumienie fundamentów API i HTTP  
✅ Praktyczne budowanie REST API  
✅ Autoryzacja i bezpieczeństwo  
✅ Dokumentowanie API  
✅ Testowanie automatyczne  
✅ Projekt końcowy  

### 📚 Dalsze kroki:
1. **Bazy danych:** Integracja z PostgreSQL/MongoDB
2. **Zaawansowane:** GraphQL, WebSockets, gRPC
3. **Deployment:** Docker, Kubernetes, AWS
4. **Monitoring:** Logging, metrics, alerting

### 🎓 Certyfikat ukończenia
Każdy uczestnik, który ukończył projekt końcowy, otrzymuje certyfikat!

---

## 📎 Załączniki

### Przydatne linki:
- [Postman Learning Center](https://learning.postman.com/)
- [Flask Documentation](https://flask.palletsprojects.com/)
- [JWT.io](https://jwt.io/)
- [REST API Tutorial](https://restfulapi.net/)
- [OpenAPI Specification](https://swagger.io/specification/)

### Cheat Sheets:
Do pobrania z repozytorium szkolenia!

---

**🎉 Gratulacje ukończenia szkolenia! Jesteś teraz API Developer! 🎉**
