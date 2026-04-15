# LANDING-ENERG — Devlog

Landing page di lead generation per **FORIGO ENER-G** (macchina agricola 100% elettrica di Roter Italia S.r.l.).
Distribuita via QR code in fiera / mail / materiale stampato. Target: **100% smartphone**.

## URL pubblico
- Produzione: https://forigo-energ-fiera.netlify.app/
- Video YouTube embeddato: https://youtu.be/M0JwmwVc8CM

## Stack
- HTML statico + TailwindCSS (CDN) + Manrope font + Material Symbols
- Nessun build step — deploy = drag & drop cartella su Netlify
- Mobile-first con `clamp()` per tipografia fluida
- Zero dipendenze JS esterne

## Struttura
```
index.html         landing principale (splash + hero + sezioni + form)
privacy.html       informativa GDPR a norma
assets/img/        8 immagini (logo, render, gallery)
qr-energ*.png      3 varianti QR pronte stampa (classico / branded / dark)
```

## Raccolta lead
- Form POST verso **Google Apps Script Web App** → scrive riga in Google Sheet "ENERG Leads"
- Apps Script invia anche **email automatica** a `federica.forigo@forigo.it` ad ogni submit
- Trigger: `MailApp.sendEmail` dentro `doPost`
- Endpoint in `index.html` → costante `SICRM_ENDPOINT`

### Colonne Sheet
Timestamp | Source | Lingua | Nome | Azienda | Email | Telefono | Ruolo | Ettari | Coltura | Note | Privacy | User-Agent

## Scelte UX chiave
- **Splash intro 3.8s**: Forigo top → macchina centro (blur→nitida) → ENERG bottom → dissolve → hero pulito. Tap = skip.
- **Hero senza overlap** macchina/testo (su mobile macchina solo nello splash, su desktop side-by-side)
- **CTA unificato**: un solo "Rimani Aggiornato" sticky in basso mobile + submit nel form. Nessun duplicato altrove.
- **Gallery horizontal swipe** su mobile (scroll-snap), grid su desktop
- **Bilingue IT/EN** via `data-it` / `data-en` sugli elementi, `setLang()` swap runtime
- Validazione form: nome + azienda + email + privacy obbligatori
- Video via `youtube-nocookie.com` per ridurre cookie tracking

## GDPR
- Privacy policy `privacy.html` con titolare identificato (Roter Italia S.r.l., P.IVA 00777410234, sede Ostiglia MN)
- Consenso esplicito checkbox pre-submit
- Menzione Google come responsabile + Data Privacy Framework per trasferimento extra-UE
- Durata conservazione: 24 mesi
- Email diritti: `forigo@forigo.it` (oggetto "PRIVACY ENERG")
- Cookie: solo tecnici + YouTube embed (menzionato)

## Branding
- Primary: `#e00719` (rosso Forigo)
- Surface dark: `#131313` / `#0e0e0e`
- Manrope 200–900

## Non applicabili
- NIS2: Roter Italia non è infrastruttura critica (fuori scope decreto 138/2024)

## TODO futuri
- [ ] Dominio custom `energ.forigo.it` (DNS Forigo)
- [ ] Traduzione EN completa di `privacy.html`
- [ ] Pagina `404.html`
- [ ] Open Graph image dedicata (1200×630)
- [ ] Eventuale integrazione diretta con SI-CRM (endpoint Marketing) al posto di Google Sheet
