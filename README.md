# Mój Mały Wszechświat

Przeglądarkowa gra o zbieraniu surowców i odkrywaniu kolejnych planet. Gracz rozbudowuje dostępny teren, ulepsza narzędzia i pokonuje przeciwników, aby odblokować portal do następnego świata. Projekt powstał jako zabawa inspirowana *My Little Universe*, zgodnie z opisem repozytorium.

Całość mieści się w jednym pliku [`index.html`](index.html): HTML, CSS, logika JavaScript, grafika Canvas 2D i dźwięki Web Audio. Nie ma bibliotek zewnętrznych ani procesu budowania.

## Co można zobaczyć

- Pięć planet o różnych zasobach i parametrach.
- Drewno, kamień, złoto i kryształy; zakup sąsiednich kafli za surowce.
- Automatyczne dobieranie narzędzia, zbieranie surowców i walka po podejściu do celu.
- Ulepszanie siekiery, kilofa i miecza, przeciwnicy oraz bossowie.
- Trzy poziomy trudności, podpowiedzi i strzałka prowadząca do celu.
- Klawiatura oraz joystick ekranowy dla urządzeń dotykowych.
- Lokalny zapis postępu i przełącznik dźwięku.

## Uruchomienie lokalne

Wymagania: współczesna przeglądarka z Canvas 2D, Web Audio i `localStorage`. Do poniższej metody podglądu potrzebny jest też Python 3.

```sh
git clone https://github.com/dzwoniar/my-max-universe.git
cd my-max-universe
python -m http.server 8000 --bind 127.0.0.1
```

Otwórz [http://127.0.0.1:8000](http://127.0.0.1:8000) i kliknij **Graj!**. Na systemach, w których Python jest dostępny jako `python3`, użyj tej nazwy polecenia. Serwer zatrzymasz przez `Ctrl+C`.

Można też otworzyć `index.html` bezpośrednio, ale serwer lokalny daje stały adres dla zapisu przeglądarki. Gra nie wymaga konta, kluczy API ani połączenia z zewnętrznymi serwisami.

## Sterowanie

| Wejście | Działanie |
| --- | --- |
| `WASD` lub strzałki | Poruszanie postacią |
| Joystick ekranowy | Ruch na ekranie dotykowym |
| Podejście do złoża / wroga | Automatyczne zbieranie / walka |
| Spacja lub przycisk odblokowania | Zakup pobliskiego kafla, jeśli wystarcza zasobów |
| Przycisk sklepu | Ulepszanie narzędzi |
| Przycisk dźwięku | Wyciszenie / włączenie efektów |

## Architektura jednego pliku

Kod jest podzielony komentarzami na sekcje. Najważniejsze punkty wejścia w [`index.html`](index.html):

| Sekcja / funkcja | Rola |
| --- | --- |
| `USTAWIENIA` | Balans gry, narzędzia, zasoby, przeciwnicy i definicje planet |
| `zbudujPlanete`, `losowarka` | Generowanie kafli i deterministyczne rozmieszczenie złóż |
| `wejscie` | Klawiatura i joystick |
| `rysuj`, `aktualizujKamere` | Canvas 2D, kamera i efekty |
| `aktualizujGracza`, `automatycznaAkcja` | Ruch, kolizje, wydobycie i walka |
| `stan`, `zapisz`, `wczytaj` | Bieżąca rozgrywka oraz zapis w przeglądarce |
| `start`, `klatka` | Inicjalizacja i pętla `requestAnimationFrame` |

Parametry rozgrywki zmienia się w `USTAWIENIA`, bez instalowania narzędzi deweloperskich.

## Zapis i ograniczenia

Postęp trafia do `localStorage`: numer planety, zasoby, poziomy narzędzi, odblokowane kafle, trudność i ustawienie dźwięku. Stan świata jest odtwarzany; zapis nie jest pełną migawką każdej pozycji i każdego przeciwnika.

Zapis jest przypisany do przeglądarki i adresu strony. Nie synchronizuje się między urządzeniami; wyczyszczenie danych strony lub opcja rozpoczęcia od nowa usuwa postęp. Gdy przeglądarka blokuje `localStorage`, gra może działać bez trwałego zapisu. Dźwięk uruchamia się po interakcji użytkownika.

Nie ma backendu, multiplayera ani automatycznych testów. Ręczny przegląd obejmuje ruch klawiaturą i dotykiem, zakup kafla, ulepszenie narzędzia, walkę, portal oraz odtworzenie postępu po odświeżeniu pod tym samym adresem.
