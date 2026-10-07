# Adjust-Connect
Tool voor samenwerking binnen bedrijf
Adjust Connect — simpele backend
Hiermee log je in met e-mailadres + wachtwoord en wordt alle data gedeeld en onthouden tussen iedereen die de tool gebruikt. Geen Microsoft-registratie nodig — ideaal om met je teamlead te testen.
Wat het is
Een klein Node-servertje met drie dingen:
`POST /api/auth` — inloggen; bestaat het account nog niet, dan wordt het meteen aangemaakt.
`GET /api/state` — de gedeelde inhoud ophalen (sessies, hulpvragen, aanbod, forum).
`PUT /api/state` — de gedeelde inhoud opslaan.
Wachtwoorden worden versleuteld opgeslagen (bcrypt). Data staat in twee JSON-bestanden (`users.json`, `state.json`).
Lokaal draaien (om te proberen)
Node 18+ installeren.
In deze map:
```
   npm install
   npm start
   ```
De server draait op `http://localhost:3001`.
Open `adjust-connect.html`, en zet bovenaan in het script bij `CONFIG.api`:
```js
   api: { base: "http://localhost:3001" }
   ```
Open de HTML in je browser → log in met een e-mailadres + wachtwoord. Klaar.
Online zetten (zodat je teamlead het kan testen)
De frontend staat op Vercel; de backend zet je op een dienst die processen laat draaien. Render is het simpelst:
Zet de map `backend/` in een Git-repo (GitHub).
Ga naar Render → New → Web Service → koppel de repo.
Instellingen:
Build Command: `npm install`
Start Command: `npm start`
Environment variables:
`JWT\_SECRET` = een lange willekeurige tekst
(optioneel) `ALLOWED\_DOMAIN` = `adjust.nl` om alleen Adjust-adressen toe te laten
(aanbevolen) voeg een Persistent Disk toe en zet `DATA\_DIR` op dat pad (bijv. `/var/data`), anders wordt de data gewist bij een herstart/redeploy.
Je krijgt een URL, bijv. `https://adjust-connect-backend.onrender.com`.
Zet die in `adjust-connect.html` bij `CONFIG.api.base`, en deploy de HTML opnieuw op Vercel.
> Railway of Fly.io werken net zo goed. Let alleen op dat de opslag persistent is (schijf), anders is data na een herstart weg.
Belangrijk / eerlijk
Dit is een lichtgewicht opzet voor testen en klein gebruik. Het slaat de hele gedeelde inhoud op als één document met "laatste schrijfactie wint" — prima voor een klein team, niet bedoeld voor honderden gelijktijdige gebruikers.
Voor productie op termijn: of dit uitbreiden met een echte database, of overstappen op de Microsoft-login uit `HANDOVER-IT.md`. De twee kunnen samen: Microsoft-login voor identiteit + agenda, deze backend (of een DB) voor de gedeelde lijsten.
`CORS` staat nu open voor alle websites (makkelijk testen). Beperk dit later tot je eigen frontend-URL in `server.js`.
