# 🛠️ Warsztaty Praktyczne - API Training

## Warsztat 1: "API Detective - Odkrywanie prawdziwych API" (45 min)

### 🎯 Cel
Nauczenie się analizy i pracy z prawdziwymi, publicznymi API

### 📋 Przygotowanie
Każdy uczestnik potrzebuje:
- Zainstalowany Postman
- Dostęp do internetu
- Notatnik do zapisywania obserwacji

### 🔍 Zadania

#### Część 1: GitHub API (15 min)

**Endpoint:** `https://api.github.com`

**Zadania do wykonania:**

1. **Pobierz informacje o użytkowniku GitHub**
   ```
   GET https://api.github.com/users/octocat
   ```
   
   **Pytania do analizy:**
   - Ile pól zawiera odpowiedź?
   - Jakie informacje są publiczne o użytkowniku?
   - Co oznacza pole `public_repos`?
   - Jaki jest URL do avatara użytkownika?

2. **Pobierz repozytoria użytkownika**
   ```
   GET https://api.github.com/users/octocat/repos
   ```
   
   **Zadanie:**
   - Znajdź repozytorium z największą liczbą gwiazdek
   - Znajdź najbardziej niedawno zaktualizowane repo
   - Użyj parametrów: `?sort=updated&direction=desc`

3. **Eksploracja Rate Limiting**
   ```
   GET https://api.github.com/rate_limit
   ```
   
   **Obserwacja:**
   - Ile zapytań możesz wykonać bez autoryzacji?
   - Kiedy limit się resetuje?

**Zadanie dodatkowe:**
Znajdź w dokumentacji GitHub API (https://docs.github.com/en/rest), jak wyszukać repozytoria po języku programowania. Znajdź 5 najpopularniejszych repozytoriów w Pythonie.

---

#### Część 2: OpenWeather API (15 min)

**Endpoint:** `https://api.openweathermap.org/data/2.5/weather`

**⚠️ Wymaga API Key!** 
- Zarejestruj się na: https://openweathermap.org/api
- Otrzymasz darmowy API key (może potrwać kilka minut aktywacji)

**Zadania:**

1. **Pogoda dla Twojego miasta**
   ```
   GET https://api.openweathermap.org/data/2.5/weather?q=Warsaw&appid=YOUR_API_KEY&units=metric
   ```
   
   **Zadanie:**
   - Jaka jest aktualna temperatura?
   - Jaka jest wilgotność?
   - Z której strony wieje wiatr?

2. **Eksperymenty z parametrami**
   - Zmień `units=metric` na `units=imperial` - co się zmieniło?
   - Dodaj `lang=pl` - co się zmieniło?
   - Użyj współrzędnych geograficznych zamiast nazwy miasta:
     ```
     ?lat=52.23&lon=21.01&appid=YOUR_API_KEY
     ```

3. **Prognoza 5-dniowa**
   ```
   GET https://api.openweathermap.org/data/2.5/forecast?q=Warsaw&appid=YOUR_API_KEY&units=metric
   ```
   
   **Analiza:**
   - Ile prognoz otrzymujesz?
   - Co ile godzin są prognozy?
   - Znajdź najcieplejszy dzień w prognozie

---

#### Część 3: REST Countries API (15 min)

**Endpoint:** `https://restcountries.com/v3.1`

**Zadania:**

1. **Informacje o Polsce**
   ```
   GET https://restcountries.com/v3.1/name/poland
   ```
   
   **Pytania:**
   - Jaka jest stolica?
   - Ile milionów mieszkańców?
   - Jakie są oficjalne języki?
   - W jakich strefach czasowych leży Polska?

2. **Wyszukiwanie po kodzie waluty**
   ```
   GET https://restcountries.com/v3.1/currency/EUR
   ```
   
   **Zadanie:**
   - Ile krajów używa Euro?
   - Które z nich są w Europie?

3. **Filtrowanie pól (optymalizacja)**
   ```
   GET https://restcountries.com/v3.1/all?fields=name,capital,population
   ```
   
   **Obserwacja:**
   - Porównaj rozmiar odpowiedzi z poprzednim zapytaniem
   - Dlaczego filtrowanie pól jest ważne?

**Zadanie kreatywne:**
Stwórz "quiz geograficzny" - przygotuj 5 pytań bazując na danych z API (np. "Która stolica ma najwięcej mieszkańców w Europie?")

---

## Warsztat 2: "Buduj API - Krok po kroku" (90 min)

### 🎯 Cel
Budowanie kompleksowego API od podstaw z najlepszymi praktykami

### 📦 Temat: API dla sklepu internetowego (E-commerce)

### Krok 1: Projektowanie (15 min)

**Burza mózgów w grupach:**
Jakie endpointy potrzebuje prosty sklep internetowy?

**Wymagane funkcjonalności:**
- Przeglądanie produktów
- Wyszukiwanie i filtrowanie
- Koszyk zakupowy
- Zamówienia
- Profile użytkowników

**Wspólnie zaprojektujcie strukturę endpointów!**

**Przykładowe rozwiązanie:**
```
# Produkty
GET    /api/v1/products
GET    /api/v1/products/{id}
GET    /api/v1/products/search?q=laptop
GET    /api/v1/products?category=electronics&price_max=1000

# Koszyk
GET    /api/v1/cart
POST   /api/v1/cart/items
PUT    /api/v1/cart/items/{id}
DELETE /api/v1/cart/items/{id}

# Zamówienia
GET    /api/v1/orders
GET    /api/v1/orders/{id}
POST   /api/v1/orders
```

---

### Krok 2: Setup projektu (10 min)

```bash
mkdir ecommerce-api
cd ecommerce-api
python -m venv venv
source venv/bin/activate  # lub venv\Scripts\activate na Windows
pip install flask flask-cors
```

Struktura folderów:
```
ecommerce-api/
  ├── app.py
  ├── models.py
  ├── config.py
  ├── requirements.txt
  └── README.md
```

---

### Krok 3: Implementacja modeli danych (15 min)

**`models.py`:**

```python
from datetime import datetime

class Product:
    """Model produktu"""
    def __init__(self, id, name, description, price, category, stock, image_url=None):
        self.id = id
        self.name = name
        self.description = description
        self.price = price
        self.category = category
        self.stock = stock
        self.image_url = image_url
        self.created_at = datetime.now()
    
    def to_dict(self):
        """Konwersja do słownika (JSON-serializable)"""
        return {
            'id': self.id,
            'name': self.name,
            'description': self.description,
            'price': self.price,
            'category': self.category,
            'stock': self.stock,
            'image_url': self.image_url,
            'in_stock': self.stock > 0,
            'created_at': self.created_at.isoformat()
        }

class CartItem:
    """Model elementu koszyka"""
    def __init__(self, product_id, quantity):
        self.product_id = product_id
        self.quantity = quantity
    
    def to_dict(self, product):
        """Konwersja z informacjami o produkcie"""
        return {
            'product': product.to_dict(),
            'quantity': self.quantity,
            'subtotal': product.price * self.quantity
        }

class Order:
    """Model zamówienia"""
    def __init__(self, id, user_id, items, total, status='pending'):
        self.id = id
        self.user_id = user_id
        self.items = items  # lista CartItem
        self.total = total
        self.status = status  # pending, confirmed, shipped, delivered
        self.created_at = datetime.now()
    
    def to_dict(self):
        return {
            'id': self.id,
            'user_id': self.user_id,
            'items': self.items,
            'total': self.total,
            'status': self.status,
            'created_at': self.created_at.isoformat()
        }

# Przykładowe dane (w prawdziwej aplikacji byłaby baza danych)
PRODUCTS = [
    Product(1, "Laptop Dell XPS 13", "Lekki i wydajny laptop", 4999.99, "electronics", 15),
    Product(2, "Klawiatura mechaniczna", "RGB, przełączniki Cherry MX", 399.99, "accessories", 30),
    Product(3, "Mysz bezprzewodowa", "Ergonomiczna, 2.4GHz", 149.99, "accessories", 50),
    Product(4, "Monitor 27\" 4K", "IPS, HDR, 60Hz", 1899.99, "electronics", 8),
    Product(5, "Słuchawki nauszne", "Noise cancelling, Bluetooth", 899.99, "audio", 20),
]

CARTS = {}  # user_id -> lista CartItem
ORDERS = []  # lista Order
```

**Zadanie dla uczestników:**
Dodaj model `User` z polami: id, email, name, address

---

### Krok 4: Implementacja endpointów (30 min)

**`app.py`:**

```python
from flask import Flask, jsonify, request
from flask_cors import CORS
from models import PRODUCTS, CARTS, ORDERS, CartItem, Order

app = Flask(__name__)
CORS(app)  # Umożliwia zapytania z frontendu

# Helper function do znajdowania produktu
def find_product(product_id):
    return next((p for p in PRODUCTS if p.id == product_id), None)

# ==================== PRODUKTY ====================

@app.route('/api/v1/products', methods=['GET'])
def get_products():
    """Pobierz wszystkie produkty z opcjonalnym filtrowaniem"""
    category = request.args.get('category')
    price_max = request.args.get('price_max', type=float)
    in_stock_only = request.args.get('in_stock', type=bool)
    
    filtered = PRODUCTS
    
    # Filtrowanie
    if category:
        filtered = [p for p in filtered if p.category == category]
    if price_max:
        filtered = [p for p in filtered if p.price <= price_max]
    if in_stock_only:
        filtered = [p for p in filtered if p.stock > 0]
    
    return jsonify({
        'products': [p.to_dict() for p in filtered],
        'count': len(filtered)
    })

@app.route('/api/v1/products/<int:product_id>', methods=['GET'])
def get_product(product_id):
    """Pobierz szczegóły produktu"""
    product = find_product(product_id)
    if not product:
        return jsonify({'error': 'Product not found'}), 404
    return jsonify({'product': product.to_dict()})

@app.route('/api/v1/products/search', methods=['GET'])
def search_products():
    """Wyszukaj produkty"""
    query = request.args.get('q', '').lower()
    if not query:
        return jsonify({'error': 'Query parameter "q" is required'}), 400
    
    results = [
        p for p in PRODUCTS 
        if query in p.name.lower() or query in p.description.lower()
    ]
    
    return jsonify({
        'products': [p.to_dict() for p in results],
        'count': len(results),
        'query': query
    })

@app.route('/api/v1/categories', methods=['GET'])
def get_categories():
    """Pobierz listę kategorii"""
    categories = list(set(p.category for p in PRODUCTS))
    return jsonify({'categories': categories})

# ==================== KOSZYK ====================

@app.route('/api/v1/cart', methods=['GET'])
def get_cart():
    """Pobierz zawartość koszyka"""
    # W prawdziwej aplikacji user_id byłby z tokenu JWT
    user_id = request.args.get('user_id', default=1, type=int)
    
    cart_items = CARTS.get(user_id, [])
    
    cart_with_details = []
    total = 0
    
    for item in cart_items:
        product = find_product(item.product_id)
        if product:
            cart_with_details.append(item.to_dict(product))
            total += product.price * item.quantity
    
    return jsonify({
        'items': cart_with_details,
        'total': round(total, 2),
        'items_count': sum(item.quantity for item in cart_items)
    })

@app.route('/api/v1/cart/items', methods=['POST'])
def add_to_cart():
    """Dodaj produkt do koszyka"""
    data = request.get_json()
    user_id = data.get('user_id', 1)
    product_id = data.get('product_id')
    quantity = data.get('quantity', 1)
    
    # Walidacja
    if not product_id:
        return jsonify({'error': 'product_id is required'}), 400
    if quantity < 1:
        return jsonify({'error': 'quantity must be at least 1'}), 400
    
    product = find_product(product_id)
    if not product:
        return jsonify({'error': 'Product not found'}), 404
    if product.stock < quantity:
        return jsonify({'error': f'Only {product.stock} items in stock'}), 400
    
    # Dodaj do koszyka
    if user_id not in CARTS:
        CARTS[user_id] = []
    
    # Sprawdź czy produkt już jest w koszyku
    existing_item = next((item for item in CARTS[user_id] if item.product_id == product_id), None)
    if existing_item:
        existing_item.quantity += quantity
    else:
        CARTS[user_id].append(CartItem(product_id, quantity))
    
    return jsonify({'message': 'Product added to cart'}), 201

@app.route('/api/v1/cart/items/<int:product_id>', methods=['PUT'])
def update_cart_item(product_id):
    """Zaktualizuj ilość produktu w koszyku"""
    data = request.get_json()
    user_id = data.get('user_id', 1)
    quantity = data.get('quantity')
    
    if quantity < 1:
        return jsonify({'error': 'quantity must be at least 1'}), 400
    
    if user_id not in CARTS:
        return jsonify({'error': 'Cart is empty'}), 404
    
    item = next((item for item in CARTS[user_id] if item.product_id == product_id), None)
    if not item:
        return jsonify({'error': 'Product not in cart'}), 404
    
    product = find_product(product_id)
    if product.stock < quantity:
        return jsonify({'error': f'Only {product.stock} items in stock'}), 400
    
    item.quantity = quantity
    return jsonify({'message': 'Cart updated'})

@app.route('/api/v1/cart/items/<int:product_id>', methods=['DELETE'])
def remove_from_cart(product_id):
    """Usuń produkt z koszyka"""
    user_id = request.args.get('user_id', default=1, type=int)
    
    if user_id not in CARTS:
        return jsonify({'error': 'Cart is empty'}), 404
    
    CARTS[user_id] = [item for item in CARTS[user_id] if item.product_id != product_id]
    
    return jsonify({'message': 'Product removed from cart'})

# ==================== ZAMÓWIENIA ====================

@app.route('/api/v1/orders', methods=['POST'])
def create_order():
    """Utwórz zamówienie z zawartości koszyka"""
    data = request.get_json()
    user_id = data.get('user_id', 1)
    
    if user_id not in CARTS or not CARTS[user_id]:
        return jsonify({'error': 'Cart is empty'}), 400
    
    # Oblicz total i przygotuj items
    total = 0
    order_items = []
    
    for cart_item in CARTS[user_id]:
        product = find_product(cart_item.product_id)
        if not product:
            continue
        
        # Sprawdź dostępność
        if product.stock < cart_item.quantity:
            return jsonify({
                'error': f'Insufficient stock for {product.name}'
            }), 400
        
        # Zmniejsz stock
        product.stock -= cart_item.quantity
        
        order_items.append({
            'product_id': product.id,
            'name': product.name,
            'quantity': cart_item.quantity,
            'price': product.price,
            'subtotal': product.price * cart_item.quantity
        })
        total += product.price * cart_item.quantity
    
    # Utwórz zamówienie
    order = Order(
        id=len(ORDERS) + 1,
        user_id=user_id,
        items=order_items,
        total=round(total, 2)
    )
    ORDERS.append(order)
    
    # Wyczyść koszyk
    CARTS[user_id] = []
    
    return jsonify({
        'message': 'Order created successfully',
        'order': order.to_dict()
    }), 201

@app.route('/api/v1/orders', methods=['GET'])
def get_orders():
    """Pobierz wszystkie zamówienia użytkownika"""
    user_id = request.args.get('user_id', default=1, type=int)
    
    user_orders = [o for o in ORDERS if o.user_id == user_id]
    
    return jsonify({
        'orders': [o.to_dict() for o in user_orders],
        'count': len(user_orders)
    })

@app.route('/api/v1/orders/<int:order_id>', methods=['GET'])
def get_order(order_id):
    """Pobierz szczegóły zamówienia"""
    order = next((o for o in ORDERS if o.id == order_id), None)
    
    if not order:
        return jsonify({'error': 'Order not found'}), 404
    
    return jsonify({'order': order.to_dict()})

# ==================== HEALTHCHECK ====================

@app.route('/health', methods=['GET'])
def health():
    return jsonify({'status': 'healthy', 'api_version': 'v1'})

if __name__ == '__main__':
    app.run(debug=True, port=5000)
```

---

### Krok 5: Testowanie (20 min)

**Scenariusz testowy w Postmanie:**

1. **GET wszystkie produkty**
2. **GET produkty z kategorii "electronics"**
3. **Search produkt po nazwie "laptop"**
4. **POST dodaj laptop do koszyka**
5. **POST dodaj mysz do koszyka**
6. **GET zawartość koszyka - sprawdź total**
7. **PUT zmień ilość laptopa na 2**
8. **POST utwórz zamówienie**
9. **GET lista zamówień**
10. **GET szczegóły ostatniego zamówienia**

**Testy automatyczne w Postmanie:**

```javascript
// Test dla GET /api/v1/products
pm.test("Status is 200", () => {
    pm.response.to.have.status(200);
});

pm.test("Response has products array", () => {
    const json = pm.response.json();
    pm.expect(json).to.have.property('products');
    pm.expect(json.products).to.be.an('array');
});

pm.test("Products have required fields", () => {
    const json = pm.response.json();
    const product = json.products[0];
    pm.expect(product).to.have.property('id');
    pm.expect(product).to.have.property('name');
    pm.expect(product).to.have.property('price');
    pm.expect(product).to.have.property('stock');
});
```

---

## Warsztat 3: "Security Challenge" (60 min)

### 🎯 Cel
Zrozumienie zagrożeń bezpieczeństwa i implementacja ochrony

### 🚨 Scenariusze do zabezpieczenia

#### Challenge 1: "SQL Injection" (15 min)

**Scenariusz:**
Masz endpoint wyszukiwania:
```python
@app.route('/search')
def search():
    query = request.args.get('q')
    # Niebezpieczne! (gdyby była prawdziwa baza danych)
    result = db.execute(f"SELECT * FROM products WHERE name LIKE '%{query}%'")
    return jsonify(result)
```

**Zadanie:**
1. Co się stanie, jeśli użytkownik wpisze: `'; DROP TABLE products; --` ?
2. Jak zabezpieczyć ten endpoint?

**Rozwiązanie:**
```python
# Używaj parametryzowanych zapytań!
@app.route('/search')
def search():
    query = request.args.get('q')
    # Bezpieczne
    result = db.execute(
        "SELECT * FROM products WHERE name LIKE ?", 
        (f'%{query}%',)
    )
    return jsonify(result)
```

---

#### Challenge 2: "Rate Limiting Bypass" (15 min)

**Scenariusz:**
API ma rate limiting, ale atakujący zmienia adres IP przy każdym zapytaniu.

**Zadanie:**
Zaimplementuj rate limiting bazujący na:
1. IP address (podstawowe)
2. API Key (lepsze)
3. User ID z JWT (najlepsze)

**Rozwiązanie:**

```python
from flask_limiter import Limiter
from flask_limiter.util import get_remote_address
import jwt

limiter = Limiter(
    app=app,
    key_func=lambda: get_user_id_from_token() or get_remote_address(),
    default_limits=["100 per hour"]
)

def get_user_id_from_token():
    """Wyciągnij user ID z JWT tokenu"""
    token = request.headers.get('Authorization', '').replace('Bearer ', '')
    if token:
        try:
            payload = jwt.decode(token, app.config['SECRET_KEY'], algorithms=['HS256'])
            return f"user_{payload['user_id']}"
        except:
            pass
    return None
```

---

#### Challenge 3: "Broken Authentication" (15 min)

**Scenariusz:**
Endpoint pozwala pobrać dane użytkownika bez weryfikacji tożsamości:

```python
@app.route('/api/users/<int:user_id>')
def get_user(user_id):
    user = find_user(user_id)
    return jsonify(user.to_dict())  # ❌ Każdy może zobaczyć dane każdego!
```

**Zadanie:**
Zabezpiecz endpoint tak, aby:
1. Użytkownik mógł zobaczyć tylko własne dane
2. Admin mógł zobaczyć dane wszystkich

**Rozwiązanie:**

```python
from functools import wraps

def require_auth(f):
    @wraps(f)
    def decorated(*args, **kwargs):
        token = request.headers.get('Authorization', '').replace('Bearer ', '')
        if not token:
            return jsonify({'error': 'Token required'}), 401
        
        try:
            payload = jwt.decode(token, app.config['SECRET_KEY'], algorithms=['HS256'])
            kwargs['current_user_id'] = payload['user_id']
            kwargs['is_admin'] = payload.get('is_admin', False)
        except:
            return jsonify({'error': 'Invalid token'}), 401
        
        return f(*args, **kwargs)
    return decorated

@app.route('/api/users/<int:user_id>')
@require_auth
def get_user(user_id, current_user_id, is_admin):
    # Sprawdź uprawnienia
    if user_id != current_user_id and not is_admin:
        return jsonify({'error': 'Forbidden'}), 403
    
    user = find_user(user_id)
    return jsonify(user.to_dict())
```

---

#### Challenge 4: "Sensitive Data Exposure" (15 min)

**Problem:**
API zwraca za dużo danych, łącznie z wrażliwymi informacjami.

```python
@app.route('/api/users/<int:user_id>')
def get_user(user_id):
    user = find_user(user_id)
    return jsonify({
        'id': user.id,
        'username': user.username,
        'email': user.email,
        'password_hash': user.password_hash,  # ❌ NIGDY!
        'credit_card': user.credit_card,       # ❌ NIGDY!
        'ssn': user.ssn,                       # ❌ NIGDY!
    })
```

**Zadanie:**
Stwórz bezpieczną serializację użytkownika.

**Rozwiązanie:**

```python
class User:
    def __init__(self, id, username, email, password_hash, credit_card, role):
        self.id = id
        self.username = username
        self.email = email
        self.password_hash = password_hash
        self.credit_card = credit_card
        self.role = role
    
    def to_public_dict(self):
        """Publiczne dane - dla wszystkich"""
        return {
            'id': self.id,
            'username': self.username,
        }
    
    def to_private_dict(self):
        """Prywatne dane - tylko dla właściciela"""
        return {
            'id': self.id,
            'username': self.username,
            'email': self.email,
            'role': self.role,
            # NIE zawiera password_hash ani credit_card!
        }
    
    def to_admin_dict(self):
        """Pełne dane - tylko dla adminów"""
        return {
            'id': self.id,
            'username': self.username,
            'email': self.email,
            'role': self.role,
            'has_payment_method': bool(self.credit_card),  # Nie pokazuj numeru!
            'account_created': self.created_at
        }

# Użycie
@app.route('/api/users/<int:user_id>')
@require_auth
def get_user(user_id, current_user_id, is_admin):
    user = find_user(user_id)
    
    if is_admin:
        return jsonify(user.to_admin_dict())
    elif user_id == current_user_id:
        return jsonify(user.to_private_dict())
    else:
        return jsonify(user.to_public_dict())
```

---

### 🏆 Security Checklist

Po zakończeniu warsztatów sprawdź swoje API:

- [ ] Wszystkie hasła są hashowane (bcrypt, argon2)
- [ ] Używasz HTTPS w produkcji
- [ ] Rate limiting jest włączony
- [ ] Tokeny JWT mają expiration time
- [ ] Walidujesz wszystkie dane wejściowe
- [ ] Nie logujesz wrażliwych danych
- [ ] Używasz parametryzowanych zapytań SQL
- [ ] CORS jest odpowiednio skonfigurowany
- [ ] Zwracasz tylko niezbędne dane
- [ ] Masz obsługę błędów (nie ujawniasz stack trace)

---

## 🎉 Podsumowanie Warsztatów

Gratulacje! Zrealizowaliście:
✅ Analizę prawdziwych API  
✅ Budowanie kompleksowego e-commerce API  
✅ Zabezpieczanie API przed atakami  

**Kolejne kroki:**
- Dodaj prawdziwą bazę danych (PostgreSQL, MongoDB)
- Zaimplementuj pełny system autoryzacji
- Dodaj testy jednostkowe (pytest)
- Wdróż na serwer (Heroku, AWS, DigitalOcean)

**Keep coding! 🚀**
