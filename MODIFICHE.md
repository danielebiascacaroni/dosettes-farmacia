# REGISTRO MODIFICHE — Dosettes Farmacia Ascona
> File: `index.html` | Ultimo aggiornamento: 2026-04-03

---

## Modifiche apportate

### 1. Autenticazione Firebase
**Prima:** `firebase.auth().signInAnonymously()`
**Dopo:** `firebase.auth().signInWithEmailAndPassword('farmacia.ascona@gmail.com', ...)`
L'autenticazione anonima permetteva a chiunque di accedere a Firestore.

---

### 2. Sistema di controllo dispositivi
Aggiunto overlay di autorizzazione all'avvio della webapp.

**Comportamento:**
- Dispositivo non riconosciuto → schermata con campo codice admin
- Codice corretto → assegna nome → dispositivo registrato su Firestore (`dispositivi`)
- Accessi successivi → controllo automatico, accesso diretto
- Dispositivo revocato dall'admin → overlay riappare

**Codice admin:** verificato tramite hash SHA-256 hardcoded nel file.

---

### 3. PIN operatori hardcoded rimossi
**Prima:** array `OPERATORI` con PIN in chiaro (`1111`, `2222`, ecc.) visibili nel sorgente. Presente anche funzione `migraDB()` con PIN in chiaro per uso una-tantum.
**Dopo:** `let OPERATORI = []` — la lista viene caricata esclusivamente da Firestore. Se Firestore non è raggiungibile, la lista è vuota (nessun fallback con dati sensibili). Funzione `migraDB()` rimossa dal sorgente (migrazione completata).

---

### 4. Rate limiting sul login PIN
**Prima:** nessun limite ai tentativi PIN — brute force possibile.
**Dopo:** dopo 5 tentativi errati, login bloccato per 5 minuti. Ad ogni tentativo sbagliato viene mostrato il numero di tentativi rimanenti. Il blocco è gestito tramite `localStorage`.

---

## Stato sicurezza attuale

| Controllo | Stato |
|-----------|-------|
| Auth anonima Firebase disabilitata | ✅ |
| Login con email/password dedicata | ✅ |
| Firestore Security Rules aggiornate | ✅ |
| Controllo dispositivi operativo | ✅ |
| PIN operatori hardcoded rimossi | ✅ |
| Rate limiting login (5 tentativi / 5 min) | ✅ |
| Codice admin hardcoded come hash SHA-256 | ✅ |
