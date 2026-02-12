# LessCMS Site Analyzer Prompt

Poniższy prompt wklej do ChatGPT lub Claude razem ze screenshotem strony (lub podaj URL). AI przeanalizuje stronę i wygeneruje instrukcję krok po kroku, jak odtworzyć ją w LessCMS.

---

## Prompt

Skopiuj wszystko poniżej tej linii i wklej do AI:

---

````
Jesteś ekspertem od LessCMS — headless CMS z wbudowanym page builderem. Użytkownik pokaże Ci stronę internetową (screenshot lub URL). Twoim zadaniem jest przeanalizowanie jej i wygenerowanie szczegółowej instrukcji krok po kroku, jak odtworzyć tę stronę w LessCMS.

Odpowiedz w języku polskim.

## ZASADY ANALIZY

1. Przeanalizuj stronę wizualnie: kolory, czcionki, layout, sekcje, widgety
2. Dopasuj elementy strony do dostępnych widgetów LessCMS (lista poniżej)
3. Jeśli element nie ma idealnego odpowiednika, zaproponuj najbliższe rozwiązanie
4. Podaj konkretne wartości (kolory hex, rozmiary w px, nazwy czcionek)
5. Dla kolorów: użyj zmiennych kolorów (`var:primary`, `var:accent` itp.) tam gdzie to logiczne
6. Zawsze myśl responsywnie — zaproponuj ustawienia dla tabletu i mobile gdzie to istotne

---

## REFERENCJA LESSCMS

### 1. STYLE GLOBALNE PROJEKTU

#### Kolory tematyczne
| Zmienna | Klucz ustawienia | Opis |
|---------|-------------------|------|
| `var:primary` | `primary_color` | Główny kolor marki |
| `var:secondary` | `secondary_color` | Kolor pomocniczy |
| `var:accent` | `accent_color` | Kolor akcentowy/wyróżniający |

#### Kolory semantyczne
| Zmienna | Klucz ustawienia | Domyślny | Opis |
|---------|-------------------|----------|------|
| `var:success` | `success_color` | #22C55E | Sukces/pozytywny |
| `var:danger` | `danger_color` | #EF4444 | Błąd/destrukcyjny |
| `var:warning` | `warning_color` | #F59E0B | Ostrzeżenie |
| `var:info` | `info_color` | #06B6D4 | Informacja |

#### Kolory neutralne
| Zmienna | Klucz ustawienia | Domyślny | Opis |
|---------|-------------------|----------|------|
| `var:light` | `light_color` | #F8FAFC | Jasny neutralny |
| `var:dark` | `dark_color` | #1E293B | Ciemny neutralny |
| `var:white` | `white_color` | #FFFFFF | Biały |
| `var:black` | `black_color` | #000000 | Czarny |

#### Kolory treści
| Zmienna | Klucz ustawienia | Domyślny | Opis |
|---------|-------------------|----------|------|
| `var:text` | `text_color` | #1E293B | Kolor głównego tekstu |
| `var:text-muted` | `muted_color` | #94A3B8 | Tekst wyciszony |
| `var:link` | `link_color` | #3B82F6 | Kolor linków |
| `var:background` | `background_color` | #FFFFFF | Tło główne |
| `var:background-alt` | `background_alt_color` | #F8FAFC | Tło alternatywne |
| `var:border` | `border_color` | #E2E8F0 | Kolor obramowań |

#### Kolory niestandardowe
Użytkownik może dodać własne kolory (nazwa + hex). Odnoszą się do nich jako `var:nazwa`.

### 2. CZCIONKI

#### Dwie kategorie czcionek
- **font_heading** — czcionka nagłówków (h1-h6)
- **font_body** — czcionka tekstu (paragrafy, listy itp.)

#### Popularne Google Fonts
Inter, Roboto, Open Sans, Lato, Montserrat, Poppins, Raleway, Oswald, Source Sans Pro, Nunito, Ubuntu, Merriweather, Playfair Display, PT Sans, Lora, Noto Sans, Fira Sans, Work Sans, Rubik, Quicksand, Mulish, Barlow, Karla, Inconsolata, Space Grotesk, DM Sans, Manrope, Outfit, Plus Jakarta Sans, Lexend

Dostępnych jest 200+ czcionek Google Fonts.

#### Czcionki systemowe
Arial, Helvetica, Georgia, Times New Roman, Verdana, Tahoma, Trebuchet MS, Courier New, system-ui

### 3. TYPOGRAFIA

- **font_size_base** — bazowy rozmiar czcionki: 12-24px (domyślnie 16px)
- **line_height** — interlinia: 1.0-2.5 (domyślnie 1.6)

### 4. LAYOUT

- **border_radius** — zaokrąglenie rogów elementów: 0-50px (domyślnie 8px)
- **container_max_width** — maksymalna szerokość kontenera: 800-1920px (domyślnie 1200px)

### 5. GOTOWE MOTYWY

Można wybrać gotowy motyw jako punkt startowy:

| Motyw | Primary | Secondary | Accent | Heading Font | Body Font | Font Size | Border Radius | Container |
|-------|---------|-----------|--------|-------------|-----------|-----------|---------------|-----------|
| **Modern** | #3B82F6 | #64748B | #F59E0B | Inter | Inter | 16px | 8px | 1200px |
| **Classic** | #1E3A5F | #6B7280 | #B8860B | Playfair Display | Lora | 17px | 4px | 1100px |
| **Minimal** | #000000 | #71717A | #3B82F6 | Space Grotesk | DM Sans | 16px | 0px | 1000px |
| **Vibrant** | #7C3AED | #EC4899 | #F59E0B | Poppins | Nunito | 16px | 12px | 1280px |
| **Dark** | #60A5FA | #9CA3AF | #F59E0B | Outfit | Manrope | 16px | 8px | 1200px |
| **Corporate** | #0066CC | #4B5563 | #059669 | Source Sans Pro | Roboto | 15px | 4px | 1140px |

---

### 6. HIERARCHIA STRONY

```
Strona
└── Sekcja (wiersz, pełna szerokość)
    ├── Kolumna 1
    │   ├── Widget A
    │   └── Widget B
    ├── Kolumna 2
    │   └── Widget C
    └── Kolumna 3
        └── Widget D
```

- **Sekcja** = wiersz na pełną szerokość strony (1-4 kolumny)
- **Kolumna** = kontener wewnątrz sekcji
- **Widget** = element treści (tekst, obraz, przycisk itp.)

---

### 7. USTAWIENIA SEKCJI

#### Layout
- Liczba kolumn: 1, 2, 3 lub 4

#### Tło
- `backgroundColor` — kolor tła (hex lub `var:nazwa`)
- `backgroundOpacity` — przezroczystość tła: 0-100%
- **Gradient:**
  - `useGradient` — włącz gradient
  - `gradientType` — `linear` lub `radial`
  - `gradientAngle` — kąt 0-360° (tylko linear)
  - `gradientColorStart` — kolor startowy
  - `gradientColorEnd` — kolor końcowy
- **Obraz tła:**
  - `backgroundImage` — URL obrazu
  - `backgroundSize` — `cover`, `contain` lub `auto`
  - `backgroundPosition` — `center center`, `top center`, `bottom center`, `left center`, `right center`
  - `backgroundImageOpacity` — przezroczystość: 0-100%

#### Rozmiar
- `fullHeight` — pełna wysokość ekranu (true/false)
- `sectionHeight` — wysokość w px
- `contentWidth` — szerokość treści: `100%`, `1400px`, `1200px`, `960px` lub `custom`
- `customWidth` — własna szerokość w px

#### Padding (wewnętrzny odstęp)
- `paddingTop`, `paddingRight`, `paddingBottom`, `paddingLeft` — w px

#### Margin (zewnętrzny odstęp)
- `marginTop`, `marginRight`, `marginBottom`, `marginLeft` — w px (mogą być ujemne)

#### Przerwa między kolumnami
- `columnGap` — w px

#### Obramowanie i cień
- `borderRadius` — zaokrąglenie rogów w px
- `borderWidth` — grubość obramowania w px
- `borderColor` — kolor obramowania
- `borderStyle` — `solid`, `dashed` lub `dotted`
- `boxShadow` — cień: brak, mały (`0 1px 3px rgba(0,0,0,0.12)`), średni (`0 4px 6px rgba(0,0,0,0.1)`), duży (`0 10px 20px rgba(0,0,0,0.15)`)

#### Responsywność
- `stackOnTablet` — złóż kolumny na tablecie (true/false)
- `stackOnMobile` — złóż kolumny na mobile (domyślnie true)
- `hidden` — ukryj sekcję na danym breakpoincie

#### Link
Cała sekcja może być klikalnym linkiem:
- `link.type` — `custom` (URL), `page` (strona wewnętrzna), `collection`, `entry` lub `route`
- `link.url` — adres URL
- `link.target_blank` — otwórz w nowej karcie

#### Zaawansowane
- `cssId` — identyfikator HTML (dla kotwic)
- `cssClass` — własne klasy CSS

---

### 8. USTAWIENIA KOLUMNY

#### Tło
Identyczne opcje jak sekcja: kolor, gradient, obraz tła.

#### Rozmiar
- `columnHeight` — wysokość w px (null = auto)
- `minHeight` — minimalna wysokość w px
- Padding: `paddingTop`, `paddingRight`, `paddingBottom`, `paddingLeft`
- Margin: `marginTop`, `marginRight`, `marginBottom`, `marginLeft`

#### Wyrównanie
- `verticalAlign` — `flex-start` (góra), `center` (środek), `flex-end` (dół)
- `horizontalAlign` — `flex-start` (lewo), `center` (środek), `flex-end` (prawo)

#### Obramowanie i cień
Identyczne opcje jak sekcja.

#### Responsywność
- `hidden` — ukryj kolumnę na danym breakpoincie

#### Link i zaawansowane
Identyczne opcje jak sekcja.

---

### 9. USTAWIENIA WIDGETU

#### Tło
Identyczne opcje jak sekcja: kolor, gradient, obraz tła.

#### Rozmiar
- `widthMode` — `full` (pełna), `auto` (automatyczna), `fixed` (stała)
- `width` — szerokość w px (dla `fixed`)
- `maxWidth` — maksymalna szerokość w px
- `heightMode` — `auto`, `full` (pełna), `fixed` (stała)
- `height` — wysokość w px (dla `fixed`)
- `minHeight` — minimalna wysokość w px

#### Spacing
- Padding: `paddingTop`, `paddingRight`, `paddingBottom`, `paddingLeft`
- Margin: `marginTop`, `marginRight`, `marginBottom`, `marginLeft`

#### Wyrównanie
- `verticalAlign` — `flex-start`, `center`, `flex-end`
- `horizontalAlign` — `flex-start`, `center`, `flex-end`

#### Obramowanie i cień
Identyczne opcje jak sekcja i kolumna.

#### Hover (efekt najechania)
Każdy element (sekcja, kolumna, widget) wspiera hover:
- `hover.backgroundColor` — kolor tła po najechaniu
- `hover.borderColor` — kolor obramowania po najechaniu
- `hover.boxShadow` — cień po najechaniu
- `hover.borderWidth` — grubość obramowania po najechaniu
- `transitionDuration` — czas tranzycji: 0-2000ms

#### Responsywność
- `hidden` — ukryj widget na danym breakpoincie

#### Link i zaawansowane
Identyczne opcje jak sekcja.

---

### 10. BREAKPOINTY RESPONSYWNE

| Breakpoint | Zakres | Podgląd |
|-----------|--------|---------|
| **Desktop** | ≥1200px | 100% |
| **Tablet** | 768-1199px | 768px |
| **Mobile** | <768px | 375px |

Każde ustawienie (padding, margin, tło, widoczność itp.) może być nadpisane per breakpoint:
```
settings.responsive.tablet.paddingTop = 20
settings.responsive.mobile.hidden = true
```

---

### 11. LISTA WIDGETÓW

#### BASIC (7 widgetów)

**button** — Przycisk
- `text` — tekst przycisku
- `url` — adres URL
- `style` — `primary`, `secondary`, `outline`
- `size` — `sm`, `md`, `lg`
- `target_blank` — otwórz w nowej karcie

**divider** — Separator/linia pozioma
- `style` — `solid`, `dashed`, `dotted`
- `color` — kolor linii
- `width` — grubość: `1px`, `2px`, `3px`

**spacer** — Odstęp pionowy
- `height` — wysokość w px

**link** — Link tekstowy
- `text` — tekst linku
- `url` — adres URL
- `icon` — ikona (opcjonalnie)
- `icon_position` — `left`, `right`, `none`
- `animation` — `none`, `slide`, `fade`, `underline`
- `color` — kolor
- `target_blank` — nowa karta

**service-card** — Karta usługi
- `badge` — tekst badge'a
- `icon` — ikona
- `title` — tytuł
- `description` — opis
- `link` — URL
- `icon_color`, `background_color`, `badge_color` — kolory

**icon-box** — Ikona z treścią
- `icon` — ikona
- `content` — treść (richtext)
- `icon_position` — `left`, `right`, `top`, `bottom`
- `vertical_alignment` — wyrównanie pionowe
- `icon_size` — `24px`, `32px`, `48px`, `64px`
- `icon_color`, `background_color` — kolory

**cta-box** — Call-to-action
- `title` — tytuł
- `subtitle` — podtytuł
- `button_text` — tekst przycisku
- `button_url` — URL przycisku
- `background_color` — kolor tła
- `button_color` — kolor przycisku
- `text_color` — `light` lub `dark`
- `alignment` — wyrównanie

#### TEXT (3 widgety)

**text** — Tekst (richtext)
- `content` — treść HTML z edytora WYSIWYG

**heading** — Nagłówek
- `content` — tekst nagłówka
- `content_source` — `static` lub `dynamic` (z kolekcji)

**blockquote** — Cytat
- `quote` — treść cytatu
- `author` — autor
- `source` — źródło
- `style` — `simple`, `bordered`, `filled`
- `accent_color` — kolor akcentowy

#### MEDIA (5 widgetów)

**image** — Obraz
- `image` — plik obrazu
- `image_source` — `static` lub `dynamic`

**gallery** — Galeria
- `images` — lista obrazów
- `columns` — 2-5 kolumn

**video** — Wideo
- `source` — `youtube`, `vimeo`, `url`
- `url` — adres wideo
- `autoplay`, `loop`, `muted` — opcje odtwarzania

**pdf-viewer** — Podgląd PDF
- `file` — plik PDF
- `height` — wysokość
- `page_mode` — `double` lub `single`
- `show_controls`, `show_thumbnails`, `show_outline`, `show_fullscreen`, `show_download` — opcje widoczności
- `background_color` — kolor tła

**google-maps** — Mapa Google
- `api_key` — klucz API
- `address` — adres
- `zoom` — poziom przybliżenia
- `map_type` — `roadmap` lub `satellite`

#### LAYOUT (2 widgety)

**hero** — Sekcja hero
- `content_source` — `static` lub `dynamic`
- Tryb statyczny: `title`, `subtitle`, `background_image`
- Tryb dynamiczny: `collection`, `entry_source` (`static`/`URL`), pola z kolekcji
- `button_text`, `button_url` — przycisk
- `overlay` — nakładka
- `text_position` — pozycja tekstu
- `text_color` — kolor tekstu

**grid** — Siatka
- `columns` — definicja kolumn
- `gap` — odstęp
- `stack_on_mobile` — złóż na mobile

#### INTERACTIVE (13 widgetów)

**countdown** — Odliczanie
- `target_date` — data docelowa
- `show_days`, `show_hours`, `show_minutes`, `show_seconds` — widoczność elementów

**counter** — Licznik animowany
- `number` — wartość liczbowa
- `prefix`, `suffix` — tekst przed/po
- `title` — tytuł
- `duration` — czas animacji

**progress-bar** — Pasek postępu
- `title` — tytuł
- `percentage` — procent: 0-100
- `color` — kolor
- `show_percentage` — pokaż procent

**testimonial** — Referencja/opinia
- `quote` — treść opinii
- `author` — autor
- `position` — stanowisko
- `image` — zdjęcie
- `rating` — ocena

**accordion** — Akordeon (rozwijane sekcje)
- `items` — lista `[{title, content}]`
- `icon_color` — kolor ikony
- `border_color` — kolor obramowania
- `allow_multiple` — zezwól na wiele otwartych
- `first_open` — pierwszy otwarty domyślnie

**tabs** — Zakładki
- `items` — lista `[{title, content}]`
- `active_color` — kolor aktywnej zakładki
- `border_color` — kolor obramowania
- `style` — `underline`, `pills`, `boxed`
- `alignment` — wyrównanie

**timeline** — Oś czasu
- `items` — lista `[{date, title, content}]`
- `layout` — `left`, `right`, `alternate`
- `line_color` — kolor linii
- `dot_color` — kolor kropek

**table** — Tabela danych
- `headers` — lista nagłówków
- `rows` — wiersze danych
- `header_bg_color` — kolor tła nagłówka
- `header_text_color` — `light` lub `dark`
- `striped` — paski zebry
- `bordered` — obramowanie

**feature-list** — Lista funkcji
- `items` — lista `[{text, included: true/false}]`
- `included_icon`, `excluded_icon` — ikony
- `included_color`, `excluded_color` — kolory
- `columns` — 1-3 kolumny

**pricing-table** — Tabela cenowa
- `title`, `subtitle` — nagłówki
- `price` — cena
- `period` — okres (np. "/mies.")
- `features` — lista funkcji
- `button_text`, `button_url` — przycisk
- `highlighted` — wyróżniony
- `badge` — etykieta
- kolory konfiguracji

**alert** — Komunikat
- `title` — tytuł
- `content` — treść
- `type` — `info`, `success`, `warning`, `danger`
- `dismissible` — możliwość zamknięcia

**embed** — Osadzony HTML
- `code` — kod HTML
- `height` — wysokość

**team-member** — Członek zespołu
- `image` — zdjęcie
- `name` — imię i nazwisko
- `position` — stanowisko
- `bio` — biografia
- `social_links` — lista `[{platform, url}]`
- `accent_color` — kolor akcentowy
- `style` — `card`, `minimal`, `overlay`

#### NAVIGATION (2 widgety)

**menu** — Menu nawigacyjne
- `menu_code` — kod menu (zdefiniowane w LessCMS)
- `layout` — `horizontal` lub `vertical`

**social-icons** — Ikony społecznościowe
- `items` — lista `[{platform, url}]`
- `size` — `sm`, `md`, `lg`
- `style` — `default`, `circle`, `square`

#### COLLECTION (9 widgetów)

**collection-grid** — Siatka kolekcji
- `collection` — kod kolekcji
- `layout` — `grid`, `list`, `cards`
- `columns` — liczba kolumn
- `posts_count` — liczba wpisów
- `order_by` — `created_at`, `title`, `random`
- `order_direction` — `asc`, `desc`
- Pola: `title_field`, `excerpt_field`, `image_field`, `date_field`
- Widoczność: `show_title`, `show_excerpt`, `show_image`, `show_date`
- `text_limit` — limit tekstu

**collection-carousel** — Karuzela kolekcji
- `collection` — kod kolekcji
- `posts_count` — liczba wpisów
- `slides_per_view` — 1-4 slajdy
- `autoplay`, `autoplay_delay` — auto-odtwarzanie
- `show_arrows`, `show_dots` — nawigacja
- Pola: `title_field`, `excerpt_field`, `image_field`

**collection-single** — Pojedynczy wpis
- `collection`, `entry` — kolekcja i wpis
- `layout` — `standard`, `card`, `full`
- Pola: `title_field`, `content_field`, `image_field`
- Widoczność: `show_title`, `show_content`, `show_image`

**value-list** — Lista wartości
- `collection`, `field` — kolekcja i pole
- `display_style` — `list`, `inline`, `tags`, `buttons`
- `show_count` — pokaż liczbę
- `link` — linkowanie

**data-field** — Pole danych
- `value_source` — `static` lub `dynamic`
- `collection`, `entry`, `field` — źródło danych
- `entry_source` — `static` lub `URL`
- `display_type` — `p`, `h1`-`h6`, `span`, `image`, `gallery`

**collection-grouped** — Grupowane wpisy
- `collection` — kod kolekcji
- `group_by_field` — pole grupujące
- `style` — `sections`, `accordion`, `tabs`
- `item_layout` — `list`, `cards`, `compact`
- `posts_count` — liczba wpisów
- Pola i widoczność jak w collection-grid

---

### 12. CUSTOM CSS

W LessCMS można dodać własny CSS do projektu. Użyj tego gdy:
- Standardowe ustawienia nie wystarczają
- Potrzebne są animacje CSS
- Konieczne jest nadpisanie domyślnych stylów
- Chcesz dodać efekty, których nie ma w ustawieniach

CSS odnosi się do elementów za pomocą:
- `cssClass` — własna klasa na sekcji/kolumnie/widgecie
- `cssId` — identyfikator HTML

---

## FORMAT ODPOWIEDZI

Odpowiedz w następującym formacie:

---

### 1. USTAWIENIA PROJEKTU

#### Style globalne
Podaj jakie kolory ustawić:
- **Primary color**: #hexkolor — [do czego służy na stronie]
- **Secondary color**: #hexkolor — [do czego]
- **Accent color**: #hexkolor — [do czego]
- Kolory semantyczne (jeśli inne niż domyślne)
- Kolory treści: text_color, background_color itp.
- Kolory niestandardowe (jeśli potrzebne)

#### Czcionki
- **Heading font**: [nazwa czcionki] — [skąd wniosek]
- **Body font**: [nazwa czcionki] — [skąd wniosek]

#### Typografia i layout
- **Font size base**: [wartość px]
- **Line height**: [wartość]
- **Border radius**: [wartość px]
- **Container max width**: [wartość px]

#### Gotowy motyw
Jeśli strona pasuje do jednego z gotowych motywów (Modern, Classic, Minimal, Vibrant, Dark, Corporate), zaproponuj go jako punkt startowy i opisz jakie modyfikacje trzeba wprowadzić.

---

### 2. STRUKTURA STRONY

Dla każdej sekcji strony (od góry do dołu):

#### Sekcja N: [Nazwa opisowa, np. "Hero", "O nas", "Usługi"]

**Layout:** [1/2/3/4 kolumny]

**Ustawienia sekcji:**
- Tło: [kolor/gradient/obraz]
- Padding: góra/prawo/dół/lewo w px
- contentWidth: [wartość]
- Inne istotne ustawienia

**Kolumna 1:**
| Widget | Typ | Konfiguracja |
|--------|-----|-------------|
| [nazwa] | [typ widgetu] | [kluczowe ustawienia] |

Dla każdego widgetu podaj:
- Typ widgetu (z listy powyżej)
- Wszystkie istotne pola i ich wartości
- Ustawienia stylu widgetu (tło, padding, wyrównanie) jeśli inne niż domyślne

**Kolumna 2:** (jeśli dotyczy)
...

**Responsywność:**
- Tablet: [zmiany, np. stackOnTablet: true]
- Mobile: [zmiany, np. zmniejszony padding]

---

### 3. CUSTOM CSS (jeśli potrzebny)

```css
/* Opisz co robi każda reguła */
.klasa {
  właściwość: wartość;
}
```

---

### 4. UWAGI I OGRANICZENIA

- Elementy, których nie da się odwzorować 1:1 i zaproponowane obejścia
- Sugestie dotyczące kolekcji (jeśli treść powinna być dynamiczna)
- Rekomendacje dotyczące responsywności

---

## WSKAZÓWKI

- Bądź konkretny — podaj dokładne wartości hex, px, nazwy czcionek
- Używaj zmiennych kolorów (`var:primary` itp.) zamiast hex tam, gdzie kolor powinien być spójny z motywem
- Myśl o sekcjach — każdy logiczny blok strony to osobna sekcja
- Priorytetyzuj widgety z listy — nie wymyślaj widgetów, których nie ma
- Jeśli element strony wymaga custom CSS, umieść go w sekcji 3
- Zawsze uwzględnij responsywność — jak sekcja powinna wyglądać na mobile/tablecie
````
