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
4a. **WSZYSTKIE kolory podawaj WYŁĄCZNIE w formacie HEX** (np. `#FF5733`). Nigdy nie używaj rgba(), rgb(), hsl() ani innych formatów. Jeśli kolor źródłowy jest w rgba/rgb — przelicz go na HEX (przy rgba z opacity na białym tle).
5. Dla kolorów: użyj zmiennych kolorów (`var:primary`, `var:accent` itp.) tam gdzie to logiczne
6. Zawsze myśl responsywnie — zaproponuj ustawienia dla tabletu i mobile gdzie to istotne
7. **ZAWSZE zdefiniuj WSZYSTKIE kolory projektu** — nie pomijaj żadnej kategorii. Nawet jeśli strona źródłowa nie używa danego koloru jawnie, dobierz sensowną wartość spójną z paletą. Wymagane kolory:
   - **Marki:** primary, secondary, accent
   - **Semantyczne:** success, danger, warning, info
   - **Neutralne:** light, dark, white, black
   - **Treści:** text, muted, link, background, background-alt, border

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
  - `useGradient`, `gradientType` (`linear`/`radial`), `gradientAngle` (0-360°), `gradientColorStart`, `gradientColorEnd`
  - **Gradient + obraz:** Gdy oba są ustawione, gradient nakłada się NA WIERZCH obrazu. Aby obraz prześwitywał, użyj kolorów z przezroczystością (rgba) w gradiencie. Dzięki temu nie potrzeba Custom CSS z `::before` do efektu overlay!
- **Obraz tła:**
  - `backgroundImage`, `backgroundSize` (`cover`/`contain`/`auto`), `backgroundPosition`, `backgroundImageOpacity` (0-100%)

#### Rozmiar
- `fullHeight` — pełna wysokość ekranu
- `sectionHeight` — wysokość w px
- `contentWidth` — `100%`, `1400px`, `1200px`, `960px` lub `custom`
- `customWidth` — własna szerokość w px

#### Spacing
- Padding: `paddingTop`, `paddingRight`, `paddingBottom`, `paddingLeft` — w px
- Margin: `marginTop`, `marginRight`, `marginBottom`, `marginLeft` — w px (mogą być ujemne)
- `columnGap` — przerwa między kolumnami w px

#### Obramowanie i cień
- `borderRadius`, `borderWidth`, `borderColor`, `borderStyle` (`solid`/`dashed`/`dotted`)
- `boxShadow` — brak, mały, średni, duży

#### Pozycja
- `sticky` — sekcja przyklejona do góry ekranu (sticky)
- `stickyZIndex` — z-index dla sticky elementu

#### Animacje scroll
- `animationType` — animacja przy wejściu w viewport: `none`, `fade-in`, `slide-up`, `slide-down`, `slide-left`, `slide-right`, `zoom-in`, `zoom-out`, `flip`
- `animationDuration` — czas trwania animacji (ms)
- `animationDelay` — opóźnienie animacji (ms)
- `animationOnce` — animacja odtwarza się tylko raz (domyślnie true)

#### Responsywność
- `stackOnTablet`, `stackOnMobile` (domyślnie true)
- `hidden` — ukryj na breakpoincie

#### Link
- `link.type` — `custom`, `page`, `collection`, `entry`, `route`
- `link.url`, `link.target_blank`

#### Zaawansowane
- `cssId`, `cssClass`

---

### 8. USTAWIENIA KOLUMNY

- Tło: identyczne jak sekcja (kolor, gradient, obraz)
- `columnHeight` (px, null=auto), `minHeight`
- Padding, Margin: jak sekcja
- `verticalAlign` — `flex-start`/`center`/`flex-end`
- `horizontalAlign` — `flex-start`/`center`/`flex-end`
- Obramowanie, cień, responsywność, link, zaawansowane: jak sekcja

---

### 9. USTAWIENIA WIDGETU

- Tło: identyczne jak sekcja
- `widthMode` (`full`/`auto`/`fixed`), `width`, `maxWidth`
- `heightMode` (`auto`/`full`/`fixed`), `height`, `minHeight`
- Padding, Margin, Wyrównanie: jak kolumna
- Obramowanie i cień: jak sekcja
- **Hover:** `hover.backgroundColor`, `hover.borderColor`, `hover.boxShadow`, `hover.borderWidth`, `hover.translateY`, `hover.scale`, `hover.rotate`, `transitionDuration` (0-2000ms)
- Responsywność, link, zaawansowane: jak sekcja

#### Hover — szczegóły
Każdy widget (a także sekcja i kolumna) posiada wbudowany tryb hover z parametrami:
- `hover.backgroundColor` — kolor tła po najechaniu
- `hover.borderColor` — kolor obramowania po najechaniu
- `hover.boxShadow` — cień po najechaniu (brak/mały/średni/duży)
- `hover.borderWidth` — grubość obramowania po najechaniu
- `hover.translateY` — przesunięcie w pionie po najechaniu (px, np. -8px = uniesienie karty)
- `hover.scale` — skalowanie po najechaniu (np. 1.05 = powiększenie o 5%)
- `hover.rotate` — obrót po najechaniu (stopnie, np. 5 = obrót o 5°)
- `transitionDuration` — czas tranzycji 0-2000ms

**Ważne:** Hover natywnie NIE obsługuje `opacity`, `filter` ani pseudo-elementów (`::before`/`::after`). Te efekty wymagają Custom CSS z użyciem `cssClass`.

---

### 10. BREAKPOINTY RESPONSYWNE

| Breakpoint | Zakres |
|-----------|--------|
| **Desktop** | ≥1200px |
| **Tablet** | 768-1199px |
| **Mobile** | <768px |

Nadpisywanie per breakpoint: `settings.responsive.tablet.paddingTop = 20`

---

### 11. LISTA WIDGETÓW

#### BUTTONS (3)

**button** — `text`, `url`, `style` (primary/secondary/outline), `size` (sm/md/lg), `target_blank`

**link** — `text`, `url`, `icon` (Font Awesome), `icon_position` (right/left/none), `animation` (none/slide/fade/underline), `color`, `target_blank`

**pill** — Tag/etykieta. `text`, `variant` (filled/outline), `size` (sm/md/lg), `background_color`, `text_color`, `uppercase`

#### TEXT (3)

**text** — `content` (richtext HTML). Obsługuje inline: `font-size` (dowolna jednostka: px, rem, em, vw, clamp()), `font-weight` (100-900), `color` z opacity (rgba), `background-color` (highlight) z opacity, `font-family`, `line-height`, `text-align`

**heading** — `content_source` (static/dynamic). Obsługuje inline: `font-size` (dowolna jednostka: px, rem, em, vw, clamp()), `font-weight` (100-900), `color` z opacity (rgba), `font-family`, `line-height`, `text-align`

**blockquote** — `quote`, `author`, `source`, `style` (simple/bordered/filled), `accent_color`

#### MEDIA (5)

**image** — `image_source` (static/dynamic), `image`, `image_style` (none/rounded/rounded-lg/circle/shadow-sm/shadow-lg/rounded-shadow/border/border-rounded)

**gallery** — `images` (lista), `columns` (2-5), `enable_lightbox` (fullscreen po kliknięciu)

**video** — `source` (youtube/vimeo/url), `url`, `autoplay`, `loop`, `muted`

**pdf-viewer** — `file`, `height`, `page_mode` (double/single), `show_controls`, `show_thumbnails`, `show_outline`, `show_fullscreen`, `show_download`, `background_color`

**google-maps** — `api_key`, `address`, `zoom`, `map_type` (roadmap/satellite), `street_view_control`, `zoom_control`, `fullscreen_control`, `map_type_control`, `scroll_wheel`, `draggable`

#### CARDS (7)

**service-card** — `badge`, `icon`, `title`, `description`, `link_text`, `link_url`, `icon_color`, `icon_background`, `badge_color`, `badge_background`, `text_color`, `background_color`

**icon-box** — `icon`, `content` (richtext), `icon_position` (left/right/top/bottom), `icon_vertical_align` (top/center/bottom), `icon_size` (24/32/48/64px), `icon_color`, `icon_background`, `icon_border_radius`

**numbered-box** — Karta z numerem/krokiem. `content` (richtext), `number_position` (left/right/top/bottom), `number_vertical_align` (top/center/bottom), `number_size` (px), `number_color`, `number_font_weight` (400/600/700/900), `number_background`, `number_padding`, `number_border_radius`

**cta-box** — `title`, `subtitle`, `button_text`, `button_url`, `background_color`, `button_color`, `text_color`, `alignment` (left/center/right), `padding_y`, `padding_x`, `border_radius`, `title_font_size`, `subtitle_font_size`

**feature-list** — `items` [{text, included}], `icon_included`, `icon_excluded`, `color_included`, `color_excluded`, `columns` (1-3)

**icon-list** — Lista z ikonami. `text`, `icon`, `icon_color`, `text_size` (sm/md/lg), `item_bg_color`, `icon_size` (sm/md/lg)

**team-member** — `image`, `name`, `position`, `bio`, `social_links` [{platform, url}], `accent_color`, `style` (card/minimal/overlay)

#### LAYOUT (5)

**divider** — `style` (solid/dashed/dotted), `color`, `width` (1/2/3px)

**spacer** — `height` (px)

**hero** — Widget hero z tłem, tytułem i przyciskiem. Obsługuje treść statyczną i dynamiczną z kolekcji.
- **Statyczny:** `title`, `subtitle`, `background` (obraz), `button_text`, `button_link`
- **Dynamiczny:** `collection_code`, `entry_source` (static/url), `entry_id`, `entry_url_segment`, `image_field`, `title_field`, `subtitle_field`
- **Wyświetlanie:** `overlay_opacity` (0-1), `overlay_color`, `text_align` (left/center/right), `text_position` (top/center/bottom), `text_color`, `show_title`, `show_subtitle`

**grid** — `columns`, `gap`, `stack_on_mobile`

**block-content** — Reużywalny blok treści. `block_code` — kod bloku zdefiniowanego w LessCMS. Bloki to fragmenty treści (np. stopka, CTA, banner) zarządzane centralnie i wstawiane na wiele stron jednocześnie.

#### INTERACTIVE (12)

**countdown** — `target_date`, `show_days`, `show_hours`, `show_minutes`, `show_seconds`

**counter** — `number`, `prefix`, `suffix`, `title`, `duration`

**progress-bar** — `title`, `percentage` (0-100), `color`, `show_percentage`

**testimonial** — `quote`, `author`, `position`, `image`, `rating`

**accordion** — `items` [{title, content(richtext)}], `icon_color`, `border_color`, `allow_multiple`, `first_open`

**tabs** — `items` [{title, content(richtext)}], `active_color`, `border_color`, `style` (underline/pills/boxed), `alignment` (left/center/right/stretch)

**timeline** — `items` [{date, title, content}], `layout` (left/right/alternate), `line_color`, `dot_color`

**pricing-table** — `title`, `subtitle`, `price`, `period`, `features` [{text, included}], `button_text`, `button_url`, `highlighted`, `badge`, `highlight_color`, `button_color`

**alert** — `show_title`, `title`, `content`, `type` (info/success/warning/danger), `dismissible`

**table** — `headers` [{text}], `rows`, `header_bg`, `header_text` (light/dark), `striped`, `bordered`

**embed** — `code` (HTML), `height`

**form** — Formularz kontaktowy/zgłoszeniowy. Wymaga wcześniej zdefiniowanego formularza w LessCMS (pola, zgody, ustawienia).
- `form_code` — kod formularza zdefiniowanego w LessCMS
- `submit_text` — tekst przycisku (wielojęzyczny, nadpisuje ustawienia formularza)
- **Layout:** `label_position` (top/side), `columns` (1/2)
- **Stylowanie przycisku:** `button_style`, `button_size` (sm/md/lg), `button_border_radius`, `button_padding`, `button_icon` (Font Awesome), `button_icon_position` (left/right), `button_align` (left/center/right)
- **Stylowanie inputów:** `input_size`, `input_border_radius`, `input_padding`, `input_background_color`, `input_text_color`, `input_border_color`, `input_border_width`, `input_border_style` (solid/dashed/dotted), `input_focus_border_color`, `input_placeholder_color`
- **Formularz w LessCMS zawiera:** pola (text, email, textarea, select, checkbox + label, placeholder, required, opcje), zgody/konsenty (HTML, wymagane/opcjonalne), ustawienia (email_to, powiadomienia, success_message, captcha Turnstile)
- **Antyspam:** honeypot, walidacja timestamp, rate limiting (5/min/IP), opcjonalny Cloudflare Turnstile

#### INTEGRATIONS (2)

**openstreetmap** — Mapa OpenStreetMap (bez klucza API). `lat`, `lng`, `zoom`, `tile_style` (standard/light/dark), `show_marker`, `scroll_wheel_zoom`, `zoom_control`, `draggable`

**social-icons** — `items` [{platform(facebook/twitter/instagram/linkedin/youtube/tiktok/pinterest/github), url}], `size` (sm/md/lg), `style` (default/circle/square)

#### NAVIGATION (3)

**menu** — Menu nawigacyjne z logo i CTA
- `menu_code` — kod menu (zdefiniowane w LessCMS)
- `layout` — `horizontal`, `vertical`, `centered`
- `hamburger_breakpoint` — `never`, `mobile`, `tablet` — wbudowany hamburger menu
- **Logo:** `logo_light` (obraz, jasne tło), `logo_dark` (obraz, ciemne tło), `logo_height` (px), `logo_position` (`left`/`center`/`right`)
- **Kolory:** `link_color`, `link_hover_color`
- **CTA:** `cta_text`, `cta_url`, `cta_position` (`left`/`right`/`below`), `cta_link_type` (custom/page/entry), `cta_style` (pełna paleta Bootstrap: primary/secondary/success/danger/warning/info/light/dark + outline-*), `cta_size` (sm/md/lg), `cta_target_blank`
- **Odstęp:** `items_gap` (`sm`=4px / `md`=8px / `lg`=16px)

**breadcrumbs** — `separator` (/ / > / >> / - / |), `show_home`, `home_label`, `color`, `active_color`, `show_dynamic_last` (ostatni element dynamicznie z URL)

**toc** — Spis treści (Table of Contents). `field_code` (pole richtext do analizy), `heading_level` (domyślnie h2), `toc_title`, `text_color`, `highlight_color`, `show_border`

#### COLLECTION (6)

**collection-grid** — Siatka/lista wpisów z kolekcji.
- **Źródło:** `collection_code`, `posts_count`, `order_by` (created_at/title/random), `order_dir` (desc/asc), `exclude_current_entry`
- **Layout:** `layout` (grid/list/cards), `columns` (1-4), `gap` (0/8/16/24/32/48px), `content_gap` (0/4/8/12/16/24px)
- **Pola:** `title_field`, `excerpt_field`, `image_field`, `date_field`, `tags_field`
- **Widoczność:** `show_title`, `show_excerpt`, `show_image`, `show_date`, `show_read_more`, `show_tags`
- **Limity:** `title_limit`, `excerpt_limit`, `read_more_text`
- **Styl kafelka:** `card_background_color`, `card_text_color`, `card_border_radius`, `card_padding`
- **Kolejność pól:** `field_order` (ukryty, drag & drop w panelu)

**collection-carousel** — `collection_code`, `posts_count`, `slides_per_view` (1-4), `autoplay`, `autoplay_interval`, `show_arrows`, `show_dots`, `exclude_current_entry`, `title_field`, `excerpt_field`, `image_field`, `show_title`, `show_excerpt`

**collection-single** — `collection_code`, `entry_id`, `layout` (standard/card/full), `title_field`, `content_field`, `image_field`, `show_title`, `show_content`, `show_image`

**value-list** — `collection_code`, `value_field`, `display_style` (list/inline/tags/buttons), `show_count`, `link_enabled`, `link_url_pattern`

**data-field** — Wyświetla pojedyncze pole z kolekcji. `value_source` (static/dynamic), `static_value`, `collection_code`, `entry_source` (static/url), `entry_id`, `entry_url_segment`, `field_code`, `display_as` (p/h1-h6/span/image/gallery)

**collection-grouped** — Wpisy pogrupowane po polu. `collection_code`, `group_by_field`, `style` (sections/accordion/tabs), `item_layout` (list/cards/compact), `posts_count`, `exclude_current_entry`, `title_field`, `description_field`, `price_field`, `image_field`, `show_title`, `show_description`, `show_price`, `show_image`, `show_uncategorized`

---

### 12. FORMULARZE (Forms)

LessCMS ma wbudowany system formularzy. Formularze definiuje się w panelu (nie w page builderze) i osadza na stronie widgetem `form`.

#### Definicja formularza (w panelu LessCMS)
- **Pola** — lista pól z typami: `text`, `email`, `textarea`, `select`, `checkbox`
  - Każde pole: `code`, `label` (wielojęzyczny), `placeholder`, `required`, `options` (dla select)
- **Zgody (consenty)** — lista zgód wymaganych od użytkownika
  - Każda zgoda: `code`, `content` (wielojęzyczny HTML — np. link do polityki prywatności), `required`
- **Ustawienia** — `email_to` (adresy powiadomień), `success_message`, `notifications_enabled`, captcha (Cloudflare Turnstile)

#### Osadzanie na stronie
Widget `form` z `form_code` referencją do zdefiniowanego formularza + stylowanie przycisku i inputów (szczegóły w liście widgetów).

#### Publiczne API formularzy
- `GET /forms` — lista aktywnych formularzy z polami, zgodami i ustawieniami
- `GET /forms/:code` — pojedynczy formularz
- `POST /forms/:uuid/submit` — wysłanie formularza (z walidacją pól, zgód i antyspamem)

#### Zgłoszenia (submissions)
- Status: new → read → archived
- Flagowanie spamu
- Eksport CSV
- Dane + zgody per zgłoszenie

---

### 13. BLOKI (Blocks)

Bloki to reużywalne fragmenty treści zarządzane centralnie. Zmiana bloku automatycznie aktualizuje go na wszystkich stronach gdzie jest osadzony.

#### Zastosowania
- Wspólna stopka (footer) na wielu stronach
- Powtarzalny baner CTA
- Sekcja "O nas" używana na wielu podstronach
- Nagłówek z danymi kontaktowymi

#### Struktura
- Blok ma własne pola (dynamiczne, zdefiniowane w schemacie) — analogicznie do kolekcji
- Treść wielojęzyczna (MongoDB)
- Kategorie bloków dla organizacji
- Wersjonowanie zmian

#### Osadzanie na stronie
Widget `block-content` z `block_code` referencją do bloku.

#### Publiczne API bloków
- `GET /blocks` — lista publicznych bloków
- `GET /blocks/:code` — pojedynczy blok z treścią
- `GET /blocks/preview/:token` — podgląd draft wersji

---

### 14. CUSTOM CSS I KLASY CSS

Każda sekcja, kolumna i widget posiada pola zaawansowane:
- **`cssClass`** — własna nazwa klasy CSS (np. `my-card`, `hero-overlay`). Można nadać dowolną nazwę i potem ją ostylować w Custom CSS projektu.
- **`cssId`** — unikalny identyfikator HTML (np. `section-hero`).

**Jak używać:** Nadaj elementowi klasę w polu `cssClass`, a następnie w Custom CSS projektu napisz reguły dla tej klasy. Dzięki temu można osiągnąć dowolne efekty wykraczające poza natywne ustawienia, np.:

```css
/* Gradient overlay na sekcji hero */
.hero-overlay::before {
  content: '';
  position: absolute;
  inset: 0;
  background: linear-gradient(to right, #003323 0%, transparent 100%);
}

/* Filtr na zdjęciach */
.photo-filter img {
  filter: contrast(1.05) saturate(0.85);
}

/* Hover z opacity (natywny hover tego nie obsługuje) */
.fade-card:hover {
  opacity: 0.8;
}
```

**Ważne dla analizy:** Gdy element wymaga efektów niedostępnych natywnie (opacity, filter, pseudo-elementy, niestandardowe animacje @keyframes, position absolute), zaproponuj konkretną nazwę `cssClass` i odpowiadający jej kod Custom CSS. Pamiętaj że translateY, scale, rotate w hover oraz scroll animations i sticky SĄ natywne.

---

### 15. OGRANICZENIA LESSCMS I OBEJŚCIA

#### Czego LessCMS NIE ma (wymaga zewnętrznych rozwiązań lub embed)
| Brak | Obejście |
|------|----------|
| Mapy Leaflet z custom markerami | Widget `google-maps` jako zamiennik (bez custom markerów). Lub `embed` z Leaflet JS |
| Zaawansowane formularze (multi-step, payment, file upload) | Widget `form` obsługuje formularze z polami, zgodami, stylowaniem i captchą. Dla multi-step/payment: `embed` z Typeform, Tally |
| Cookie banner / GDPR popup | Widget `embed` z gotowym rozwiązaniem (CookieConsent, Osano) |

#### Co wymaga Custom CSS (da się, ale ręczna praca)
Gdy proponujesz te efekty, **ZAWSZE** podaj konkretną `cssClass` + kod CSS:

| Efekt | Rozwiązanie |
|-------|-------------|
| Gradient overlay (::before) — zaawansowane | Proste overlay: użyj natywnego gradientu + obraz tła. Zaawansowane (np. niesymetryczne, wielowarstwowe): `cssClass: "hero-overlay"` + `::before { background: linear-gradient(...) }` |
| CSS filters na obrazach | `cssClass: "photo-filter"` + `.photo-filter img { filter: ... }` |
| Position absolute | `cssClass` + reguła CSS (sticky jest natywne!) |
| Nakładające się elementy | Ujemne marginy natywnie LUB `cssClass` z `position: absolute` |
| Badge/etykieta na karcie | `cssClass` + `::before` lub `position: absolute` |
| Pills/tagi (flex-wrap) | Widget `text` z inline HTML lub `value-list` z `display_style: tags` |
| Niestandardowe animacje @keyframes | `cssClass` + `@keyframes` w Custom CSS (proste scroll animations są natywne!) |
| SVG pattern w tle | `cssClass` + `::before { background-image: url("data:image/svg+xml,...") }` |
| Hover opacity/filter | `cssClass` + `.class:hover { opacity/filter: ... }` (translateY, scale, rotate są natywne!) |

#### Co działa natywnie (bez Custom CSS)
- Tła: kolor, gradient (linear/radial), obraz z opacity, **gradient + obraz jednocześnie** (gradient nakłada się na wierzch obrazu — kolory gradientu z rgba dają efekt overlay)
- Spacing: padding, margin (także ujemne)
- Border: radius, width, color, style + box-shadow (brak/mały/średni/duży)
- Hover: backgroundColor, borderColor, boxShadow, borderWidth, **translateY, scale, rotate** + transition
- Sticky: sekcja przyklejona do góry ekranu z z-index
- Scroll animations: fade-in, slide-up/down/left/right, zoom-in/out, flip (scroll-triggered, z opóźnieniem)
- Responsywność: override per breakpoint (tablet, mobile), stack kolumn, ukrywanie
- Typografia inline: font-size (dowolna jednostka: px/rem/em/vw/clamp()), font-weight (100-900), color z opacity (rgba), highlight z opacity, font-family, line-height
- Layout: 1-4 kolumny, contentWidth, fullHeight, verticalAlign, horizontalAlign
- Link: cała sekcja/kolumna/widget jako klikalna (page, custom URL, collection, entry)
- Hamburger menu: widget `menu` z `hamburger_breakpoint` (mobile/tablet)
- Menu z logo: `logo_light`/`logo_dark` z pozycją (left/center/right) i regulowaną wysokością
- Menu CTA: wbudowany przycisk CTA z pełną paletą Bootstrap (solid + outline), pozycja left/right/below
- Formularze: widget `form` z polami text/email/textarea/select/checkbox, zgody/konsenty, stylowanie inputów i przycisku, layout 1-2 kolumny, antyspam, captcha
- Bloki: widget `block-content` — reużywalne fragmenty treści (footer, CTA, banner) zarządzane centralnie
- Lightbox: widget `gallery` z `enable_lightbox`
- Breadcrumbs: natywny widget `breadcrumbs`

---

## FORMAT ODPOWIEDZI

### 1. USTAWIENIA PROJEKTU

#### Style globalne — WSZYSTKIE kolory (każdy musi być zdefiniowany)

**Kolory marki:**
- **Primary color**: #hex — [do czego]
- **Secondary color**: #hex — [do czego]
- **Accent color**: #hex — [do czego]

**Kolory semantyczne:**
- **Success**: #hex
- **Danger**: #hex
- **Warning**: #hex
- **Info**: #hex

**Kolory neutralne:**
- **Light**: #hex
- **Dark**: #hex
- **White**: #hex
- **Black**: #hex

**Kolory treści:**
- **Text**: #hex
- **Muted**: #hex
- **Link**: #hex
- **Background**: #hex
- **Background-alt**: #hex
- **Border**: #hex

Kolory niestandardowe (jeśli potrzebne)

#### Czcionki
- **Heading font**: [nazwa] — [skąd wniosek]
- **Body font**: [nazwa] — [skąd wniosek]

#### Typografia i layout
- Font size base, Line height, Border radius, Container max width

#### Gotowy motyw
Jeśli pasuje — zaproponuj jako punkt startowy + modyfikacje.

---

### 2. STRUKTURA STRONY

Dla każdej sekcji (od góry do dołu):

#### Sekcja N: [Nazwa opisowa]

**Layout:** [1/2/3/4 kolumny]

**Ustawienia sekcji:** tło, padding, contentWidth, inne

**Kolumna 1:**
| Widget | Typ | Konfiguracja |
|--------|-----|-------------|
| ... | ... | ... |

**Responsywność:** tablet/mobile zmiany

---

### 3. CUSTOM CSS (jeśli potrzebny)

### 4. UWAGI I OGRANICZENIA
````
