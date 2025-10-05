# 🎴 Karty Ćwiczeń - Poziomy Zaawansowania

## Jak korzystać z kart?

Każda karta zawiera:
- 🎯 **Cel** - co osiągniesz
- ⏱️ **Czas** - szacowany czas wykonania
- 📊 **Poziom** - 🟢 Podstawowy | 🟡 Średnio-zaawansowany | 🔴 Zaawansowany
- ✅ **Kryteria sukcesu** - jak sprawdzić, czy zadanie jest dobrze wykonane

---

# 🟢 POZIOM PODSTAWOWY

## Karta #1: "Pierwsze zapytanie GET"

**🎯 Cel:** Wykonać pierwsze zapytanie HTTP i zinterpretować odpowiedź

**⏱️ Czas:** 10 minut

**📝 Zadanie:**
Użyj Postmana, aby pobrać listę użytkowników z publicznego API.

**Endpoint:** `GET https://jsonplaceholder.typicode.com/users`

**Pytania:**
1. Jaki kod statusu otrzymałeś?
2. Ile użytkowników jest w odpowiedzi?
3. Jakie pola ma każdy użytkownik?
4. Znajdź użytkownika o najdłuższej nazwie firmy
5. Jaki jest email użytkownika o ID 5?

**✅ Kryteria sukcesu:**
- [ ] Wykonano zapytanie pomyślnie (status 200)
- [ ] Odpowiedziano na wszystkie pytania
- [ ] Zapisano zapytanie w kolekcji Postmana

**💡 Wskazówka:** Użyj zakładki "Pretty" w Postmanie, aby łatwiej czytać JSON

---

## Karta #2: "Parametry zapytania (Query Params)"

**🎯 Cel:** Nauczyć się używać parametrów do filtrowania wyników

**⏱️ Czas:** 15 minut

**📝 Zadanie:**
Pracujesz z API blogowym. Użyj parametrów zapytania, aby precyzyjnie filtrować posty.

**Endpoint bazowy:** `GET https://jsonplaceholder.typicode.com/posts`

**Podzadania:**

1. **Pobierz wszystkie posty**
   - URL: `/posts`
   - Ile postów jest w sumie?

2. **Pobierz posty użytkownika o ID 1**
   - Dodaj parametr: `userId=1`
   - Ile postów ma ten użytkownik?

3. **Pobierz pojedynczy post o ID 10**
   - URL: `/posts/10`
   - Jaki jest tytuł tego posta?

4. **Kombinacja parametrów** (eksperyment!)
   - Spróbuj: `/posts?userId=2&_limit=3`
   - Co robi parametr `_limit`?

**✅ Kryteria sukcesu:**
- [ ] Wszystkie podzadania wykonane
- [ ] Rozumiesz różnicę między path parameters (/posts/10) a query parameters (?userId=1)
- [ ] Potrafisz wyjaśnić, kiedy użyć którego

---

## Karta #3: "POST - Tworzenie zasobu"

**🎯 Cel:** Nauczyć się wysyłać dane do API metodą POST

**⏱️ Czas:** 20 minut

**📝 Zadanie:**
Stwórz nowy post na blogu przy użyciu metody POST.

**Endpoint:** `POST https://jsonplaceholder.typicode.com/posts`

**Instrukcje krok po kroku:**

1. W Postmanie wybierz metodę `POST`
2. Wpisz URL
3. Przejdź do zakładki **Headers**:
   - Dodaj: `Content-Type: application/json`
4. Przejdź do zakładki **Body**:
   - Wybierz **raw**
   - Wybierz **JSON**
   - Wpisz:
   ```json
   {
     "title": "Mój pierwszy post przez API",
     "body": "To jest treść mojego posta. Uczę się używać API!",
     "userId": 1
   }
   ```
5. Kliknij **Send**

**Pytania:**
1. Jaki kod statusu otrzymałeś? (Powinno być 201 Created)
2. Jakie ID dostał nowy post?
3. Czy serwer zwrócił cały obiekt posta?

**Eksperymenty:**
- Co się stanie, jeśli wyślesz POST bez pola `title`?
- Co się stanie, jeśli wyślesz POST bez nagłówka `Content-Type`?
- Spróbuj utworzyć post z polskim znakami (ą, ć, ę) - czy działa?

**✅ Kryteria sukcesu:**
- [ ] Post został utworzony pomyślnie (201)
- [ ] Odpowiedziano na wszystkie pytania
- [ ] Przeprowadzono wszystkie eksperymenty

---

## Karta #4: "PUT vs PATCH - Aktualizacja zasobu"

**🎯 Cel:** Zrozumieć różnicę między pełną a częściową aktualizacją

**⏱️ Czas:** 20 minut

**📝 Zadanie:**
Zaktualizuj istniejący post dwiema metodami i porównaj wyniki.

**Część A: PUT (pełna aktualizacja)**

**Endpoint:** `PUT https://jsonplaceholder.typicode.com/posts/1`

**Body:**
```json
{
  "id": 1,
  "title": "Zaktualizowany tytuł",
  "body": "Całkowicie nowa treść",
  "userId": 1
}
```

**Obserwacja:** Porównaj odpowiedź - wszystkie pola zostały zastąpione

**Część B: PATCH (częściowa aktualizacja)**

**Endpoint:** `PATCH https://jsonplaceholder.typicode.com/posts/1`

**Body:**
```json
{
  "title": "Tylko tytuł został zmieniony"
}
```

**Obserwacja:** Tylko pole `title` się zmieniło, reszta bez zmian

**Pytania refleksyjne:**
1. Kiedy użyjesz PUT, a kiedy PATCH?
2. Co się stanie, jeśli w PUT zapomnisz jakiegoś pola?
3. Która metoda jest bardziej ekonomiczna (mniej danych)?

**Real-world przykład:**
Masz profil użytkownika z 20 polami. Użytkownik chce zmienić tylko zdjęcie profilowe.
- PUT: musiałbyś wysłać wszystkie 20 pól
- PATCH: wysyłasz tylko `{"avatar_url": "new_url.jpg"}`

**✅ Kryteria sukcesu:**
- [ ] Wykonano oba typy aktualizacji
- [ ] Rozumiesz różnicę między PUT a PATCH
- [ ] Potrafisz wybrać odpowiednią metodę dla danego scenariusza

---

## Karta #5: "DELETE i obsługa błędów"

**🎯 Cel:** Nauczyć się usuwać zasoby i obsługiwać błędy 404

**⏱️ Czas:** 15 minut

**📝 Zadanie:**

**Część A: Poprawne usunięcie**

1. `DELETE https://jsonplaceholder.typicode.com/posts/1`
2. Jaki kod statusu otrzymałeś?
3. Czy serwer zwrócił jakieś dane?

**Część B: Usunięcie nieistniejącego zasobu**

1. `DELETE https://jsonplaceholder.typicode.com/posts/99999`
2. Jaki kod statusu otrzymałeś?
3. Jaka jest treść błędu?

**Część C: Scenariusze błędów**

Wypróbuj następujące scenariusze i zanotuj kody statusu:

1. GET na nieistniejący zasób: `GET /posts/99999`
2. POST bez wymaganego pola (usuń `title` z body)
3. PUT na nieistniejący zasób: `PUT /posts/99999`

**Tworzenie tabeli błędów:**

| Scenariusz | Kod statusu | Znaczenie | Co zrobić? |
|------------|-------------|-----------|------------|
| GET /posts/99999 | ??? | ??? | ??? |
| POST bez title | ??? | ??? | ??? |
| DELETE /posts/1 | ??? | ??? | ??? |

**✅ Kryteria sukcesu:**
- [ ] Wypełniono tabelę błędów
- [ ] Rozumiesz różnicę między 4xx a 5xx
- [ ] Wiesz, jak reagować na różne kody błędów

---

# 🟡 POZIOM ŚREDNIO-ZAAWANSOWANY

## Karta #6: "Autoryzacja API Key"

**🎯 Cel:** Zaimplementować autoryzację przy użyciu API Key

**⏱️ Czas:** 30 minut

**📝 Zadanie:**
Stwórz endpoint chroniony API Key i przetestuj różne scenariusze dostępu.

**Kod do implementacji (Flask):**

```python
from flask import Flask, request, jsonify
from functools import wraps

app = Flask(__name__)

# Symulowana baza kluczy API
VALID_API_KEYS = {
    'key_12345_test': {'user': 'jan', 'plan': 'free', 'requests_limit': 100},
    'key_67890_prod': {'user': 'anna', 'plan': 'premium', 'requests_limit': 10000}
}

def require_api_key(f):
    @wraps(f)
    def decorated(*args, **kwargs):
        api_key = request.headers.get('X-API-Key')
        
        if not api_key:
            return jsonify({'error': 'API key missing'}), 401
        
        if api_key not in VALID_API_KEYS:
            return jsonify({'error': 'Invalid API key'}), 401
        
        # Przekaż info o użytkowniku
        kwargs['api_info'] = VALID_API_KEYS[api_key]
        return f(*args, **kwargs)
    
    return decorated

@app.route('/api/public')
def public_endpoint():
    return jsonify({'message': 'This is public, no key needed'})

@app.route('/api/protected')
@require_api_key
def protected_endpoint(api_info):
    return jsonify({
        'message': 'Welcome to protected endpoint',
        'user': api_info['user'],
        'plan': api_info['plan']
    })

@app.route('/api/premium')
@require_api_key
def premium_endpoint(api_info):
    if api_info['plan'] != 'premium':
        return jsonify({'error': 'Premium plan required'}), 403
    
    return jsonify({'message': 'Premium content here!'})

if __name__ == '__main__':
    app.run(debug=True, port=5000)
```

**Scenariusze testowe w Postmanie:**

| # | Endpoint | API Key | Oczekiwany wynik |
|---|----------|---------|------------------|
| 1 | GET /api/public | (brak) | 200 OK |
| 2 | GET /api/protected | (brak) | 401 Unauthorized |
| 3 | GET /api/protected | key_12345_test | 200 OK |
| 4 | GET /api/protected | wrong_key | 401 Unauthorized |
| 5 | GET /api/premium | key_12345_test | 403 Forbidden |
| 6 | GET /api/premium | key_67890_prod | 200 OK |

**✅ Kryteria sukcesu:**
- [ ] Wszystkie scenariusze testowe przeszły pomyślnie
- [ ] Rozumiesz różnicę między 401 (Unauthorized) a 403 (Forbidden)
- [ ] Potrafisz wyjaśnić, dlaczego API Key w nagłówku jest lepszy niż w URL

**💡 Best Practices:**
- ❌ Nigdy: `GET /api/data?api_key=12345` (widoczne w logach!)
- ✅ Zawsze: Header `X-API-Key: 12345`
- ✅ W produkcji: Używaj HTTPS (szyfrowanie)

---

## Karta #7: "JWT Authentication Flow"

**🎯 Cel:** Zaimplementować pełny flow autoryzacji JWT (login → token → dostęp)

**⏱️ Czas:** 45 minut

**📝 Zadanie:**
Stwórz system logowania zwracający JWT token i użyj go do dostępu do chronionych endpointów.

**Krok 1: Implementacja (Flask + PyJWT)**

```python
from flask import Flask, request, jsonify
import jwt
import datetime
from functools import wraps

app = Flask(__name__)
app.config['SECRET_KEY'] = 'super-secret-change-in-production'

# Symulowana baza użytkowników
USERS = {
    'jan@example.com': {
        'password': 'haslo123',  # W produkcji: hashowane!
        'user_id': 1,
        'role': 'admin'
    },
    'anna@example.com': {
        'password': 'haslo456',
        'user_id': 2,
        'role': 'user'
    }
}

def token_required(f):
    @wraps(f)
    def decorated(*args, **kwargs):
        token = request.headers.get('Authorization')
        
        if not token:
            return jsonify({'error': 'Token is missing'}), 401
        
        # Usuń "Bearer " prefix
        if token.startswith('Bearer '):
            token = token[7:]
        
        try:
            data = jwt.decode(token, app.config['SECRET_KEY'], algorithms=["HS256"])
            current_user = {
                'user_id': data['user_id'],
                'email': data['email'],
                'role': data['role']
            }
        except jwt.ExpiredSignatureError:
            return jsonify({'error': 'Token has expired'}), 401
        except jwt.InvalidTokenError:
            return jsonify({'error': 'Invalid token'}), 401
        
        return f(current_user=current_user, *args, **kwargs)
    
    return decorated

@app.route('/api/login', methods=['POST'])
def login():
    data = request.get_json()
    email = data.get('email')
    password = data.get('password')
    
    # Walidacja
    if not email or not password:
        return jsonify({'error': 'Email and password required'}), 400
    
    # Sprawdź credentials
    user = USERS.get(email)
    if not user or user['password'] != password:
        return jsonify({'error': 'Invalid credentials'}), 401
    
    # Generuj token JWT
    token = jwt.encode({
        'user_id': user['user_id'],
        'email': email,
        'role': user['role'],
        'exp': datetime.datetime.utcnow() + datetime.timedelta(hours=24)
    }, app.config['SECRET_KEY'], algorithm='HS256')
    
    return jsonify({
        'token': token,
        'user_id': user['user_id'],
        'email': email,
        'expires_in': '24 hours'
    })

@app.route('/api/profile', methods=['GET'])
@token_required
def get_profile(current_user):
    return jsonify({
        'profile': current_user
    })

@app.route('/api/admin', methods=['GET'])
@token_required
def admin_only(current_user):
    if current_user['role'] != 'admin':
        return jsonify({'error': 'Admin access required'}), 403
    
    return jsonify({
        'message': 'Welcome to admin panel',
        'users': list(USERS.keys())
    })

if __name__ == '__main__':
    app.run(debug=True, port=5000)
```

**Krok 2: Testowanie workflow w Postmanie**

**Scenariusz 1: Poprawne logowanie**
1. `POST http://localhost:5000/api/login`
   ```json
   {
     "email": "jan@example.com",
     "password": "haslo123"
   }
   ```
2. Skopiuj otrzymany token
3. `GET http://localhost:5000/api/profile`
   - Header: `Authorization: Bearer <token>`
4. Sprawdź, czy otrzymałeś dane profilu

**Scenariusz 2: Błędne dane logowania**
1. `POST http://localhost:5000/api/login`
   ```json
   {
     "email": "jan@example.com",
     "password": "zle_haslo"
   }
   ```
2. Oczekiwany wynik: 401 Unauthorized

**Scenariusz 3: Dostęp bez tokenu**
1. `GET http://localhost:5000/api/profile`
   - **Bez** nagłówka Authorization
2. Oczekiwany wynik: 401 Token is missing

**Scenariusz 4: Dostęp admina**
1. Zaloguj się jako jan (admin)
2. `GET http://localhost:5000/api/admin` z tokenem
3. Oczekiwany wynik: 200 OK + lista użytkowników

**Scenariusz 5: Użytkownik próbuje dostać się do panelu admina**
1. Zaloguj się jako anna (user)
2. `GET http://localhost:5000/api/admin` z tokenem
3. Oczekiwany wynik: 403 Forbidden

**Krok 3: Analiza tokenu**
1. Wejdź na https://jwt.io/
2. Wklej swój token w sekcji "Encoded"
3. Przeanalizuj sekcje: Header, Payload, Signature

**Pytania:**
1. Jakie dane są w payload tokenu?
2. Czy można odczytać dane z tokenu bez klucza?
3. Czy można zmodyfikować token bez klucza?
4. Po jakim czasie token wygasa?

**✅ Kryteria sukcesu:**
- [ ] Wszystkie 5 scenariuszy testowych działają poprawnie
- [ ] Rozumiesz strukturę JWT (header.payload.signature)
- [ ] Potrafisz wyjaśnić różnicę między 401 a 403
- [ ] Rozumiesz, dlaczego używamy "Bearer" przed tokenem

---

## Karta #8: "Pagination i Filtering"

**🎯 Cel:** Zaimplementować stronicowanie i zaawansowane filtrowanie wyników

**⏱️ Czas:** 40 minut

**📝 Zadanie:**
Stwórz endpoint z paginacją, sortowaniem i filtrowaniem.

**Implementacja:**

```python
from flask import Flask, request, jsonify

app = Flask(__name__)

# Przykładowe dane - 50 produktów
PRODUCTS = [
    {
        'id': i,
        'name': f'Product {i}',
        'price': round(10.0 + i * 5.5, 2),
        'category': ['electronics', 'clothing', 'books', 'food'][i % 4],
        'in_stock': i % 3 != 0,
        'rating': round(1 + (i % 5), 1)
    }
    for i in range(1, 51)
]

@app.route('/api/products', methods=['GET'])
def get_products():
    # Pobierz parametry z query string
    page = request.args.get('page', default=1, type=int)
    per_page = request.args.get('per_page', default=10, type=int)
    category = request.args.get('category', type=str)
    min_price = request.args.get('min_price', type=float)
    max_price = request.args.get('max_price', type=float)
    in_stock_only = request.args.get('in_stock', type=bool)
    sort_by = request.args.get('sort_by', default='id', type=str)
    order = request.args.get('order', default='asc', type=str)
    
    # Walidacja
    if per_page > 100:
        return jsonify({'error': 'per_page cannot exceed 100'}), 400
    if page < 1:
        return jsonify({'error': 'page must be >= 1'}), 400
    
    # Filtrowanie
    filtered = PRODUCTS.copy()
    
    if category:
        filtered = [p for p in filtered if p['category'] == category]
    
    if min_price is not None:
        filtered = [p for p in filtered if p['price'] >= min_price]
    
    if max_price is not None:
        filtered = [p for p in filtered if p['price'] <= max_price]
    
    if in_stock_only:
        filtered = [p for p in filtered if p['in_stock']]
    
    # Sortowanie
    reverse = (order == 'desc')
    if sort_by in ['id', 'price', 'rating']:
        filtered.sort(key=lambda x: x[sort_by], reverse=reverse)
    elif sort_by == 'name':
        filtered.sort(key=lambda x: x['name'], reverse=reverse)
    
    # Paginacja
    total_items = len(filtered)
    total_pages = (total_items + per_page - 1) // per_page
    
    start = (page - 1) * per_page
    end = start + per_page
    paginated = filtered[start:end]
    
    return jsonify({
        'products': paginated,
        'pagination': {
            'page': page,
            'per_page': per_page,
            'total_items': total_items,
            'total_pages': total_pages,
            'has_next': page < total_pages,
            'has_prev': page > 1
        },
        'links': {
            'self': f'/api/products?page={page}&per_page={per_page}',
            'next': f'/api/products?page={page+1}&per_page={per_page}' if page < total_pages else None,
            'prev': f'/api/products?page={page-1}&per_page={per_page}' if page > 1 else None
        }
    })

if __name__ == '__main__':
    app.run(debug=True, port=5000)
```

**Scenariusze testowe:**

**Test 1: Podstawowa paginacja**
```
GET /api/products?page=1&per_page=5
```
Oczekiwany wynik: 5 produktów, total_pages = 10

**Test 2: Druga strona**
```
GET /api/products?page=2&per_page=5
```
Oczekiwany wynik: Produkty 6-10

**Test 3: Filtrowanie po kategorii**
```
GET /api/products?category=electronics
```
Ile produktów z kategorii electronics?

**Test 4: Filtrowanie po cenie**
```
GET /api/products?min_price=50&max_price=100
```

**Test 5: Tylko dostępne produkty**
```
GET /api/products?in_stock=true
```

**Test 6: Sortowanie**
```
GET /api/products?sort_by=price&order=desc
```
Czy produkty są posortowane od najdroższych?

**Test 7: Kombinacja wszystkiego**
```
GET /api/products?category=electronics&min_price=30&in_stock=true&sort_by=price&order=asc&page=1&per_page=5
```

**Zadania dodatkowe:**
1. Dodaj parametr `search` do wyszukiwania po nazwie produktu
2. Dodaj walidację: `sort_by` może być tylko jednym z: id, name, price, rating
3. Dodaj header `X-Total-Count` zwracający całkowitą liczbę produktów

**✅ Kryteria sukcesu:**
- [ ] Wszystkie 7 testów przeszło pomyślnie
- [ ] Paginacja działa poprawnie
- [ ] Linki `next` i `prev` są poprawne
- [ ] Rozumiesz, dlaczego paginacja jest ważna (wydajność, UX)

---

# 🔴 POZIOM ZAAWANSOWANY

## Karta #9: "Rate Limiting z Redis"

**🎯 Cel:** Zaimplementować zaawansowany rate limiting z użyciem Redis

**⏱️ Czas:** 60 minut

**📝 Zadanie:**
Stwórz system rate limitingu, który:
- Limituje zapytania per użytkownik (z JWT)
- Limituje zapytania per IP (dla niezalogowanych)
- Ma różne limity dla różnych planów (free/premium)
- Przechowuje liczniki w Redis

**Wymagania:**
```bash
pip install flask redis flask-limiter
```

**Instalacja Redis:**
```bash
# Mac:
brew install redis
redis-server

# Ubuntu:
sudo apt install redis-server
sudo systemctl start redis

# Windows: 
# Użyj Docker lub WSL
```

**Implementacja:**

```python
from flask import Flask, request, jsonify
from flask_limiter import Limiter
from flask_limiter.util import get_remote_address
import redis
import jwt
from functools import wraps

app = Flask(__name__)
app.config['SECRET_KEY'] = 'your-secret-key'

# Połączenie z Redis
redis_client = redis.Redis(host='localhost', port=6379, db=0, decode_responses=True)

# Custom key function dla limitera
def get_user_identifier():
    """Zwraca identyfikator do rate limitingu: user_id lub IP"""
    token = request.headers.get('Authorization', '').replace('Bearer ', '')
    
    if token:
        try:
            payload = jwt.decode(token, app.config['SECRET_KEY'], algorithms=['HS256'])
            user_id = payload['user_id']
            plan = payload.get('plan', 'free')
            return f"user:{user_id}:{plan}"
        except:
            pass
    
    return f"ip:{get_remote_address()}"

# Konfiguracja limitera
limiter = Limiter(
    app=app,
    key_func=get_user_identifier,
    storage_uri="redis://localhost:6379"
)

# Custom limits dla różnych planów
def dynamic_limit():
    """Zwraca limit w zależności od planu użytkownika"""
    identifier = get_user_identifier()
    
    if ':premium' in identifier:
        return "1000 per hour"
    elif ':free' in identifier:
        return "100 per hour"
    else:  # IP (niezalogowany)
        return "10 per hour"

@app.route('/api/data')
@limiter.limit(dynamic_limit)
def get_data():
    identifier = get_user_identifier()
    
    # Pobierz informacje o limitach z Redis
    limit_info = limiter.limiter.hit(dynamic_limit(), identifier)
    
    return jsonify({
        'data': 'Here is your data',
        'rate_limit': {
            'remaining': limit_info.remaining,
            'limit': limit_info.limit,
            'reset_at': limit_info.reset_at
        }
    })

# Endpoint do sprawdzenia statusu rate limit
@app.route('/api/rate-limit-status')
def rate_limit_status():
    identifier = get_user_identifier()
    
    # Pobierz dane z Redis
    key = f"LIMITER/{identifier}/api/data"
    count = redis_client.get(key) or 0
    ttl = redis_client.ttl(key)
    
    limit_value = dynamic_limit().split()[0]  # "100 per hour" -> "100"
    
    return jsonify({
        'identifier': identifier,
        'requests_made': int(count),
        'limit': limit_value,
        'remaining': int(limit_value) - int(count),
        'resets_in_seconds': ttl
    })

# Custom error handler dla 429
@app.errorhandler(429)
def ratelimit_error(e):
    return jsonify({
        'error': 'Rate limit exceeded',
        'message': str(e.description),
        'retry_after_seconds': e.description
    }), 429

# Endpoint logowania (dla testów)
@app.route('/api/login', methods=['POST'])
def login():
    data = request.get_json()
    email = data.get('email')
    plan = data.get('plan', 'free')  # free lub premium
    
    token = jwt.encode({
        'user_id': 1,
        'email': email,
        'plan': plan
    }, app.config['SECRET_KEY'], algorithm='HS256')
    
    return jsonify({'token': token, 'plan': plan})

if __name__ == '__main__':
    app.run(debug=True, port=5000)
```

**Scenariusze testowe:**

**Test 1: Niezalogowany użytkownik (limit: 10/hour)**
1. Wyślij 12 zapytań: `GET /api/data` (bez tokenu)
2. Po 10 zapytaniach powinieneś otrzymać 429

**Test 2: Użytkownik free (limit: 100/hour)**
1. Zaloguj się: `POST /api/login` z `{"email": "test@test.com", "plan": "free"}`
2. Wyślij 15 zapytań z tokenem
3. Wszystkie powinny przejść (limit 100)

**Test 3: Użytkownik premium (limit: 1000/hour)**
1. Zaloguj się z `{"plan": "premium"}`
2. Sprawdź status: `GET /api/rate-limit-status`
3. Powinien pokazać limit 1000

**Test 4: Reset limitu**
1. Wyczerpaj limit (10 zapytań jako niezalogowany)
2. Sprawdź `resets_in_seconds` w statusie
3. Poczekaj (lub zmień TTL w Redis)
4. Spróbuj ponownie - powinno działać

**Test 5: Monitoring w Redis**
```bash
redis-cli
> KEYS *
> GET "LIMITER/ip:127.0.0.1/api/data"
> TTL "LIMITER/ip:127.0.0.1/api/data"
```

**Zadania dodatkowe:**
1. Dodaj endpoint `/api/admin/reset-limit/<user_id>` (tylko dla adminów) do resetowania limitu użytkownika
2. Zaimplementuj sliding window zamiast fixed window
3. Dodaj możliwość "burst" - pozwól na krótkie przekroczenie limitu
4. Loguj przekroczenia limitu do pliku/bazy danych

**✅ Kryteria sukcesu:**
- [ ] Rate limiting działa dla wszystkich trzech planów
- [ ] Limity są przechowywane w Redis
- [ ] Odpowiedzi 429 zawierają informację o czasie resetu
- [ ] Potrafisz wyjaśnić, czym Redis jest lepszy od pamięci aplikacji

**💡 Dlaczego Redis?**
- ⚡ Bardzo szybki (in-memory database)
- 🔄 Współdzielony między wieloma instancjami API (skalowanie horizontal)
- ⏰ Automatyczne wygasanie kluczy (TTL)
- 📊 Łatwy monitoring

---

## Karta #10: "WebSocket Real-time API"

**🎯 Cel:** Stworzyć real-time API używając WebSocket do czatu na żywo

**⏱️ Czas:** 90 minut

**📝 Zadanie:**
Zbuduj API czatu z real-time komunikacją:
- Użytkownicy mogą dołączyć do pokoi
- Wiadomości są broadcastowane w czasie rzeczywistym
- Historia wiadomości jest zapisywana
- Powiadomienia o dołączeniu/opuszczeniu

**Wymagania:**
```bash
pip install flask flask-socketio python-socketio
```

**Implementacja:**

```python
from flask import Flask, request, jsonify
from flask_socketio import SocketIO, emit, join_room, leave_room
from datetime import datetime

app = Flask(__name__)
app.config['SECRET_KEY'] = 'your-secret-key'
socketio = SocketIO(app, cors_allowed_origins="*")

# Przechowywanie danych w pamięci
chat_rooms = {}  # {room_name: [messages]}
active_users = {}  # {sid: {username, room}}

# REST endpoint - pobierz listę pokoi
@app.route('/api/rooms', methods=['GET'])
def get_rooms():
    rooms_info = []
    for room_name, messages in chat_rooms.items():
        users_in_room = [
            user['username'] 
            for user in active_users.values() 
            if user['room'] == room_name
        ]
        rooms_info.append({
            'name': room_name,
            'users_count': len(users_in_room),
            'messages_count': len(messages),
            'active_users': users_in_room
        })
    return jsonify({'rooms': rooms_info})

# REST endpoint - historia wiadomości pokoju
@app.route('/api/rooms/<room_name>/messages', methods=['GET'])
def get_room_messages(room_name):
    if room_name not in chat_rooms:
        return jsonify({'error': 'Room not found'}), 404
    
    limit = request.args.get('limit', default=50, type=int)
    messages = chat_rooms[room_name][-limit:]  # Ostatnie N wiadomości
    
    return jsonify({
        'room': room_name,
        'messages': messages,
        'total': len(chat_rooms[room_name])
    })

# WebSocket - połączenie
@socketio.on('connect')
def handle_connect():
    print(f'Client connected: {request.sid}')
    emit('connected', {'message': 'Connected to server'})

# WebSocket - rozłączenie
@socketio.on('disconnect')
def handle_disconnect():
    sid = request.sid
    if sid in active_users:
        user_data = active_users[sid]
        username = user_data['username']
        room = user_data['room']
        
        # Powiadom innych w pokoju
        emit('user_left', {
            'username': username,
            'timestamp': datetime.now().isoformat()
        }, room=room)
        
        del active_users[sid]
        print(f'{username} left {room}')

# WebSocket - dołącz do pokoju
@socketio.on('join_room')
def handle_join_room(data):
    username = data.get('username')
    room = data.get('room')
    
    if not username or not room:
        emit('error', {'message': 'Username and room are required'})
        return
    
    # Dołącz do pokoju
    join_room(room)
    
    # Zapisz użytkownika
    active_users[request.sid] = {
        'username': username,
        'room': room
    }
    
    # Utwórz pokój jeśli nie istnieje
    if room not in chat_rooms:
        chat_rooms[room] = []
    
    # Powiadom innych w pokoju
    emit('user_joined', {
        'username': username,
        'timestamp': datetime.now().isoformat()
    }, room=room)
    
    # Wyślij potwierdzenie do użytkownika
    emit('joined_room', {
        'room': room,
        'username': username,
        'message': f'You joined {room}'
    })
    
    print(f'{username} joined {room}')

# WebSocket - opuść pokój
@socketio.on('leave_room')
def handle_leave_room(data):
    room = data.get('room')
    
    if request.sid in active_users:
        username = active_users[request.sid]['username']
        leave_room(room)
        
        # Powiadom innych
        emit('user_left', {
            'username': username,
            'timestamp': datetime.now().isoformat()
        }, room=room)
        
        # Usuń z active_users
        del active_users[request.sid]

# WebSocket - wyślij wiadomość
@socketio.on('send_message')
def handle_send_message(data):
    if request.sid not in active_users:
        emit('error', {'message': 'You must join a room first'})
        return
    
    user_data = active_users[request.sid]
    username = user_data['username']
    room = user_data['room']
    message_text = data.get('message')
    
    if not message_text:
        emit('error', {'message': 'Message cannot be empty'})
        return
    
    # Utwórz obiekt wiadomości
    message = {
        'id': len(chat_rooms[room]) + 1,
        'username': username,
        'message': message_text,
        'timestamp': datetime.now().isoformat()
    }
    
    # Zapisz wiadomość
    chat_rooms[room].append(message)
    
    # Broadcast do wszystkich w pokoju
    emit('new_message', message, room=room)
    
    print(f'[{room}] {username}: {message_text}')

# WebSocket - użytkownik pisze (typing indicator)
@socketio.on('typing')
def handle_typing(data):
    if request.sid in active_users:
        user_data = active_users[request.sid]
        username = user_data['username']
        room = user_data['room']
        is_typing = data.get('is_typing', False)
        
        # Wyślij do innych (nie do siebie)
        emit('user_typing', {
            'username': username,
            'is_typing': is_typing
        }, room=room, skip_sid=request.sid)

if __name__ == '__main__':
    socketio.run(app, debug=True, port=5000)
```

**Klient testowy (HTML + JavaScript):**

Utwórz plik `test_chat.html`:

```html
<!DOCTYPE html>
<html>
<head>
    <title>WebSocket Chat Test</title>
    <script src="https://cdn.socket.io/4.5.4/socket.io.min.js"></script>
    <style>
        body { font-family: Arial, sans-serif; max-width: 800px; margin: 50px auto; }
        #messages { height: 400px; overflow-y: scroll; border: 1px solid #ccc; padding: 10px; margin: 20px 0; }
        .message { margin: 5px 0; padding: 5px; background: #f0f0f0; border-radius: 5px; }
        .system { background: #fff3cd; color: #856404; }
        .typing { font-style: italic; color: #999; }
        input, button { padding: 10px; margin: 5px; }
        button { cursor: pointer; background: #007bff; color: white; border: none; }
    </style>
</head>
<body>
    <h1>WebSocket Chat Test</h1>
    
    <div id="connection-status">Disconnected</div>
    
    <div id="join-form">
        <input type="text" id="username" placeholder="Your username" />
        <input type="text" id="room" placeholder="Room name" value="general" />
        <button onclick="joinRoom()">Join Room</button>
    </div>
    
    <div id="chat" style="display:none;">
        <div id="messages"></div>
        <div id="typing-indicator" class="typing"></div>
        <input type="text" id="message-input" placeholder="Type a message..." />
        <button onclick="sendMessage()">Send</button>
        <button onclick="leaveRoom()">Leave Room</button>
    </div>
    
    <script>
        const socket = io('http://localhost:5000');
        
        let currentRoom = null;
        let currentUsername = null;
        
        // Event listeners
        socket.on('connect', () => {
            document.getElementById('connection-status').textContent = 'Connected';
            document.getElementById('connection-status').style.color = 'green';
        });
        
        socket.on('disconnect', () => {
            document.getElementById('connection-status').textContent = 'Disconnected';
            document.getElementById('connection-status').style.color = 'red';
        });
        
        socket.on('joined_room', (data) => {
            currentRoom = data.room;
            currentUsername = data.username;
            document.getElementById('join-form').style.display = 'none';
            document.getElementById('chat').style.display = 'block';
            addSystemMessage(data.message);
        });
        
        socket.on('user_joined', (data) => {
            addSystemMessage(`${data.username} joined the room`);
        });
        
        socket.on('user_left', (data) => {
            addSystemMessage(`${data.username} left the room`);
        });
        
        socket.on('new_message', (data) => {
            addMessage(data);
        });
        
        socket.on('user_typing', (data) => {
            const indicator = document.getElementById('typing-indicator');
            if (data.is_typing) {
                indicator.textContent = `${data.username} is typing...`;
            } else {
                indicator.textContent = '';
            }
        });
        
        socket.on('error', (data) => {
            alert('Error: ' + data.message);
        });
        
        // Functions
        function joinRoom() {
            const username = document.getElementById('username').value.trim();
            const room = document.getElementById('room').value.trim();
            
            if (!username || !room) {
                alert('Please enter username and room name');
                return;
            }
            
            socket.emit('join_room', { username, room });
        }
        
        function leaveRoom() {
            socket.emit('leave_room', { room: currentRoom });
            document.getElementById('chat').style.display = 'none';
            document.getElementById('join-form').style.display = 'block';
            document.getElementById('messages').innerHTML = '';
            currentRoom = null;
            currentUsername = null;
        }
        
        function sendMessage() {
            const input = document.getElementById('message-input');
            const message = input.value.trim();
            
            if (message) {
                socket.emit('send_message', { message });
                input.value = '';
                socket.emit('typing', { is_typing: false });
            }
        }
        
        function addMessage(data) {
            const messagesDiv = document.getElementById('messages');
            const messageEl = document.createElement('div');
            messageEl.className = 'message';
            
            const time = new Date(data.timestamp).toLocaleTimeString();
            messageEl.innerHTML = `<strong>${data.username}</strong> [${time}]: ${data.message}`;
            
            messagesDiv.appendChild(messageEl);
            messagesDiv.scrollTop = messagesDiv.scrollHeight;
        }
        
        function addSystemMessage(text) {
            const messagesDiv = document.getElementById('messages');
            const messageEl = document.createElement('div');
            messageEl.className = 'message system';
            messageEl.textContent = text;
            messagesDiv.appendChild(messageEl);
        }
        
        // Typing indicator
        let typingTimer;
        document.getElementById('message-input').addEventListener('input', () => {
            socket.emit('typing', { is_typing: true });
            
            clearTimeout(typingTimer);
            typingTimer = setTimeout(() => {
                socket.emit('typing', { is_typing: false });
            }, 1000);
        });
        
        // Send message on Enter
        document.getElementById('message-input').addEventListener('keypress', (e) => {
            if (e.key === 'Enter') {
                sendMessage();
            }
        });
    </script>
</body>
</html>
```

**Scenariusze testowe:**

**Test 1: Podstawowa komunikacja**
1. Otwórz `test_chat.html` w dwóch różnych przeglądarkach/kartach
2. Dołącz jako różni użytkownicy do tego samego pokoju
3. Wyślij wiadomości - czy widzisz je w obu oknach real-time?

**Test 2: REST + WebSocket**
1. Dołącz do pokoju "test" i wyślij kilka wiadomości
2. W Postmanie: `GET /api/rooms`
3. Sprawdź: `GET /api/rooms/test/messages`
4. Czy historia wiadomości jest poprawna?

**Test 3: Typing indicator**
1. Dwie osoby w tym samym pokoju
2. Zacznij pisać (ale nie wysyłaj)
3. Czy druga osoba widzi "username is typing..."?

**Test 4: Powiadomienia join/leave**
1. Użytkownik A już jest w pokoju
2. Użytkownik B dołącza
3. Czy A widzi powiadomienie o dołączeniu B?
4. B opuszcza pokój - czy A widzi powiadomienie?

**Test 5: Wiele pokoi**
1. Utwórz 3 różne pokoje: "general", "tech", "random"
2. W każdym pokoju niech będzie inna osoba
3. Wyślij wiadomości - czy są izolowane per pokój?

**Zadania dodatkowe:**
1. Dodaj prywatne wiadomości (direct messages)
2. Dodaj możliwość edycji/usuwania wiadomości
3. Dodaj emoji reactions do wiadomości
4. Zaimplementuj persystencję (zapisuj do bazy danych)
5. Dodaj autentykację JWT do WebSocket

**✅ Kryteria sukcesu:**
- [ ] Wiadomości są przesyłane w czasie rzeczywistym
- [ ] Historia wiadomości jest dostępna przez REST API
- [ ] Powiadomienia join/leave działają
- [ ] Typing indicator działa
- [ ] Wiadomości w różnych pokojach są izolowane
- [ ] Rozumiesz różnicę między WebSocket a HTTP

**💡 Kiedy używać WebSocket?**
- ✅ Czaty, komunikatory
- ✅ Live updates (giełda, sport)
- ✅ Collaborative editing (Google Docs-style)
- ✅ Gaming
- ❌ Standardowe CRUD operations (REST jest lepszy)

---

## 🎉 Gratulacje!

Jeśli dotarłeś tutaj i wykonałeś wszystkie ćwiczenia, jesteś prawdziwym **API Masterem**! 🏆

**Co osiągnąłeś:**
- ✅ 10 zaawansowanych kart ćwiczeń
- ✅ Od podstaw GET/POST do WebSocket real-time
- ✅ Bezpieczeństwo (JWT, API Keys, Rate Limiting)
- ✅ Zaawansowane techniki (Pagination, Redis, WebSocket)

**Następny level:**
- GraphQL APIs
- gRPC
- API Gateway patterns
- Microservices architecture
- Kubernetes & Docker
- API monitoring & observability

**Keep learning, keep building! 🚀**
