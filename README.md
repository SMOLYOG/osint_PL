# OSINT (Biały Wywiad) – Przewodnik Podstawowy

## Cel

Ten przewodnik wprowadza w metodologię OSINT (Open Source Intelligence – wywiad ze źródeł jawnych): jak pracować, jakich narzędzi używać i jak zbudować systematyczny proces śledczy.

## Kiedy się przyda

- Gdy zaczynasz pracę z OSINT i potrzebujesz uporządkowanej metodologii
- Gdy prowadzisz podstawowe śledztwo dotyczące domeny lub osoby
- Gdy uczysz się narzędzi i procesów OSINT w celach zawodowych

---

## Spis treści

1. [Wprowadzenie do OSINT](#wprowadzenie-do-osint)
2. [Metodologia i cykl OSINT](#metodologia-i-cykl-osint)
3. [Podstawowe narzędzia](#podstawowe-narzędzia)
4. [OSINT według kategorii](#osint-według-kategorii)
5. [Konfiguracja maszyny do OSINT](#konfiguracja-maszyny-do-osint)
6. [Procedury śledcze według typu identyfikatora](#procedury-śledcze-według-typu-identyfikatora)
7. [Przykładowe procesy śledcze](#przykładowe-procesy-śledcze)
8. [Zabezpieczanie dowodów](#zabezpieczanie-dowodów)
9. [Zgłaszanie nadużyć](#zgłaszanie-nadużyć)
10. [Dobre praktyki i OPSEC](#dobre-praktyki-i-opsec)
11. [Aspekty prawne i etyczne](#aspekty-prawne-i-etyczne)
12. [Materiały do dalszej nauki](#materiały-do-dalszej-nauki)

---

## Wprowadzenie do OSINT

**OSINT (biały wywiad)** to zbieranie i analiza informacji pochodzących z ogólnodostępnych źródeł. Wykorzystuje się go m.in. w:

- **Cyberbezpieczeństwie** – analiza zagrożeń, mapowanie powierzchni ataku
- **Organach ścigania** – śledztwa, poszukiwania osób zaginionych
- **Bezpieczeństwie korporacyjnym** – analiza konkurencji, due diligence
- **Dziennikarstwie śledczym** – weryfikacja faktów
- **Bezpieczeństwie osobistym** – sprawdzanie reputacji

### Co czyni wywiad „jawnym"?

- Dane **publicznie dostępne** – z internetu, mediów społecznościowych, rejestrów publicznych
- Pozyskane **legalnie** – bez włamań, nieautoryzowanego dostępu czy socjotechniki
- Zbierane **etycznie** – z poszanowaniem prawa i regulaminów serwisów

---

## Metodologia i cykl OSINT

### Cykl pracy

1. **Określenie celów** – jakie pytania wywiadowcze chcemy rozstrzygnąć
2. **Identyfikacja źródeł** – gdzie szukać danych
3. **Zbieranie danych** – pozyskiwanie informacji
4. **Przetwarzanie** – porządkowanie i filtrowanie
5. **Analiza** – łączenie faktów, szukanie wzorców
6. **Prezentacja wyników** – w formie użytecznej dla odbiorcy
7. **Informacja zwrotna** – korekta podejścia na podstawie efektów

### Struktura OSINT Framework

Śledztwa dzieli się zwykle według typu identyfikatora:

- **Nazwa użytkownika** – obecność w social media, konta online
- **Adres e-mail** – wyliczanie kont, dane z wycieków
- **Domena** – rekordy DNS, WHOIS, informacje o stronie
- **Adres IP** – geolokalizacja, informacje sieciowe
- **Numer telefonu** – operator, powiązania z kontami
- **Osoba** – rejestry publiczne, profile społecznościowe
- **Firma** – rejestry, pracownicy, infrastruktura
- **Kryptowaluty** – śledzenie portfeli, bazy oszustw

---

## Podstawowe narzędzia

Poniżej najważniejsze narzędzia pogrupowane wg zastosowania (podstawowe komendy podane orientacyjnie – pełną składnię sprawdzaj w dokumentacji projektu).

### E-mail i nazwa użytkownika

| Narzędzie | Zastosowanie | Link |
|---|---|---|
| **theHarvester** | zbieranie e-maili, subdomen, nazwisk z wielu źródeł (Google, Bing, LinkedIn) | github.com/laramies/theHarvester |
| **Sherlock** | wyszukiwanie nazwy użytkownika w 400+ serwisach | github.com/sherlock-project/sherlock |
| **Maigret** | bardziej rozbudowana wersja Sherlocka, 2500+ serwisów, eksport do PDF/HTML | github.com/soxoj/maigret |
| **Blackbird** | dodatkowe platformy nieobjęte przez Sherlocka/Maigreta | github.com/p1ngul1n0/blackbird |
| **H8mail** | wyszukiwanie e-maila w bazach wycieków haseł | github.com/khast3x/h8mail |
| **Holehe** | sprawdza, na jakich serwisach zarejestrowany jest dany e-mail | github.com/megadose/holehe |

Przykład (theHarvester):
```bash
theHarvester -d target.com -l 500 -b all
```

### Numer telefonu

**PhoneInfoga** – sprawdzanie operatora, poprawności numeru, powiązań z kontami społecznościowymi.
```bash
phoneinfoga scan -n +1234567890
```
github.com/sundowndev/phoneinfoga

### Rozpoznanie domen i sieci

| Narzędzie | Zastosowanie |
|---|---|
| **Recon-ng** | rozbudowany framework do automatyzacji rekonesansu |
| **Amass** (OWASP) | mapowanie sieci, wyliczanie subdomen, ponad 50 źródeł danych |
| **Subfinder** | szybkie wyliczanie subdomen |
| **httpx** | sprawdzanie żywych hostów i identyfikacja stosu technologicznego |
| **waybackurls** | pobiera historyczne adresy URL znane Wayback Machine |
| **asn** (CLI) | szybkie sprawdzenie ASN, zakresu sieci i kontaktu do zgłaszania nadużyć |

Typowy potok pracy:
```bash
subfinder -d target.com -silent | httpx -td -server -title -asn
```

### Crawlery i analiza stron

- **Photon** – bardzo szybki crawler wydobywający adresy URL, pliki JS, e-maile
- **SpiderFoot** – zautomatyzowany OSINT ze 100+ modułami (DNS, e-maile, social media, dark web); dostępny w wersji GUI i CLI

### Zabezpieczanie dowodów (archiwizacja)

- **Monolith** – zapisuje stronę jako jeden samodzielny plik HTML (dowód sądowej jakości)
- **CutyCapt** – zrzuty ekranu z linii komend
- **WaybackPy** – programowe zgłaszanie stron do Wayback Machine

### Analiza powiązań

**Maltego** – wizualna analiza powiązań (link analysis), setki integracji danych (DNS, WHOIS, social media, rejestry publiczne). Wersje: Community (darmowa), Classic, XL. maltego.com

### Inne przydatne narzędzia

- **Metagoofil** – wydobywa metadane z publicznych dokumentów (autorzy, wersje oprogramowania, ścieżki wewnętrzne)
- **WhatWeb** – identyfikuje technologie użyte na stronie (CMS, frameworki)

---

## OSINT według kategorii

### Wyszukiwarki i dorking

Przykłady Google dorków:
```
site:target.com filetype:pdf
site:target.com inurl:admin
site:target.com intitle:"index of"
"@target.com" site:pastebin.com
```

Specjalistyczne wyszukiwarki:
- **Shodan** – wyszukiwarka urządzeń podłączonych do internetu
- **Censys** – skanowanie i analiza internetu
- **GreyNoise** – analiza „szumu tła" internetu
- **Hunter.io** – wyszukiwanie i weryfikacja e-maili
- **Have I Been Pwned** – powiadomienia o wyciekach danych
- **CriminalIP, Netlas, FullHunt, ZoomEye** – alternatywy dla Shodan/Censys
- **AbuseIPDB** – baza zgłoszeń nadużyć adresów IP
- **SecurityTrails, DNSDumpster** – historia DNS i rekonesans
- **URLScan.io** – skanowanie stron ze zrzutami ekranu
- **crt.sh** – wyszukiwanie w logach Certificate Transparency

```bash
curl -s "https://crt.sh/?q=%25.target.com&output=json" | jq '.[].name_value' | sort -u
```

### OSINT w mediach społecznościowych

Techniki: korelacja nazw użytkowników między platformami, wyszukiwanie zdjęcia profilowego po obrazie, mapowanie powiązań, wyciąganie metadanych z postów, geolokalizacja na podstawie zdjęć.

### Geolokalizacja i analiza obrazu

Narzędzia: Google Earth Pro (zdjęcia historyczne), Google Street View, Yandex Maps (silny w Rosji/Europie Wsch.), Baidu Maps (Chiny), ekstraktory danych EXIF.

Techniki: odwrotne wyszukiwanie obrazem (Google, Yandex, TinEye), analiza cieni, identyfikacja punktów orientacyjnych, wyciąganie współrzędnych GPS z EXIF.

### Dark web / deep web

Dostęp: Tor Browser, I2P, Tails OS (system live nastawiony na prywatność).
Wyszukiwanie: Ahmia.fi, DarkSearch.io, OnionLand Search. Do dokumentowania śledztwa w przeglądarce przydaje się np. Hunchly.

### Wywiad biznesowy

Źródła: rejestry firm, SEC EDGAR (spółki publiczne w USA), LinkedIn (pracownicy), Glassdoor/Indeed (opinie, zarobki), Crunchbase (startupy, inwestycje), ZoomInfo/RocketReach (dane kontaktowe).

### Rejestry publiczne (USA – przykład)

PACER (akta sądów federalnych), rejestry sądów stanowych, ewidencje nieruchomości, rejestry spółek (Secretary of State), licencje zawodowe.

### Dane z wycieków

Have I Been Pwned, DeHashed (płatne), LeakPeek, Snusbase, IntelX, Leak-Lookup, monitoring serwisów typu Pastebin.

### Kryptowaluty

Bazy zgłoszeń oszustw: BitcoinAbuse, ChainAbuse, CryptoScamDB.
Eksploratory blockchain: blockchain.info (Bitcoin), Etherscan (Ethereum), BlockCypher, narzędzia do klastrowania: OXT, Breadcrumbs.

---

## Konfiguracja maszyny do OSINT

### Gotowe dystrybucje

- **Buscador** (Michael Bazzell) – **wycofana**, niedostępna; jako alternatywę rozważ Trace Labs OSINT VM lub Tsurugi Linux
- **Trace Labs OSINT VM** – nastawiona na poszukiwania osób zaginionych; tracelabs.org
- **CSI Linux** – forensyka cyfrowa i OSINT; csilinux.com
- **Tsurugi Linux** – DFIR (informatyka śledcza) z pełnym zestawem narzędzi OSINT; tsurugi-linux.org

### Budowa własnej maszyny (skrót)

```bash
# System bazowy
sudo apt update && sudo apt upgrade -y
sudo apt install -y python3 python3-pip git curl wget build-essential

# Przykładowe instalacje narzędzi
git clone https://github.com/laramies/theHarvester && cd theHarvester && pip3 install -r requirements.txt
pip3 install maigret h8mail holehe waybackpy
go install -v github.com/owasp-amass/amass/v4/...@latest
```

Do tego warto doinstalować: nmap, masscan, whois, dnsutils, exiftool, ffmpeg, cutycapt, metagoofil.

---

## Procedury śledcze według typu identyfikatora

### E-mail

1. **Sprawdzenie kont**: `holehe target@email.com --only-used`
2. **Bazy wycieków**: `h8mail -t target@email.com -o breach_results.csv`, ewentualnie API HIBP
3. **Weryfikacja e-maila** (np. Hunter.io API)
4. **Analiza nagłówków** – jeśli masz oryginalną wiadomość: sprawdź łańcuch `Received:`, adres IP nadawcy, zgodność SPF/DKIM/DMARC, ślady spoofingu
5. **Wyodrębnienie domeny** – i przejście do procedury dla domeny

### Adres IP

1. **ASN i kontakt do zgłoszeń nadużyć**: `asn 1.2.3.4`
2. **Geolokalizacja**: np. `curl -s "https://ipinfo.io/1.2.3.4" | jq`
3. **Reputacja**: AbuseIPDB, Shodan
4. **Reverse DNS i porty**: `dig -x 1.2.3.4 +short`, `nmap -F -T4 --open 1.2.3.4`

Warto udokumentować: ASN, e-mail do zgłoszeń, dostawcę hostingu, otwarte usługi.

### Domena

1. **WHOIS** – zwłaszcza kontakt abuse rejestratora (kluczowy przy żądaniach usunięcia)
2. **Rekordy DNS**: A, MX, TXT, NS (`dig +short domena TYP`)
3. **Subdomeny + sprawdzenie żywych hostów**: `subfinder` → `httpx`
4. **Analiza historyczna**: `waybackurls`, wyszukiwanie w crt.sh
5. **Stos technologiczny**: `whatweb -a 3 https://domena`

### Kryptowaluty

**Formaty adresów**: Bitcoin (zaczyna się od `1`, `3` lub `bc1`), Ethereum (`0x` + 40 znaków hex), Litecoin (`L`, `M`, `3`), Monero (`4`/`8`, 95 znaków), Solana (Base58, ~44 znaki).

Kroki: sprawdzenie w bazach oszustw (BitcoinAbuse, ChainAbuse, CryptoScamDB) → analiza w eksploratorze blockchain (suma wpłat/wypłat, liczba transakcji, daty pierwszej/ostatniej transakcji) → śledzenie transakcji (źródło finansowania, wpłaty na giełdy, klastrowanie adresów) → dokumentacja (adres, typ blockchaina, kwoty, zrzuty ekranu z sygnaturami czasu).

Uwaga API Etherscan: wersja v1 API została wycofana, obecnie wymagana jest wersja v2 z parametrem `chainid` (1 = Ethereum mainnet).

### Numer telefonu

1. Walidacja i operator: `phoneinfoga scan -n "+15551234567"`
2. Odpowiednik programistyczny – biblioteka `phonenumbers` w Pythonie (carrier, geocoder)
3. Wykrywanie VoIP (Google Voice, TextNow, aplikacje typu „burner", numery oparte o Twilio) – często wykorzystywane przez oszustów
4. Sprawdzenie w mediach społecznościowych i usługach typu reverse-lookup (np. Truecaller)

### Nazwa użytkownika

1. Szeroki zasięg: `maigret nazwa --pdf --html -o ./wyniki/`
2. Szybka weryfikacja: `sherlock nazwa --csv`
3. Dodatkowe pokrycie: `blackbird -u nazwa`
4. Ręczna weryfikacja każdego znalezionego profilu: opis/bio, zdjęcie (reverse image search), historia postów, powiązania (znajomi, grupy), aktywność (częstotliwość, strefa czasowa, język)

---

## Przykładowe procesy śledcze

### 1. Śledztwo dotyczące osoby

Znane dane (imię, e-mail, nazwa użytkownika, lokalizacja) → wyliczanie nazw użytkownika (Sherlock/Maigret/Blackbird) → śledztwo e-mailowe (Holehe, H8mail, theHarvester) → analiza social media (profile, powiązania, oś czasu) → numer telefonu (PhoneInfoga) → geolokalizacja (EXIF, check-iny, mapy) → złożenie osi czasu → raport z wizualizacją powiązań (np. Maltego)

### 2. Śledztwo dotyczące domeny/firmy

Rekonesans wstępny (WHOIS, DNS) → wyliczanie subdomen (Amass, Subfinder, crt.sh) → sprawdzenie żywych hostów (httpx, WhatWeb) → zbieranie e-maili (theHarvester, LinkedIn) → analiza aplikacji webowej (Photon, waybackurls) → wyliczanie pracowników → mapowanie infrastruktury (Shodan/Censys) → przegląd danych z wycieków → raport i wizualizacja

### 3. Zbieranie danych do analizy zagrożeń (threat intelligence)

Zbieranie wskaźników kompromitacji (IOC) → analiza pasywnego DNS → badanie malware (sandboxy, VirusTotal) → atrybucja aktora zagrożenia → analiza podatności (CVE, Exploit-DB) → korelacja infrastruktury (wspólny hosting, certyfikaty SSL) → raportowanie (formaty MISP/STIX)

### 4. Śledztwo dot. oszustwa/scamu

1. **Wstępna kwalifikacja** – dokumentacja zgłoszenia, weryfikacja uprawnień, nadanie ID sprawy (format `RRRR-NNN-TYP`)
2. **Zebranie identyfikatorów** – e-maile, telefony, domeny, adresy krypto, konta społecznościowe
3. **Równoległe śledztwo** dla każdego identyfikatora (odpowiednimi narzędziami z sekcji wyżej)
4. **Korelacja infrastruktury** – wspólny hosting, certyfikaty, WHOIS, powtarzające się szablony
5. **Zabezpieczenie dowodów** – archiwizacja stron, zrzuty ekranu, sumy kontrolne SHA256, zgłoszenie do Wayback Machine
6. **Raport** – podsumowanie, ustalenia per identyfikator, spis dowodów, kontakty abuse
7. **Zgłoszenie nadużycia** – do rejestratora, hostingu, platform, ew. IC3
8. **Kontrola końcowa** – śledzenie odpowiedzi, weryfikacja usunięcia treści, archiwizacja sprawy

---

## Zabezpieczanie dowodów

### Złote zasady

1. **Licz sumy kontrolne od razu** – SHA256 w momencie zbierania
2. **Znakuj czasem (UTC)** – wszystkie dowody
3. **Prowadź łańcuch dowodowy (chain of custody)** – kto, co i kiedy zebrał
4. **Kopie** – oryginał + kopia robocza w osobnych miejscach
5. **Weryfikuj integralność** – okresowe ponowne liczenie sum kontrolnych

### Procedura archiwizacji strony

```bash
# 1. Samodzielny plik HTML (zachowuje layout, obrazy, CSS)
monolith https://strona.com -o dowod_$(date +%Y%m%d_%H%M%S).html

# 2. Pełny zrzut ekranu
cutycapt --url=https://strona.com --out=zrzut_$(date +%Y%m%d_%H%M%S).png

# 3. Zgłoszenie do Wayback Machine (niezależny świadek)
waybackpy --url "https://strona.com" --save

# 4. Manifest sum kontrolnych
sha256sum dowod_*.html zrzut_*.png > sumy_kontrolne_$(date +%Y%m%d).txt
```

### Sugerowana struktura sprawy

```
~/Sprawy_OSINT/
└── ID-SPRAWY/
    ├── info_sprawy.md        # metadane, cele, autoryzacja
    ├── notatki.md            # notatki śledcze, oś czasu
    ├── log_dowodowy.md       # łańcuch dowodowy
    ├── dowody/
    │   ├── zrzuty_ekranu/
    │   ├── archiwa/
    │   ├── pliki/
    │   └── sumy_kontrolne/
    ├── raporty/
    └── dane_surowe/
```

Format ID sprawy: `RRRR-NNN-TYP`, np. `2026-001-PHISHING`.

---

## Zgłaszanie nadużyć

Gdy śledztwo wykryje wrogą infrastrukturę, celem staje się jej **zablokowanie**.

### Krok 1: Ustal odpowiednie strony

- Abuse rejestratora domeny (z WHOIS)
- Abuse dostawcy hostingu (z ASN)
- Adres abuse dostawcy poczty
- Kontakt do zaufania i bezpieczeństwa danej platformy społecznościowej
- Procesor płatności, jeśli w grę wchodzą pieniądze (Stripe, PayPal, giełda krypto)

### Krok 2: Przygotuj zgłoszenie

Powinno zawierać: rodzaj nadużycia, konkretne adresy URL/IP/domeny, oś czasu (UTC), odniesienia do dowodów (bez załączania – tylko odwołanie do manifestu sum kontrolnych), dane kontaktowe, oświadczenie o dobrej wierze.

### Krok 3: Kanały zgłoszeń

| Odbiorca | Kanał |
|---|---|
| Rejestrator domeny | portal abuse rejestratora lub `abuse@rejestrator.tld` |
| Dostawca hostingu | `abuse@host.tld` (z ASN/WHOIS) |
| Dostawca poczty | formularz abuse dostawcy |
| Media społecznościowe | zgłoszenie w danej platformie |
| Strony za CDN Cloudflare | formularz abuse Cloudflare |

### Krok 4-5: Dokumentacja i kontrola

Zapisz: datę zgłoszenia (UTC), odbiorcę, metodę, numer referencyjny, przypomnienie o kontroli (zwykle po 48–72h). Brak odpowiedzi → uprzejme ponowienie → eskalacja do wyższego dostawcy.

### Zgłoszenie do IC3 (FBI Internet Crime Complaint Center)

Dla ofiar/skutków w USA: ic3.gov. Wymagane m.in.: dane ofiary (za zgodą), dane podejrzanego, kwota straty, metoda płatności, zachowana korespondencja (z sumami kontrolnymi), raport śledczy (PDF), oś czasu.

### Odpowiedniki międzynarodowe

| Kraj | Instytucja |
|---|---|
| 🇺🇸 USA | IC3 (ic3.gov), FTC (reportfraud.ftc.gov) |
| 🇬🇧 UK | Action Fraud |
| 🇨🇦 Kanada | CAFC |
| 🇦🇺 Australia | Scamwatch |
| 🇪🇺 UE | Europol, krajowy CERT |

---

## Dobre praktyki i OPSEC

### Bezpieczeństwo sieciowe
- Zawsze używaj VPN lub Tor podczas działań OSINT
- Rozważ osobną sieć/ISP dla wrażliwych śledztw
- Korzystaj z migawek maszyny wirtualnej, by zachować „czysty" stan
- Rotuj adresy IP

### Bezpieczeństwo przeglądarki
- Osobna przeglądarka do OSINT (oddzielna od prywatnej)
- Wyłączaj JavaScript, gdy to możliwe
- Regularnie czyść ciasteczka i pamięć podręczną
- Przeglądarki nastawione na prywatność (Brave, utwardzony Firefox)

### Zarządzanie kontami
- Konta „burner" do rekonesansu w social media – nigdy prywatne konta
- Szczegółowo zaplanowane persony (sock puppets)
- Osobny e-mail dla każdej persony, wirtualne numery telefonu

### Ochrona tożsamości
- VPN + Tor dla maksymalnej anonimowości
- Nie loguj się na prywatne konta podczas śledztwa
- Nie mieszaj person między sobą
- Prywatne usługi pocztowe (ProtonMail, Tutanota)

### Zarządzanie danymi i dokumentacja
- Spójne nazewnictwo plików i folderów, znakowanie czasem
- Łańcuch dowodowy dla wszystkich dowodów
- Dokumentuj źródła każdej informacji
- Rób zrzuty ekranu, archiwizuj strony, nagrywaj dynamiczną treść, loguj użyte komendy i zapytania

### Weryfikacja
- Zawsze porównuj informacje z kilku źródeł
- Weryfikuj u źródła pierwotnego, gdy to możliwe
- Uważaj na dezinformację i fałszywe profile
- Sprawdzaj daty pod kątem aktualności
- Dokumentuj poziom pewności ustaleń

---

## Aspekty prawne i etyczne

### Ramy prawne

- **CFAA (USA)** – zabrania nieautoryzowanego dostępu do systemów; OSINT zbiera wyłącznie informacje *publicznie dostępne*, bez omijania zabezpieczeń
- **RODO/GDPR (UE)** – reguluje zbieranie i przetwarzanie danych osobowych; dotyczy mieszkańców UE niezależnie od lokalizacji śledczego; uwzględnij „prawo do bycia zapomnianym"
- **Regulaminy serwisów (ToS)** – szanuj je; wiele stron zabrania automatycznego scrapingu; naruszenie może skutkować konsekwencjami prawnymi

### Zasady etyczne

1. **Szanuj prywatność** – nie zbieraj więcej niż wymaga tego zakres śledztwa
2. **Nie szkodź** – rozważ konsekwencje swojego śledztwa
3. **Bądź transparentny** – wiedz, dla kogo i po co pracujesz
4. **Przestrzegaj prawa**
5. **Zachowuj profesjonalizm** – obiektywność i dokładność

### Czego nigdy nie robić

- Włamań ani nieautoryzowanego dostępu do systemów
- Socjotechniki i pretekstowania
- Doxxingu ani nękania
- Dostępu do danych prywatnych/chronionych
- Omijania zabezpieczeń
- Podszywania się w złych intencjach
- Udostępniania zebranych informacji do celów przestępczych

### Zastosowania dopuszczalne vs zakazane

**Dopuszczalne**: analiza zagrożeń cybernetycznych, śledztwa organów ścigania (z odpowiednim umocowaniem), due diligence firm, dziennikarstwo śledcze, badania naukowe, autoryzowane poszukiwania osób zaginionych, zapobieganie oszustwom, sprawdzanie danych za zgodą.

**Zakazane**: stalking i nękanie, kradzież tożsamości, nielegalny szpiegostwo korporacyjne, szantaż, nieautoryzowana inwigilacja prywatna, doxxing z zemsty, naruszanie prywatności bez uzasadnienia.

---

## Materiały do dalszej nauki

### Szkolenia i certyfikaty

- **Michael Bazzell / IntelTechniques** – seria książek „Open Source Intelligence Techniques", kursy, podcast „The Privacy, Security, & OSINT Show" (inteltechniques.com)
- **Trace Labs** – społeczność i zawody CTF w stylu OSINT (tracelabs.org)
- **SANS SEC487** – „Open Source Intelligence Gathering and Analysis", certyfikat GOSI (GIAC)
- Inne platformy: Udemy, Cybrary, TCM Security, TryHackMe

### Książki

1. *Open Source Intelligence Techniques* – Michael Bazzell (podstawowy podręcznik, regularnie aktualizowany)
2. *OSINT Handbook* – darmowy zasób od i-intelligence (szwajcarskiej prywatnej firmy szkoleniowej, mimo nazwy nie jest agencją rządową); aktualne wydanie na osinthandbook.com
3. *Social Engineering: The Science of Human Hacking* – Christopher Hadnagy
4. *The Art of Invisibility* – Kevin Mitnick

### Społeczności i fora

- Reddit: r/OSINT, r/SocialEngineering
- Discord: Trace Labs, OSINT Curious i inne
- Sector035.nl – artykuły i cotygodniowy newsletter
- OSINT Framework: osintframework.com

### Praktyka

- TraceLabs CTF, Sector035, Bellingcat's Online Investigation Toolkit
- osintchallenge.com, CTFtime.org, HackTheBox, TryHackMe

### Blogi i serwisy informacyjne

Bellingcat (bellingcat.com), blog IntelTechniques, Sector035, OSINT Curious, Aware Online

### Przydatne narzędzia weryfikacyjne

InVID (weryfikacja wideo), FotoForensics, Jeffrey's Exif Viewer, TinEye (odwrotne wyszukiwanie obrazem)

### Serwisy archiwizujące

Internet Archive / Wayback Machine (archive.org), Archive.today (archive.is), Perma.cc

---

## Zastrzeżenie prawne

> ⚠️ Ten przewodnik służy wyłącznie celom edukacyjnym, autoryzowanym badaniom bezpieczeństwa i legalnym śledztwom prowadzonym z odpowiednim umocowaniem.
>
> **Zabronione jest** wykorzystanie go do stalkingu, nękania, doxxingu, nieautoryzowanego dostępu do systemów lub danych, naruszania przepisów o ochronie prywatności oraz jakichkolwiek działań niezgodnych z prawem.
>
> Zawsze: uzyskaj odpowiednie upoważnienie przed rozpoczęciem śledztwa, przestrzegaj przepisów o ochronie danych, respektuj regulaminy serwisów, działaj etycznie i odpowiedzialnie, rozważ skutki swoich działań.
>
> Autorzy nie ponoszą odpowiedzialności za nadużycie opisanych narzędzi i informacji. W razie wątpliwości skonsultuj się z prawnikiem.

---

*Wykorzystuj tę wiedzę odpowiedzialnie i etycznie.*
