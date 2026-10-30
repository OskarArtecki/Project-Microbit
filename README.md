## Micro:bit - Projekt gry
Głównym celem projektu jest stworzenie gry zręcznościowej w mikrokontrolerze Micro:bit, oraz wdrożenie i przetestowanie systemu rejestracji i prezentacji wyników gracza w systemie rankingowym dostępnych przez Internet jak i na urządzeniu fizycznym.
## Cele projektu + Technologie
* **Integracja sprzętu z siecią:** Stworzenie stabilnego mechanizmu transmisji danych (z wykorzystaniem UART, Arduino lub innego bridge'a), który umożliwia przesłanie wyniku i identyfikatora gracza z fizycznego urządzenia do środowiska backendowego.
* **Budowa bezpiecznego REST API:** Stworzenie backendu (Python / FastAPI) odpowiedzialnego za przyjmowanie, walidację oraz przetwarzanie danych napływających z kontrolera.
* **Trwałość danych (Persystencja):** Zaimplementowanie relacyjnej bazy danych (PostgreSQL) w celu trwałego przechowywania profili graczy i unikalnych wyników.
* Stworzenie **interfejsu webowego (HTML/CSS/JS)** umożliwiającego przeglądanie aktualnego rankingu z poziomu przeglądarki internetowej.
* Zbudowanie **fizycznego modułu wyświetlającego** (micro:bit / Arduino z ekranem), który pobiera z API i prezentuje na żywo zestawienie najlepszych graczy.
* **Łatwe uruchamianie (Docker):** Użycie programu Docker (i Docker Compose), aby cały projekt — baza danych i serwer — dało się włączyć jednym poleceniem na dowolnym komputerze, bez konieczności ręcznej instalacji i konfiguracji dodatkowego oprogramowania.
## Przepływ danych
```text
[ Micro:bit (Gra) ] ──(UART/Kabel)──> [ Pośrednik / Bridge ]
                                             │
                                         (HTTP POST)
                                             │
                                             ▼
                                    [ Serwer API (FastAPI) ]
                                             │
                                      (Zapis / Odczyt)
                                             │
                                             ▼
                                  [ Baza Danych (PostgreSQL) ]
                                             │
                       ┌─────────────────────┴─────────────────────┐
                       │ (HTTP GET)                                │ (HTTP GET)
                       ▼                                           ▼
            [ Strona WWW (Ranking) ]                    [ Fizyczny Ekran Rankingu ]
```
## Backend i Baza danych
Sercem proejktu jest aplikacja API zbudowana w języku **Python** przy użyciu frameworka **FastAPI**. Serwer udostępnia dedykowane endpointy RESTful służące do komunikacji ze sprzętem oraz aplikacjami klienckimi:

| `POST` | `/api/v1/scores/submit` | Rejestracja nowego wyniku uzyskanego podczas rozgrywki |

| `GET` | `/api/v1/rankings/top` | Pobranie aktualnej tabeli liderów |

| `GET` | `/api/v1/players/{player_id}/stats` | Pobranie historii wyników oraz statystyk konkretnego gracza |
