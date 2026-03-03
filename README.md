# pracuj_pl_n8n

Scraper i system filtrujący oferty pracy z Pracuj.pl zbudowany w n8n. Automatycznie obsługuje paginację, filtruje oferty dla dwóch niezależnych profili, weryfikuje duplikaty na podstawie Google Sheets i zapisuje nowe wyniki. Konfiguracja jest w pełni zanonimizowana.

## 🚀 Główne funkcje

* **Zaawansowane filtrowanie (JavaScript):** Niestandardowy skrypt oceniający tytuły, opisy i podsumowania AI pod kątem specyficznych słów kluczowych (premiowanych) oraz wykluczeń (blacklist).
* **Ochrona przed duplikatami:** Dynamiczne sprawdzanie bazy Google Sheets zapobiega powtórnemu pobieraniu i analizowaniu tych samych linków.
* **Omijanie blokad:** Opóźnienia i obsługa błędów minimalizujące ryzyko blokady przez systemy typu Cloudflare.
* **Centralna konfiguracja:** Wszystkie zmienne środowiskowe (ID arkuszy, etykiety profili) znajdują się w jednym węźle startowym, co eliminuje potrzebę edycji kodu.

## 🧠 Logika filtrowania (SSOT)

System klasyfikuje oferty na podstawie wbudowanych słowników. 

| Marka | Zasada |
| :--- | :--- |
| **Profil A** | Szuka stanowisk z obszaru Analizy Danych, PIM (Akeneo, Pimcore, Master Data) oraz Automatyzacji (n8n, Zapier). Rygorystycznie wyklucza programistów, testerów i księgowość. |
| **Profil B** | Premiuje oferty z wymogiem języka niderlandzkiego oraz role w obszarze Process Excellence (Lean, Workflow, Optymalizacja). Wyklucza stanowiska juniorskie i obsługę klienta (call center). |

## 🛠️ Instalacja i konfiguracja

1. Pobierz plik `.json` z tego repozytorium.
2. Zaimportuj go do swojego panelu n8n.
3. Skonfiguruj poświadczenia (**Credentials**) dla węzłów Google Sheets (wymagane Google OAuth2 API).
4. Otwórz pierwszy węzeł w workflow (**Konfiguracja**) i uzupełnij zmienne:
   * `sheetId`: Główne ID Twojego arkusza Google.
   * `gid_User_A` / `gid_User_B`: Numery ID poszczególnych zakładek (tabów) dla Profilu A i B.
   * `labelA` / `labelB`: Docelowe nazwy profili (np. "Analityk", "Manager").
5. Zmień węzeł wyzwalający (Trigger) z ręcznego na harmonogram (np. Cron), jeśli chcesz, by proces działał automatycznie w tle.

## ⚠️ Zastrzeżenie

Ten workflow został stworzony wyłącznie do celów edukacyjnych i optymalizacji własnego procesu poszukiwania pracy. Pamiętaj, aby dostosować interwały pobierania danych (węzeł Wait), tak aby nie obciążać serwerów docelowych portalu.
