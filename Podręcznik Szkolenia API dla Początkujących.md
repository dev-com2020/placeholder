# Podręcznik Szkolenia API dla Początkujących

## Dzień 1: Fundamenty API i HTTP

### Rozdział 1: Wprowadzenie teoretyczne

#### Czym jest API? (Interfejs Programistyczny Aplikacji)

Interfejs Programistyczny Aplikacji, znany powszechnie jako **API** (z ang. *Application Programming Interface*), to zbiór reguł, protokołów i narzędzi, które umożliwiają różnym aplikacjom i systemom komunikację między sobą. Można go postrzegać jako pośrednika lub tłumacza, który przyjmuje zapytania od jednej aplikacji, przekazuje je do drugiej, a następnie zwraca odpowiedź. Dzięki API deweloperzy nie muszą znać wewnętrznej implementacji systemu, z którym się komunikują; wystarczy, że wiedzą, jakich "komend" (zapytań) użyć, aby uzyskać pożądane dane lub wykonać określoną akcję.

Kluczową zaletą API jest **abstrakcja** – ukrywanie złożoności. Zamiast budować każdą funkcjonalność od zera, deweloperzy mogą wykorzystać gotowe rozwiązania udostępniane przez inne serwisy. To znacznie przyspiesza proces tworzenia oprogramowania i pozwala na integrację zaawansowanych funkcji przy minimalnym wysiłku.

#### Przykłady użycia API w codziennym życiu

API są wszechobecne w cyfrowym świecie, choć często działają w tle, niezauważone przez użytkowników końcowych. Oto kilka popularnych przykładów:

*   **Mapy i lokalizacja:** Aplikacje takie jak Uber czy Bolt, które pokazują mapę i śledzą pozycję kierowcy w czasie rzeczywistym, korzystają z API dostarczanych przez Google Maps lub Mapbox. Zamiast tworzyć własny system map, integrują gotowe rozwiązanie.
*   **Prognoza pogody:** Widgety i aplikacje pogodowe pobierają aktualne dane meteorologiczne z serwisów takich jak OpenWeatherMap czy AccuWeather za pośrednictwem ich API.
*   **Logowanie przez serwisy społecznościowe:** Opcja "Zaloguj się przez Google" lub "Zaloguj się przez Facebooka" to klasyczny przykład użycia API. Strona, na której się logujemy, wysyła zapytanie do API Google/Facebooka w celu weryfikacji tożsamości użytkownika, eliminując potrzebę tworzenia nowego konta.
*   **Płatności online:** Kiedy dokonujemy zakupu w sklepie internetowym, bramki płatnicze (np. Stripe, PayPal) używają API do bezpiecznego przetwarzania transakcji bez ujawniania danych karty kredytowej sprzedawcy.

#### Różnice: REST, SOAP, GraphQL – skupienie na REST

Istnieje kilka stylów architektonicznych i protokołów służących do budowy API. Trzy najpopularniejsze to SOAP, REST i GraphQL. W tym szkoleniu skupimy się głównie na REST, który jest obecnie de facto standardem w tworzeniu webowych API.

| Cecha | REST (Representational State Transfer) | SOAP (Simple Object Access Protocol) | GraphQL (Graph Query Language) |
| :--- | :--- | :--- | :--- |
| **Paradygmat** | Styl architektoniczny, nie protokół | Ściśle zdefiniowany protokół | Język zapytań i środowisko wykonawcze |
| **Format danych** | Głównie JSON, ale też XML, HTML, tekst | Wyłącznie XML | JSON |
| **Komunikacja** | Wykorzystuje standardowe metody HTTP (GET, POST, PUT, DELETE) | Własne standardy (WSDL, XSD), często przez HTTP/SMTP | Zazwyczaj jedno zapytanie POST do jednego endpointu |
| **Struktura** | Zorientowany na zasoby (np. `/users`, `/products`) | Zorientowany na operacje (np. `getUser`, `createProduct`) | Elastyczne zapytania, klient decyduje, jakie dane chce otrzymać |
| **Zalety** | Prostota, elastyczność, skalowalność, dobra wydajność | Wysokie bezpieczeństwo (WS-Security), transakcyjność | Brak nadmiarowych danych (*over-fetching*), mniejsza liczba zapytań |
| **Wady** | Ryzyko *over-fetching* lub *under-fetching* (pobieranie za dużo lub za mało danych) | Złożoność, duży narzut danych (XML) | Bardziej skomplikowany po stronie serwera, trudniejsze cache'owanie |

**REST (Representational State Transfer)**, wprowadzony w 2000 roku przez Roya Fieldinga, zyskał ogromną popularność dzięki swojej prostocie i wykorzystaniu istniejących standardów protokołu HTTP. Jego bezstanowość (*statelessness*) i separacja klienta od serwera sprawiają, że jest idealny do budowy skalowalnych i elastycznych systemów, zwłaszcza w architekturze mikroserwisów i aplikacjach chmurowych [1].

#### Kluczowe pojęcia

Zanim przejdziemy do praktyki, musimy zrozumieć kilka fundamentalnych pojęć związanych z REST API:

*   **Endpoint:** Jest to konkretny adres URL, pod którym dostępne są zasoby API. Każdy endpoint odpowiada za określony zasób lub kolekcję zasobów. Na przykład: `https://api.example.com/users/123` to endpoint prowadzący do danych użytkownika o ID 123.
*   **Request (Zapytanie):** To wiadomość wysyłana przez klienta do serwera w celu wykonania określonej operacji. Składa się z kilku elementów: metody HTTP, endpointu, nagłówków (zawierających m.in. informacje o autoryzacji czy formacie danych) oraz opcjonalnie ciała zapytania (*body*), które przenosi dane (np. przy tworzeniu nowego zasobu).
*   **Response (Odpowiedź):** To wiadomość zwrotna wysyłana przez serwer do klienta po otrzymaniu i przetworzeniu zapytania. Odpowiedź zawiera kod statusu HTTP, nagłówki oraz ciało (*body*), w którym znajdują się żądane dane, najczęściej w formacie JSON.
*   **Status Codes (Kody Statusu HTTP):** To trzycyfrowe kody liczbowe informujące o wyniku przetworzenia zapytania. Pozwalają klientowi szybko zorientować się, czy operacja się powiodła, czy wystąpił błąd i jakiego jest on rodzaju. Najważniejsze grupy kodów to:
    *   `2xx` – Sukces (np. `200 OK`, `201 Created`)
    *   `4xx` – Błąd po stronie klienta (np. `400 Bad Request`, `401 Unauthorized`, `404 Not Found`)
    *   `5xx` – Błąd po stronie serwera (np. `500 Internal Server Error`)

---


### Rozdział 2: Podstawy HTTP i format JSON

Komunikacja w świecie REST API opiera się na dwóch fundamentalnych filarach: protokole **HTTP** (Hypertext Transfer Protocol) oraz formacie wymiany danych **JSON** (JavaScript Object Notation). Zrozumienie ich działania jest kluczowe do efektywnej pracy z API.

#### Metody HTTP: GET, POST, PUT, DELETE

Protokół HTTP definiuje zestaw metod (czasem nazywanych "czasownikami"), które określają, jaką akcję klient chce wykonać na zasobie. W kontekście REST API, cztery podstawowe metody odpowiadają operacjom CRUD (Create, Read, Update, Delete).

*   **GET:** Służy do pobierania (odczytywania) danych. Zapytania GET powinny być **bezpieczne** (*safe*), co oznacza, że nie modyfikują stanu zasobu na serwerze. Są również **idempotentne**, czyli wielokrotne wykonanie tego samego zapytania GET da identyczny rezultat. Przykład: pobranie listy wszystkich produktów.
*   **POST:** Służy do tworzenia nowego zasobu. Zapytania POST nie są ani bezpieczne, ani idempotentne – każde wywołanie powinno skutkować utworzeniem nowego, unikalnego zasobu. Przykład: dodanie nowego użytkownika do bazy danych.
*   **PUT:** Służy do aktualizacji istniejącego zasobu lub jego całkowitego zastąpienia. Jest to metoda idempotentna – wielokrotne wysłanie tego samego zapytania PUT z tymi samymi danymi będzie miało taki sam efekt jak pojedyncze zapytanie. Przykład: aktualizacja danych adresowych klienta.
*   **DELETE:** Służy do usuwania określonego zasobu. Podobnie jak PUT, jest to metoda idempotentna. Po pomyślnym usunięciu zasobu, kolejne próby jego usunięcia nie powinny zmieniać stanu systemu (choć mogą zwracać inny kod statusu, np. 404 zamiast 204) [2].

Istnieje również metoda **PATCH**, która służy do częściowej aktualizacji zasobu. W przeciwieństwie do PUT, które wymaga przesłania całej reprezentacji zasobu, PATCH pozwala na wysłanie tylko tych pól, które mają zostać zmienione. Jej wsparcie bywa jednak niejednolite w różnych systemach.

#### Nagłówki, parametry, body requestu

Każde zapytanie HTTP składa się z kilku części, które razem tworzą kompletne żądanie do serwera.

*   **Nagłówki (Headers):** Przenoszą metadane dotyczące zapytania, takie jak typ oczekiwanej odpowiedzi (`Accept: application/json`), typ zawartości przesyłanej w ciele zapytania (`Content-Type: application/json`) czy dane uwierzytelniające (`Authorization: Bearer <token>`).
*   **Parametry (Parameters):** Służą do precyzowania zapytania, zwłaszcza w przypadku metody GET. Mogą być przekazywane na dwa sposoby:
    *   **Query Parameters:** Dodawane na końcu URL po znaku `?`, np. `/users?page=2&limit=10` (do paginacji wyników).
    *   **Path Parameters:** Wbudowane w ścieżkę URL, np. `/users/123` (do identyfikacji konkretnego zasobu).
*   **Body (Ciało zapytania):** Używane w metodach POST, PUT i PATCH do przesyłania danych na serwer. Najczęściej dane te są formatowane jako obiekt JSON. Metody GET i DELETE zazwyczaj nie posiadają ciała zapytania.

#### Statusy odpowiedzi (200, 400, 401, 404, 500)

Kody statusu HTTP są niezbędne do zrozumienia wyniku operacji API. Oto najważniejsze z nich:

| Kod | Nazwa | Znaczenie |
| :--- | :--- | :--- |
| **200** | **OK** | Standardowa odpowiedź na pomyślne zapytanie GET, PUT lub PATCH. |
| **201** | **Created** | Odpowiedź na pomyślne zapytanie POST, które utworzyło nowy zasób. Odpowiedź zazwyczaj zawiera nagłówek `Location` ze ścieżką do nowego zasobu. |
| **204** | **No Content** | Pomyślna odpowiedź, która nie zwraca żadnej treści w ciele. Często używana przy zapytaniach DELETE. |
| **400** | **Bad Request** | Serwer nie może przetworzyć zapytania z powodu błędu po stronie klienta (np. nieprawidłowa składnia JSON, brak wymaganych pól). |
| **401** | **Unauthorized** | Błąd uwierzytelnienia. Klient musi się zidentyfikować, aby uzyskać dostęp. Dzieje się tak, gdy brakuje tokenu autoryzacyjnego lub jest on nieprawidłowy. |
| **403** | **Forbidden** | Klient jest zidentyfikowany, ale nie ma uprawnień do wykonania danej operacji na zasobie. |
| **404** | **Not Found** | Serwer nie może znaleźć żądanego zasobu. |
| **500** | **Internal Server Error** | Ogólny błąd po stronie serwera. Coś poszło nie tak, ale serwer nie jest w stanie sprecyzować, co dokładnie. |

#### JSON – struktura, składnia, przykłady

**JSON (JavaScript Object Notation)** to lekki format wymiany danych, który jest czytelny zarówno dla ludzi, jak i dla maszyn. Ze względu na swoją prostotę i niezależność od języka programowania, stał się dominującym formatem w komunikacji z REST API [3].

Struktura JSON opiera się na dwóch elementach:

1.  **Obiekty:** Zbiory par klucz-wartość, zamknięte w nawiasach klamrowych `{}`. Klucze muszą być ciągami znaków w podwójnych cudzysłowach.
2.  **Tablice (Arrays):** Uporządkowane listy wartości, zamknięte w nawiasach kwadratowych `[]`.

Wartościami w JSON mogą być:
*   ciągi znaków (string)
*   liczby (number)
*   obiekty (object)
*   tablice (array)
*   wartości logiczne (`true` lub `false`)
*   `null`

**Przykład obiektu JSON reprezentującego użytkownika:**

```json
{
  "userId": 101,
  "firstName": "Jan",
  "lastName": "Kowalski",
  "isActive": true,
  "roles": ["user", "reader"],
  "address": {
    "street": "Kwiatowa 1",
    "city": "Warszawa"
  },
  "lastLogin": null
}
```

Ten prosty, a zarazem elastyczny format pozwala na modelowanie złożonych struktur danych, co czyni go idealnym narzędziem do komunikacji między nowoczesnymi aplikacjami.

---


### Rozdział 4: Architektura prostego API

Zrozumienie podstawowej architektury aplikacji internetowych jest kluczowe do pojęcia, gdzie w tym wszystkim znajduje się API i jaką rolę pełni. Nowoczesne aplikacje najczęściej budowane są w oparciu o model klient-serwer, w którym API stanowi most łączący te dwa światy.

#### Backend i frontend – jak się komunikują

Aplikację internetową można podzielić na dwie główne części:

*   **Frontend (strona klienta):** To wszystko, co użytkownik widzi i z czym wchodzi w interakcję w przeglądarce internetowej. Jest to warstwa prezentacji, zbudowana przy użyciu technologii takich jak HTML, CSS i JavaScript (oraz frameworków typu React, Angular, Vue.js). Frontend odpowiada za wyświetlanie danych i wysyłanie żądań użytkownika do backendu.
*   **Backend (strona serwera):** To "mózg" aplikacji, działający na serwerze. Odpowiada za logikę biznesową, przetwarzanie danych, uwierzytelnianie użytkowników i komunikację z bazą danych. Backend udostępnia swoje funkcje i dane światu zewnętrznemu właśnie za pomocą API.

Komunikacja między frontendem a backendem odbywa się za pośrednictwem protokołu HTTP. Frontend wysyła zapytania HTTP (np. `GET /products`) do API backendu, a backend odsyła odpowiedzi HTTP z danymi (najczęściej w formacie JSON), które frontend następnie renderuje i wyświetla użytkownikowi.

Ta separacja ma ogromne zalety:

*   **Niezależność technologiczna:** Frontend i backend mogą być pisane w zupełnie różnych technologiach. Backend w Pythonie może bez problemu współpracować z frontendem napisanym w React.
*   **Wielokanałowość:** To samo API backendowe może obsługiwać różne typy klientów – nie tylko aplikację webową (frontend), ale także aplikację mobilną (iOS/Android) czy nawet inne serwery.
*   **Lepsza organizacja pracy:** Zespoły deweloperskie mogą pracować równolegle nad frontendem i backendem, o ile uzgodnią wspólną specyfikację API.

#### Czym jest serwer? Jak wygląda request-response flow?

**Serwer** to w uproszczeniu komputer (lub program na komputerze), który jest stale podłączony do sieci i nasłuchuje na przychodzące zapytania. Gdy zapytanie dotrze, serwer je przetwarza i odsyła odpowiedź. W kontekście API, serwerem jest aplikacja backendowa, która hostuje nasze API.

Przepływ zapytania i odpowiedzi (request-response flow) wygląda następująco:

1.  **Akcja użytkownika:** Użytkownik klika przycisk "Pokaż produkty" na stronie internetowej (frontend).
2.  **Zapytanie HTTP:** Przeglądarka (klient) tworzy i wysyła zapytanie HTTP, np. `GET https://api.mojsklep.com/products`.
3.  **Dotarcie do serwera:** Zapytanie podróżuje przez internet i dociera do serwera, na którym działa aplikacja backendowa.
4.  **Routing:** Serwer analizuje endpoint (`/products`) i metodę (`GET`), aby określić, która część kodu powinna obsłużyć to zapytanie (tzw. routing).
5.  **Logika biznesowa:** Odpowiedni fragment kodu (np. funkcja `get_products`) jest wykonywany. Może on np. połączyć się z bazą danych, aby pobrać listę wszystkich produktów.
6.  **Odpowiedź HTTP:** Po pobraniu danych, serwer tworzy odpowiedź HTTP. Ustawia kod statusu (`200 OK`), nagłówki (`Content-Type: application/json`) i umieszcza dane produktów (przekonwertowane na format JSON) w ciele odpowiedzi.
7.  **Powrót do klienta:** Odpowiedź jest wysyłana z powrotem do przeglądarki.
8.  **Renderowanie:** Frontend otrzymuje odpowiedź, parsuje dane JSON i dynamicznie generuje listę produktów, którą wyświetla użytkownikowi.

Cały ten cykl, choć wydaje się skomplikowany, zazwyczaj trwa ułamki sekundy.

#### Przegląd narzędzi do budowy API (Flask / Express / ASP.NET / inne)

Do tworzenia API backendowych można użyć wielu różnych języków programowania i frameworków. Wybór zależy od wymagań projektu, znajomości technologii i preferencji zespołu. Oto kilka popularnych opcji:

| Język | Framework | Opis |
| :--- | :--- | :--- |
| **Python** | **Flask, Django, FastAPI** | **Flask** to lekki "mikro-framework", idealny do szybkiego tworzenia prostych API. **Django** to potężniejszy framework z wieloma wbudowanymi funkcjami ("batteries-included"). **FastAPI** to nowoczesny framework zoptymalizowany pod kątem szybkości i automatycznego generowania dokumentacji. |
| **JavaScript/Node.js** | **Express.js, NestJS** | **Express.js** jest minimalistycznym i elastycznym frameworkiem, który stał się standardem w świecie Node.js. **NestJS** to bardziej rozbudowany framework, który narzuca architekturę inspirowaną Angularem. |
| **C#** | **ASP.NET Core** | Kompleksowa platforma od Microsoftu do budowy nowoczesnych aplikacji webowych i API, znana z wysokiej wydajności i integracji z ekosystemem .NET. |
| **Java** | **Spring Boot, Quarkus** | **Spring Boot** to niezwykle popularny framework, który upraszcza tworzenie aplikacji w ekosystemie Spring. **Quarkus** to nowoczesna alternatywa zoptymalizowana pod kątem chmury i serverless. |
| **PHP** | **Laravel, Symfony** | **Laravel** i **Symfony** to dwa wiodące frameworki PHP, które dostarczają kompletne narzędzia do budowy zaawansowanych API. |
| **Go** | **Gin, Echo** | Język Go, stworzony przez Google, zyskuje na popularności w budowie wysokowydajnych mikroserwisów. **Gin** i **Echo** to popularne, szybkie frameworki do tego celu. |

W ramach tego szkolenia skupimy się na tworzeniu prostego API przy użyciu języka **Python** i frameworka **Flask**, ze względu na jego prostotę i niski próg wejścia.

---


### Rozdział 7: Bezpieczeństwo i autoryzacja

Bezpieczeństwo jest jednym z najważniejszych aspektów projektowania i wdrażania API. Udostępniając dane i funkcjonalności na zewnątrz, musimy mieć pewność, że dostęp do nich mają tylko uprawnione osoby i systemy, a samo API jest odporne na ataki. Dwa kluczowe pojęcia w tym kontekście to **uwierzytelnianie** (authentication) – proces weryfikacji tożsamości klienta, oraz **autoryzacja** (authorization) – proces nadawania uprawnień do konkretnych zasobów.

#### Wprowadzenie do autoryzacji: API key, token JWT

Istnieje wiele metod zabezpieczania dostępu do API. Dwie z najpopularniejszych to klucze API oraz tokeny JWT.

*   **Klucz API (API Key):**
    Jest to najprostsza forma uwierzytelniania. Klient otrzymuje unikalny, wygenerowany przez serwer ciąg znaków (klucz), który musi dołączać do każdego zapytania, zazwyczaj w nagłówku (`Authorization: ApiKey <klucz>`) lub jako parametr zapytania. Serwer, otrzymując zapytanie, sprawdza, czy klucz jest poprawny i czy ma uprawnienia do danego zasobu. Klucze API są łatwe w implementacji, ale mają swoje wady: są statyczne (trudno je unieważnić bez generowania nowego) i zazwyczaj identyfikują całą aplikację, a nie konkretnego użytkownika.

*   **Token JWT (JSON Web Token):**
    To znacznie bardziej zaawansowane i bezpieczniejsze rozwiązanie, które stało się standardem w nowoczesnych aplikacjach. JWT to zaszyfrowany, samowystarczalny token w formacie JSON, który zawiera informacje (tzw. *claims*) o użytkowniku i jego uprawnieniach. Składa się z trzech części: nagłówka, ładunku (payload) i podpisu cyfrowego, który gwarantuje integralność tokenu [3].

    Przepływ wygląda następująco:
    1.  Użytkownik loguje się, podając login i hasło.
    2.  Serwer weryfikuje dane i, jeśli są poprawne, generuje token JWT podpisany tajnym kluczem.
    3.  Token jest odsyłany do klienta, który przechowuje go lokalnie (np. w pamięci przeglądarki).
    4.  Przy każdym kolejnym zapytaniu do chronionych zasobów API, klient dołącza token w nagłówku `Authorization` (np. `Authorization: Bearer <token>`).
    5.  Serwer, otrzymując zapytanie, weryfikuje podpis tokenu. Jeśli jest poprawny, ufa zawartym w nim informacjom i nadaje dostęp bez konieczności ponownego odpytywania bazy danych. Tokeny JWT są bezstanowe i mają określony czas ważności, co znacznie zwiększa bezpieczeństwo.

#### Najczęstsze błędy bezpieczeństwa (CORS, brak uwierzytelnienia)

*   **Brak uwierzytelnienia:** Najpoważniejszy błąd to pozostawienie wrażliwych endpointów bez jakiejkolwiek formy kontroli dostępu. Każdy, kto zna adres URL, może pobrać, zmodyfikować lub usunąć dane.
*   **CORS (Cross-Origin Resource Sharing):** To mechanizm bezpieczeństwa wbudowany w przeglądarki, który blokuje zapytania HTTP pochodzące z innej domeny niż ta, na której znajduje się serwer. Błędna konfiguracja CORS na serwerze (np. zezwolenie na zapytania z dowolnego źródła `*` w środowisku produkcyjnym) może otworzyć drzwi do ataków typu Cross-Site Request Forgery (CSRF).
*   **Przesyłanie wrażliwych danych w URL:** Dane takie jak klucze API czy tokeny nigdy nie powinny być przesyłane jako parametry ścieżki (query parameters), ponieważ adresy URL są często logowane przez serwery pośredniczące i mogą łatwo wyciec.
*   **Brak szyfrowania (HTTPS):** Cała komunikacja z API musi odbywać się przez szyfrowane połączenie HTTPS (SSL/TLS). W przeciwnym razie wszystkie przesyłane dane, włącznie z hasłami i tokenami, mogą zostać przechwycone.

#### Ochrona API przed nadużyciem (rate limiting, throttle)

Nawet poprawnie zabezpieczone API może być celem ataków typu DoS (Denial of Service) lub po prostu nadużywane przez złośliwych bądź nieefektywnie napisanych klientów. Aby temu zapobiec, stosuje się mechanizmy ograniczające liczbę zapytań.

*   **Rate Limiting:** Polega na ograniczeniu liczby zapytań, jakie dany klient (identyfikowany np. po kluczu API lub adresie IP) może wykonać w określonym przedziale czasowym. Na przykład, API może zezwalać na maksymalnie 1000 zapytań na godzinę. Po przekroczeniu tego limitu serwer zacznie odrzucać kolejne zapytania, zwracając kod statusu `429 Too Many Requests`.
*   **Throttling (dławienie):** To bardziej zaawansowana forma rate limitingu, która kontroluje tempo zapytań. Zamiast twardego limitu na godzinę, throttling może np. pozwalać na wykonanie średnio 10 zapytań na sekundę, wygładzając nagłe skoki ruchu i zapewniając stabilność serwera.

Implementacja tych mechanizmów jest kluczowa dla utrzymania dostępności i niezawodności publicznie dostępnego API.

---


### Rozdział 8: Dokumentowanie API

Dobra dokumentacja jest często równie ważna jak samo API. To instrukcja obsługi dla deweloperów, którzy będą korzystać z naszego API. Bez klarownej, kompletnej i aktualnej dokumentacji, nawet najlepiej zaprojektowane API będzie trudne w użyciu i integracji. Dokumentacja jest wizytówką naszego produktu i kluczowym elementem zapewniającym pozytywne doświadczenia deweloperskie (*Developer Experience*).

#### Co powinna zawierać dokumentacja API?

Kompletna dokumentacja powinna służyć jako kompleksowy przewodnik, odpowiadający na wszystkie potencjalne pytania dewelopera. Oto jej kluczowe elementy:

*   **Wprowadzenie i autoryzacja:** Jak uzyskać dostęp do API? Jakie są metody uwierzytelniania (np. jak zdobyć klucz API lub token JWT)? Jakie są podstawowe adresy URL (dla środowiska deweloperskiego i produkcyjnego)?
*   **Opis każdego endpointu:** Dla każdego dostępnego endpointu dokumentacja musi precyzować:
    *   **Metodę HTTP:** GET, POST, PUT, DELETE, etc.
    *   **Pełną ścieżkę URL:** np. `/users/{userId}/orders`.
    *   **Opis działania:** Co ten endpoint robi? (np. "Pobiera listę zamówień dla konkretnego użytkownika").
    *   **Parametry:** Szczegółowy opis wszystkich możliwych parametrów (path, query), ich typów, formatu oraz tego, czy są wymagane.
    *   **Struktura ciała zapytania (Request Body):** Dla metod POST i PUT, dokładny opis struktury JSON, którą należy wysłać, wraz z walidacją dla każdego pola.
    *   **Przykłady odpowiedzi (Response Examples):** Przykładowe ciała odpowiedzi JSON dla różnych scenariuszy (np. pomyślna odpowiedź `200 OK`, błąd `404 Not Found`).
    *   **Kody statusu:** Lista wszystkich możliwych kodów statusu HTTP, które dany endpoint może zwrócić, wraz z ich opisem.
*   **Przykłady kodu:** Gotowe do skopiowania fragmenty kodu w popularnych językach programowania (np. Python, JavaScript, Java, cURL), pokazujące, jak wykonać zapytanie do danego endpointu. To znacznie obniża próg wejścia.
*   **Modele danych:** Opis wszystkich obiektów danych używanych w API, ich pól i typów.
*   **Obsługa błędów:** Ogólny opis formatu odpowiedzi błędów i sposobu ich obsługi.

#### OpenAPI / Swagger – szybki przegląd

Ręczne pisanie i utrzymywanie dokumentacji jest czasochłonne i podatne na błędy. Z pomocą przychodzą standardy i narzędzia, które automatyzują ten proces. Najważniejszym z nich jest **OpenAPI Specification** (wcześniej znana jako Swagger Specification).

**OpenAPI** to standard opisu REST API w sposób niezależny od języka programowania. Specyfikacja API jest tworzona w formacie YAML lub JSON i zawiera wszystkie informacje o endpointach, parametrach, modelach danych i metodach autoryzacji. Taki plik specyfikacji staje się "jedynym ź_ródłem prawdy" o naszym API.

**Swagger** to zestaw narzędzi open-source zbudowanych wokół specyfikacji OpenAPI. Najważ_niejsze z nich to:

*   **Swagger UI:** Generuje interaktywną, piękną dokumentację HTML bezpośrednio z pliku specyfikacji OpenAPI. Umoż_liwia deweloperom nie tylko czytanie dokumentacji, ale także testowanie zapytań API bezpośrednio w przeglądarce.
*   **Swagger Editor:** Edytor online do tworzenia i walidacji plików specyfikacji OpenAPI.
*   **Swagger Codegen:** Narzędzie, które na podstawie pliku specyfikacji może automatycznie wygenerować szkielet kodu serwera lub klienta w różnych językach.

Uż_ycie standardu OpenAPI i narzędzi Swagger pozwala na utrzymanie spójności między kodem a dokumentacją oraz znaczne przyspieszenie cyklu życia API.

#### Generowanie dokumentacji z kodu

Najlepszym podejściem jest generowanie dokumentacji bezpośrednio z kodu ź_ródłowego aplikacji. Wiele nowoczesnych frameworków (np. FastAPI w Pythonie, NestJS w Node.js) ma wbudowane mechanizmy, które na podstawie adnotacji, dekoratorów i typowania w kodzie potrafią automatycznie wygenerować plik specyfikacji OpenAPI. Dzięki temu dokumentacja jest zawsze zsynchronizowana z faktycznym działaniem API. Każ_da zmiana w kodzie (np. dodanie nowego pola w odpowiedzi) jest natychmiast odzwierciedlana w dokumentacji, co eliminuje ryzyko jej dezaktualizacji.

---


### Rozdział 9: Dobre praktyki i cykl życia API

Tworzenie API to nie tylko kwestia technicznej implementacji, ale także sztuka projektowania z myślą o jego użytkownikach – deweloperach. Stosowanie się do ustalonych konwencji i dobrych praktyk sprawia, że API jest intuicyjne, przewidywalne i łatwe w użyciu. Równie ważne jest zrozumienie, że API, podobnie jak każde oprogramowanie, ma swój cykl życia, którym trzeba zarządzać.

#### REST API – konwencje nazw, statusów, wersjonowanie

Aby API było prawdziwie "REST-owe", powinno przestrzegać pewnych konwencji, które ułatwiają jego zrozumienie.

*   **Konwencje nazewnictwa:**
    *   **Używaj rzeczowników w liczbie mnogiej dla kolekcji:** Do reprezentowania kolekcji zasobów używaj rzeczowników w liczbie mnogiej, np. `/users`, `/products`, `/orders`. Unikaj czasowników w URL-ach (np. `getUsers`).
    *   **Używaj ID do identyfikacji pojedynczego zasobu:** Dostęp do konkretnego elementu kolekcji powinien odbywać się przez jego unikalny identyfikator, np. `/users/123`.
    *   **Używaj zagnieżdżania dla relacji:** Jeśli zasoby są ze sobą powiązane, odzwierciedlaj to w strukturze URL, np. `/users/123/orders` (zamówienia użytkownika o ID 123).
    *   **Używaj małych liter:** W URL-ach trzymaj się małych liter, a słowa oddzielaj myślnikami (`-`), aby uniknąć problemów z różnymi systemami operacyjnymi.

*   **Konwencje statusów HTTP:** Używaj kodów statusu HTTP zgodnie z ich semantycznym znaczeniem (jak opisano w Rozdziale 2). Nie zwracaj zawsze `200 OK` z opisem błędu w ciele odpowiedzi. Poprawne kody statusu pozwalają klientom na automatyczne i skuteczne reagowanie na wyniki zapytań.

*   **Wersjonowanie API:**
    API ewoluuje. Wprowadzanie zmian, które łamią kompatybilność wsteczną (tzw. *breaking changes*), jest nieuniknione. Aby nie zepsuć istniejących integracji, kluczowe jest wersjonowanie API. Istnieje kilka popularnych strategii:
    1.  **Wersjonowanie w URL (najpopularniejsze):** Wersja API jest częścią adresu URL, np. `https://api.example.com/v1/users`. Jest to proste i jawne.
    2.  **Wersjonowanie w nagłówku:** Wersja jest przekazywana w niestandardowym nagłówku HTTP, np. `Accept: application/vnd.example.v1+json`.
    3.  **Wersjonowanie przez parametr zapytania:** Wersja jest dodawana jako query parameter, np. `/users?version=1`.

#### Praca zespołowa z API – jak projektować API z myślą o użytkownikach

Projektując API, zawsze stawiaj się w roli dewelopera, który będzie z niego korzystał. Twoim celem jest, aby praca z Twoim API była przyjemnością, a nie frustracją.

*   **Spójność:** Bądź spójny w nazewnictwie, strukturze odpowiedzi i obsłudze błędów. Jeśli jedno zapytanie GET na kolekcję zwraca obiekt z polem `data` zawierającym tablicę, wszystkie inne zapytania na kolekcje też powinny tak robić.
*   **Przewidywalność:** Postępuj zgodnie z ogólnie przyjętymi standardami REST. Deweloper, który zna te standardy, powinien być w stanie intuicyjnie odgadnąć, jak działa Twoje API, nawet bez czytania dokumentacji.
*   **Dostarczaj pomocne komunikaty o błędach:** Zamiast generycznego `{"error": "Bad Request"}`, zwróć szczegółowy opis problemu, np. `{"error": "Field 'email' is required and must be a valid email address."}`.
*   **Podejście "API-First":** Coraz popularniejsze staje się podejście, w którym najpierw projektuje się i dokumentuje API (np. w pliku OpenAPI), a dopiero potem implementuje się logikę. Taka specyfikacja staje się kontraktem między zespołem frontendowym a backendowym, umożliwiając im równoległą pracę.

#### Monitoring, testowanie, utrzymanie

Cykl życia API nie kończy się na jego wdrożeniu. To dopiero początek.

*   **Monitoring:** Niezbędne jest ciągłe monitorowanie działania API. Narzędzia do monitoringu (np. Prometheus, Datadog) pozwalają śledzić kluczowe metryki, takie jak czas odpowiedzi, odsetek błędów (`5xx`), obciążenie serwera. Dzięki temu można proaktywnie wykrywać problemy, zanim zgłoszą je użytkownicy.
*   **Testowanie:** Oprócz testów jednostkowych i integracyjnych, które powinny być częścią procesu CI/CD, ważne są również testy end-to-end, które symulują rzeczywiste scenariusze użycia API. Automatyzacja testów jest kluczowa dla zapewnienia jakości i stabilności przy każdej zmianie w kodzie.
*   **Utrzymanie i wycofywanie (Deprecation):** Gdy wprowadzasz nową wersję API (np. `v2`), stara wersja (`v1`) powinna być jeszcze przez pewien czas utrzymywana, aby dać użytkownikom czas na migrację. Proces wycofywania starej wersji powinien być jasno zakomunikowany, z podaniem konkretnej daty jej wyłączenia. W nagłówkach odpowiedzi wycofywanych endpointów można umieszczać informację o tym fakcie, aby ostrzec deweloperów.

Zarządzanie cyklem życia API to ciągły proces, który wymaga planowania, komunikacji i dbałości o potrzeby jego użytkowników.

---

## Referencje

[1] freeCodeCamp. (2022). *How to Use REST APIs – A Complete Beginner's Guide*. Dostępne na: [https://www.freecodecamp.org/news/how-to-use-rest-api/](https://www.freecodecamp.org/news/how-to-use-rest-api/)

[2] RESTful API Tutorial. (2023). *HTTP Methods*. Dostępne na: [https://restfulapi.net/http-methods/](https://restfulapi.net/http-methods/)

[3] Stack Overflow Blog. (2022). *A beginner's guide to JSON, the data format for the internet*. Dostępne na: [https://stackoverflow.blog/2022/06/02/a-beginners-guide-to-json-the-data-format-for-the-internet/](https://stackoverflow.blog/2022/06/02/a-beginners-guide-to-json-the-data-format-for-the-internet/)



### Rozdział 3: Praca z API przy użyciu Postman (Warsztat praktyczny)

Teoria jest ważna, ale najlepszym sposobem na naukę API jest praktyka. W tym rozdziale przejdziemy od teorii do działania, używając **Postmana** – jednego z najpopularniejszych narzędzi do testowania i pracy z API. Postman pozwala na łatwe wysyłanie zapytań HTTP, analizowanie odpowiedzi i automatyzację testów bez pisania ani jednej linijki kodu.

#### Instalacja i podstawy obsługi Postman

1.  **Instalacja:** Pobierz i zainstaluj aplikację Postman z oficjalnej strony: [https://www.postman.com/downloads/](https://www.postman.com/downloads/). Proces instalacji jest prosty i sprowadza się do kilku kliknięć.

2.  **Podstawy interfejsu:** Po uruchomieniu aplikacji zobaczysz główny interfejs, który składa się z kilku kluczowych obszarów:
    *   **Pasek boczny (Sidebar):** Tutaj znajdują się Twoje kolekcje, historia zapytań i środowiska.
    *   **Główne okno (Builder):** To tutaj tworzysz i wysyłasz zapytania. Możesz wybrać metodę HTTP, wpisać URL, dodać nagłówki, parametry i ciało zapytania.
    *   **Okno odpowiedzi (Response):** Poniżej okna budowniczego wyświetlana jest odpowiedź z serwera, w tym kod statusu, nagłówki i ciało odpowiedzi.

#### Wysyłanie pierwszych zapytań GET i POST

Do naszych ćwiczeń wykorzystamy **JSONPlaceholder** ([https://jsonplaceholder.typicode.com/](https://jsonplaceholder.typicode.com/)), darmowe, publiczne API, które idealnie nadaje się do nauki i testowania.

**Ćwiczenie 1: Wysyłanie zapytania GET**

Naszym celem jest pobranie listy wszystkich postów.

1.  W głównym oknie Postmana, w nowej karcie, upewnij się, że metoda HTTP jest ustawiona na **GET**.
2.  W polu adresu URL wpisz: `https://jsonplaceholder.typicode.com/posts`.
3.  Kliknij przycisk **Send**.
4.  W oknie odpowiedzi powinieneś zobaczyć:
    *   **Status:** `200 OK` (w prawym górnym rogu okna odpowiedzi).
    *   **Body:** Listę 100 postów w formacie JSON, z których każdy ma pola `userId`, `id`, `title` i `body`.

Gratulacje, właśnie wykonałeś swoje pierwsze zapytanie do API!

**Ćwiczenie 2: Wysyłanie zapytania POST**

Teraz spróbujemy utworzyć nowy post.

1.  Zmień metodę HTTP na **POST**.
2.  URL pozostaje ten sam: `https://jsonplaceholder.typicode.com/posts`.
3.  Przejdź do zakładki **Body** pod adresem URL.
4.  Wybierz opcję **raw** i z listy rozwijanej po prawej stronie wybierz **JSON**.
5.  W polu tekstowym wpisz następujący obiekt JSON:

    ```json
    {
      "title": "Mój pierwszy post",
      "body": "To jest treść mojego pierwszego posta w Postmanie.",
      "userId": 1
    }
    ```

6.  Kliknij **Send**.
7.  W oknie odpowiedzi zobaczysz:
    *   **Status:** `201 Created`.
    *   **Body:** Obiekt JSON reprezentujący nowo utworzony post, z dodanym polem `id` (np. `101`).

#### Autoryzacja: API key, Basic Auth, Bearer Token

Większość API wymaga uwierzytelnienia. Postman znacznie upraszcza ten proces.

1.  Przejdź do zakładki **Authorization** (obok zakładki Body).
2.  Z listy **Type** możesz wybrać odpowiednią metodę autoryzacji:
    *   **API Key:** Wybierz tę opcję, a Postman udostępni pola do wpisania klucza (Key) i wartości (Value) oraz określenia, gdzie klucz ma być dodany (Header lub Query Params).
    *   **Basic Auth:** Wystarczy, że podasz nazwę użytkownika (Username) i hasło (Password), a Postman automatycznie zakoduje je w formacie Base64 i doda do nagłówka `Authorization`.
    *   **Bearer Token:** Po wybraniu tej opcji, pojawi się pole, w którym należy wkleić token JWT. Postman sam doda go do nagłówka `Authorization` z prefiksem `Bearer `.

#### Zadanie praktyczne: użycie publicznego API

**Cel:** Użyj publicznego API **OpenWeatherMap**, aby sprawdzić aktualną pogodę dla wybranego miasta.

1.  **Zarejestruj się i zdobądź klucz API:**
    *   Przejdź na stronę [https://openweathermap.org/](https://openweathermap.org/) i załóż darmowe konto.
    *   Po zalogowaniu, w panelu użytkownika znajdź sekcję "API keys" i skopiuj swój domyślny klucz API.

2.  **Znajdź dokumentację:**
    *   W sekcji "API" znajdź dokumentację dla "Current Weather Data".
    *   Odszukaj endpoint do pobierania pogody na podstawie nazwy miasta. Powinien wyglądać mniej więcej tak: `https://api.openweathermap.org/data/2.5/weather`.
    *   Sprawdź, jakich parametrów zapytania (query parameters) wymaga endpoint. Będą to `q` (nazwa miasta) oraz `appid` (Twój klucz API).

3.  **Wyślij zapytanie w Postmanie:**
    *   Utwórz nowe zapytanie **GET**.
    *   Wpisz URL endpointu.
    *   Przejdź do zakładki **Params** (pod adresem URL).
    *   Dodaj dwa parametry:
        *   **Key:** `q`, **Value:** `Warsaw` (lub inne miasto).
        *   **Key:** `appid`, **Value:** `[Twój skopiowany klucz API]`.
    *   Kliknij **Send**.

4.  **Przeanalizuj odpowiedź:**
    *   Sprawdź kod statusu. Czy jest to `200 OK`?
    *   Przejrzyj ciało odpowiedzi w formacie JSON. Jakie informacje o pogodzie możesz odczytać? (np. `temp`, `pressure`, `humidity`, `weather[0].description`).

To ćwiczenie pokazuje pełny cykl pracy z zewnętrznym API: od zdobycia klucza, przez analizę dokumentacji, po wykonanie zapytania i interpretację wyników.

---


## Dzień 2: Budowa i Testowanie Własnego API

### Rozdział 5: Tworzymy proste API (Warsztat praktyczny)

Nadszedł czas, aby przejść od konsumowania API do jego tworzenia. W tym rozdziale zbudujemy od podstaw proste API do zarządzania listą zadań (to-do list) przy użyciu języka **Python** i lekkiego frameworka **Flask**. Nasze API będzie działać lokalnie na Twoim komputerze i pozwoli na dodawanie, pobieranie i usuwanie zadań.

#### Przygotowanie środowiska

1.  **Upewnij się, że masz zainstalowanego Pythona:** Otwórz terminal (lub wiersz poleceń) i wpisz `python --version`. Jeśli nie masz Pythona, pobierz go ze strony [python.org](https://python.org).
2.  **Zainstaluj Flask:** W terminalu wpisz następującą komendę, aby zainstalować framework Flask:

    ```bash
    pip install Flask
    ```

#### Kodowanie od podstaw prostego API w Pythonie

Utwórz nowy plik o nazwie `app.py` i wklej do niego poniższy kod. Każda część kodu zostanie szczegółowo omówiona.

```python
from flask import Flask, request, jsonify

# 1. Inicjalizacja aplikacji Flask
app = Flask(__name__)

# 2. "Baza danych" w pamięci - prosta lista słowników
tasks = [
    {
        'id': 1,
        'title': 'Nauczyć się API',
        'description': 'Przerobić materiały ze szkolenia.',
        'done': False
    },
    {
        'id': 2,
        'title': 'Zbudować własne API',
        'description': 'Stworzyć proste API z listą zadań.',
        'done': False
    }
]

# 3. Endpoint GET do pobierania wszystkich zadań
@app.route('/api/tasks', methods=['GET'])
def get_tasks():
    return jsonify({'tasks': tasks})

# 4. Endpoint POST do dodawania nowego zadania
@app.route('/api/tasks', methods=['POST'])
def create_task():
    if not request.json or not 'title' in request.json:
        return jsonify({'error': 'Missing title'}), 400

    new_task = {
        'id': tasks[-1]['id'] + 1 if tasks else 1,
        'title': request.json['title'],
        'description': request.json.get('description', ""),
        'done': False
    }
    tasks.append(new_task)
    return jsonify({'task': new_task}), 201

# 5. Endpoint DELETE do usuwania zadania
@app.route('/api/tasks/<int:task_id>', methods=['DELETE'])
def delete_task(task_id):
    task = next((task for task in tasks if task['id'] == task_id), None)
    if task is None:
        return jsonify({'error': 'Task not found'}), 404
    
    tasks.remove(task)
    return jsonify({'result': True})

# 6. Uruchamianie lokalnego serwera
if __name__ == '__main__':
    app.run(debug=True, port=5000)

```

**Omówienie kodu:**

1.  **Inicjalizacja:** Tworzymy instancję aplikacji Flask.
2.  **Baza danych w pamięci:** Zamiast konfigurować prawdziwą bazę danych, na potrzeby tego ćwiczenia używamy prostej listy `tasks`, która będzie przechowywać nasze zadania. Dane te znikną po ponownym uruchomieniu serwera.
3.  **Endpoint GET (`/api/tasks`):**
    *   Dekorator `@app.route('/api/tasks', methods=['GET'])` mówi Flaskowi, że funkcja `get_tasks` ma obsługiwać zapytania GET pod tym adresem.
    *   Funkcja `jsonify` konwertuje naszą listę `tasks` na format JSON i zwraca ją jako odpowiedź HTTP z domyślnym statusem `200 OK`.
4.  **Endpoint POST (`/api/tasks`):**
    *   Obsługuje zapytania POST pod tym samym adresem.
    *   `request.json` daje nam dostęp do ciała zapytania w formacie JSON.
    *   Przeprowadzamy prostą **walidację danych**, sprawdzając, czy w zapytaniu znajduje się pole `title`.
    *   Tworzymy nowy słownik `new_task`, nadając mu nowe `id`.
    *   Dodajemy nowe zadanie do naszej listy `tasks`.
    *   Zwracamy nowo utworzone zadanie wraz z kodem statusu `201 Created`.
5.  **Endpoint DELETE (`/api/tasks/<int:task_id>`):**
    *   `<int:task_id>` w URL to **parametr ścieżki**. Flask automatycznie przekaże jego wartość jako argument do funkcji `delete_task`.
    *   Wyszukujemy zadanie o podanym ID. Jeśli go nie znajdziemy, zwracamy błąd `404 Not Found`.
    *   Jeśli zadanie istnieje, usuwamy je z listy i zwracamy potwierdzenie z domyślnym statusem `200 OK`.
6.  **Uruchamianie serwera:** Standardowy blok kodu w Pythonie, który uruchamia serwer deweloperski Flask, gdy plik jest wykonywany bezpośrednio. `debug=True` włącza tryb debugowania, który automatycznie restartuje serwer po każdej zmianie w kodzie.

#### Uruchamianie lokalnego serwera i testowanie

1.  **Uruchom serwer:** W terminalu, w folderze, w którym zapisałeś plik `app.py`, wpisz komendę:

    ```bash
    python app.py
    ```

    Powinieneś zobaczyć komunikat, że serwer działa pod adresem `http://127.0.0.1:5000/`.

2.  **Testowanie w Postmanie:**
    *   **GET all tasks:** Otwórz Postmana i wyślij zapytanie `GET` na adres `http://127.0.0.1:5000/api/tasks`. Powinieneś otrzymać listę dwóch początkowych zadań.
    *   **POST a new task:** Wyślij zapytanie `POST` na ten sam adres, z ciałem JSON (tak jak w poprzednim rozdziale), np.:
        ```json
        { "title": "Przetestować moje API", "description": "Sprawdzić endpoint POST" }
        ```
        Powinieneś otrzymać odpowiedź `201 Created` z nowym zadaniem. Wyślij ponownie zapytanie GET, aby zobaczyć, że zadanie zostało dodane do listy.
    *   **DELETE a task:** Wyślij zapytanie `DELETE` na adres `http://127.0.0.1:5000/api/tasks/2`, aby usunąć zadanie o ID 2. Powinieneś otrzymać potwierdzenie. Sprawdź listę zadań metodą GET, aby upewnić się, że zadanie zniknęło.
    *   **Testowanie błędów:** Spróbuj usunąć zadanie, które nie istnieje (np. o ID 99), aby zobaczyć odpowiedź `404 Not Found`. Spróbuj dodać zadanie bez pola `title`, aby zobaczyć odpowiedź `400 Bad Request`.

Gratulacje! Właśnie zbudowałeś, uruchomiłeś i przetestowałeś swoje pierwsze w pełni funkcjonalne REST API.

---


### Rozdział 6: Testowanie i debugowanie API

Stworzenie działającego API to dopiero połowa sukcesu. Równie ważne jest upewnienie się, że działa ono poprawnie, jest wydajne i odporne na błędy. Proces testowania i debugowania jest nieodłączną częścią cyklu życia każdego oprogramowania, a w przypadku API ma on kluczowe znaczenie dla zapewnienia jego niezawodności.

#### Testowanie manualne i automatyczne

Testowanie API możemy podzielić na dwie główne kategorie:

*   **Testowanie manualne:**
    Polega na ręcznym wysyłaniu zapytań do API i weryfikowaniu odpowiedzi. Dokładnie to robiliśmy w poprzednich rozdziałach przy użyciu Postmana. Testowanie manualne jest świetne na etapie deweloperskim, do szybkiego sprawdzania nowo zaimplementowanych funkcji lub debugowania konkretnych problemów. Jest intuicyjne i nie wymaga skomplikowanej konfiguracji. Jego główną wadą jest jednak to, że jest czasochłonne, powtarzalne i podatne na ludzkie błędy. Trudno jest manualnie przetestować wszystkie możliwe scenariusze i przypadki brzegowe.

*   **Testowanie automatyczne:**
    Polega na napisaniu skryptów, które automatycznie wykonują serię testów i porównują otrzymane wyniki z oczekiwanymi. Testy te mogą być uruchamiane za każdym razem, gdy w kodzie wprowadzana jest zmiana (w ramach procesu Continuous Integration), co gwarantuje, że nowe funkcje nie psują istniejących. Postman oferuje potężne narzędzia do automatyzacji testów za pomocą skryptów pisanych w JavaScript.

    **Przykład testu w Postmanie:**
    W Postmanie, po wysłaniu zapytania, przejdź do zakładki **Tests** w oknie budowniczego. Możesz tam pisać skrypty, które zostaną wykonane po otrzymaniu odpowiedzi. Po prawej stronie znajdują się gotowe fragmenty kodu (Snippets), które ułatwiają pisanie testów.

    Dla naszego zapytania `GET /api/tasks`, możemy dodać następujące testy:

    ```javascript
    // Test 1: Sprawdzenie, czy status odpowiedzi to 200 OK
    pm.test("Status code is 200", function () {
        pm.response.to.have.status(200);
    });

    // Test 2: Sprawdzenie, czy odpowiedź jest w formacie JSON
    pm.test("Response is in JSON format", function () {
        pm.response.to.be.json;
    });

    // Test 3: Sprawdzenie, czy w odpowiedzi znajduje się klucz 'tasks'
    pm.test("Response body contains 'tasks' array", function () {
        const responseData = pm.response.json();
        pm.expect(responseData).to.have.property('tasks');
        pm.expect(responseData.tasks).to.be.an('array');
    });
    ```

    Teraz, za każdym razem, gdy wyślesz to zapytanie, Postman automatycznie uruchomi te testy i pokaże wyniki w zakładce **Test Results** w oknie odpowiedzi.

#### Logi, błędy, debugowanie

Nawet najlepiej przetestowane API może czasem sprawiać problemy. Umiejętność skutecznego debugowania jest kluczowa.

*   **Logi serwera:** Pierwszym miejscem, w którym należy szukać przyczyny problemu, są logi serwera. Gdy uruchamialiśmy naszą aplikację Flask w trybie `debug=True`, każde przychodzące zapytanie było logowane w terminalu, np.:
    `127.0.0.1 - - [05/Oct/2025 10:30:00] "GET /api/tasks HTTP/1.1" 200 -`
    Jeśli w kodzie wystąpi błąd, pełny jego ślad (traceback) zostanie wyświetlony właśnie w terminalu, wskazując dokładne miejsce, w którym wystąpił problem.

*   **Debugowanie w kodzie:** Czasem logi to za mało. W Pythonie można użyć wbudowanego debuggera. Wystarczy w miejscu, w którym chcemy zatrzymać wykonanie kodu, dodać linię:
    ```python
    import pdb; pdb.set_trace()
    ```
    Gdy serwer dotrze do tej linii, wykonanie programu zatrzyma się, a w terminalu pojawi się interaktywna konsola debuggera, w której można sprawdzać wartości zmiennych i wykonywać kod krok po kroku.

*   **Postman Console:** Postman również ma własną konsolę (dostępna w lewym dolnym rogu okna), która pokazuje szczegółowe informacje o każdym wysłanym zapytaniu i otrzymanej odpowiedzi, włącznie z pełnymi nagłówkami i surowym ciałem. Jest to nieocenione narzędzie do diagnozowania problemów z komunikacją.

#### Wprowadzenie do testów jednostkowych API

Testy automatyczne w Postmanie są świetne do testowania API "z zewnątrz" (testy end-to-end). Jednak profesjonalne podejście wymaga również pisania **testów jednostkowych** (unit tests), które sprawdzają poszczególne fragmenty kodu (funkcje, metody) w izolacji.

W Pythonie do pisania testów jednostkowych najczęściej używa się wbudowanej biblioteki `unittest` lub bardziej rozbudowanego frameworka `pytest`. Flask dobrze integruje się z oboma.

Testy jednostkowe dla API polegają na symulowaniu zapytań HTTP do aplikacji bez uruchamiania prawdziwego serwera i sprawdzaniu, czy funkcje obsługujące endpointy zwracają poprawne dane i kody statusu.

**Przykład testu jednostkowego dla naszego API (używając `pytest`):**

```python
# plik test_app.py
import pytest
from app import app # importujemy naszą aplikację

@pytest.fixture
def client():
    app.config["TESTING"] = True
    with app.test_client() as client:
        yield client

def test_get_tasks(client):
    """Testuje pobieranie wszystkich zadań."""
    response = client.get("/api/tasks")
    assert response.status_code == 200
    assert b"Nauczy\u0107 si\u0119 API" in response.data # Sprawdzenie czy w odpowiedzi jest tekst
```

Chociaż pełne omówienie testów jednostkowych wykracza poza ramy tego wprowadzenia, warto pamiętać, że są one fundamentem niezawodnego i łatwego w utrzymaniu oprogramowania. Automatyzacja testów na różnych poziomach (jednostkowe, integracyjne, end-to-end) jest kluczową praktyką w nowoczesnym wytwarzaniu oprogramowania.

---


## Dzień 3: Dobre Praktyki, Dokumentacja i Projekt Końcowy

### Rozdział 10: Projekt końcowy

Nadszedł czas, aby połączyć w całość całą zdobytą wiedzę. Projekt końcowy ma na celu samodzielne zaprojektowanie, zbudowanie, przetestowanie i udokumentowanie prostego REST API od początku do końca. To ćwiczenie pozwoli Ci ugruntować umiejętności i zmierzyć się z realnymi wyzwaniami, jakie stoją przed deweloperem API.

#### Mini projekt: stworzenie API + dokumentacja + testy

**Temat:** Proste API do zarządzania kolekcją książek.

**Wymagania funkcjonalne:**

Twoim zadaniem jest stworzenie API, które będzie zarządzać kolekcją książek. Każda książka powinna mieć następujące atrybuty:
*   `id` (unikalny identyfikator, liczba całkowita)
*   `title` (tytuł, string)
*   `author` (autor, string)
*   `year` (rok wydania, liczba całkowita)
*   `read` (status przeczytania, boolean)

API powinno udostępniać następujące endpointy:

1.  **`GET /api/books`**
    *   Zwraca listę wszystkich książek w kolekcji.

2.  **`GET /api/books/{book_id}`**
    *   Zwraca szczegóły pojedynczej książki o podanym ID.
    *   Powinno obsłużyć przypadek, gdy książka o danym ID nie istnieje (zwracając status `404 Not Found`).

3.  **`POST /api/books`**
    *   Dodaje nową książkę do kolekcji.
    *   Ciało zapytania powinno zawierać `title`, `author` i `year`.
    *   Powinno przeprowadzić walidację (sprawdzić, czy wszystkie wymagane pola istnieją).
    *   Powinno zwrócić nowo utworzony obiekt książki wraz ze statusem `201 Created`.

4.  **`PUT /api/books/{book_id}`**
    *   Aktualizuje status przeczytania książki (`read`).
    *   Ciało zapytania powinno zawierać tylko jedno pole: `"read": true` lub `"read": false`.
    *   Powinno zwrócić zaktualizowany obiekt książki.

5.  **`DELETE /api/books/{book_id}`**
    *   Usuwa książkę o podanym ID z kolekcji.

**Wymagania techniczne:**

1.  **Implementacja:** Użyj języka Python i frameworka Flask, opierając się na kodzie z Rozdziału 5. Przechowuj dane w pamięci (w prostej liście), tak jak w poprzednim ćwiczeniu.

2.  **Testowanie:**
    *   Utwórz kolekcję w Postmanie o nazwie "API Książek - Projekt Końcowy".
    *   Dla każdego endpointu stwórz odpowiednie zapytanie (GET, POST, PUT, DELETE).
    *   Dla każdego zapytania napisz co najmniej dwa testy automatyczne w zakładce "Tests" w Postmanie (np. test na status `200 OK`, test sprawdzający zawartość odpowiedzi).
    *   Przetestuj również scenariusze błędów (np. próba pobrania nieistniejącej książki, dodanie książki bez tytułu).

3.  **Dokumentacja:**
    *   Napisz prostą dokumentację w formacie Markdown (w osobnym pliku `README.md`).
    *   Dla każdego endpointu opisz: metodę HTTP, URL, działanie, wymagane parametry/ciało zapytania oraz możliwe kody odpowiedzi wraz z przykładem odpowiedzi JSON.
    *   **Zadanie dodatkowe (dla chętnych):** Spróbuj opisać swoje API przy użyciu specyfikacji OpenAPI (w formacie YAML). Możesz użyć edytora online, takiego jak [Swagger Editor](https://editor.swagger.io/), aby Ci w tym pomógł.

#### Prezentacja i omówienie rozwiązań uczestników

Ostatnim etapem szkolenia jest prezentacja swojego projektu. Przygotuj krótkie, 5-minutowe demo, podczas którego:

1.  Pokaż działanie swojego API za pomocą kolekcji w Postmanie – zaprezentuj, jak dodajesz, pobierasz, aktualizujesz i usuwasz książki.
2.  Pokaż wyniki swoich testów automatycznych w Postmanie.
3.  Omów krótko stworzoną przez siebie dokumentację.

Celem prezentacji jest nie tylko pokazanie gotowego rozwiązania, ale także podzielenie się doświadczeniami, napotkanymi problemami i sposobami ich rozwiązania. Wspólne omówienie projektów jest doskonałą okazją do nauki i wymiany wiedzy.

Powodzenia!


