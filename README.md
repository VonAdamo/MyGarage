# MyGarage

MyGarage är en enkel webbapplikation byggd med Vue.js. Appen är skapad som en skoluppgift för att demonstrera grunderna i Vue.

## Funktioner

- Lägg till bilar med modell och årsmodell
- Visa och välj mellan flera bilar
- Lägg till planerat underhåll för den valda bilen
- Ta bort planerat underhåll
- Visa ett meddelande när en bil inte har något planerat underhåll

Projektet använder bland annat `ref`, `v-model`, `v-for`, `v-if`, `@submit` och `@click`.

## Starta projektet

Installera projektets paket:

```bash
npm install
```

Starta utvecklingsservern:

```bash
npm run dev
```

Öppna sedan adressen som visas i terminalen, vanligtvis `http://localhost:5173`.

## Teknik

- Vue.js
- Vite
- Vanlig HTML och CSS

Appens data sparas endast i minnet och försvinner när sidan laddas om.
