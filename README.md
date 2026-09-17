# 🪄 Hogwarts Bridge — la bacchetta

Un telefono, tenuto in mano come una bacchetta, che riconosce gesti e
incantesimi pronunciati a voce e li traduce in comandi reali per **Hogwarts
Legacy** su PC — tramite un joypad virtuale, esattamente come se fosse un
controller vero collegato.

**Demo pubblica:** [gabrielecorso8.github.io/bacchetta-pubblica](https://gabrielecorso8.github.io/bacchetta-pubblica/)
*(richiede il bridge in esecuzione sulla stessa rete Wi-Fi — vedi "Come funziona" più sotto)*

---

## Cos'è questo repository

Solo il **client**: la pagina web che gira sul telefono. Nessuna logica di
gioco, nessuna chiave, nessun dato sensibile — per questo è pubblica e
ospitata con GitHub Pages. Tutta la parte che conta davvero (riconoscimento
dei gesti lato server, emulazione del controller, sicurezza della rete
locale) vive in un **repository privato separato**: qui c'è solo l'interfaccia,
progettata per parlare con quel bridge tramite un protocollo definito, non
per contenerne la logica.

`index.html` è generato automaticamente da uno script del repository
privato (`build_standalone_bundle.py`) a partire dai sorgenti del progetto:
non va modificato a mano qui, va ripubblicato da lì.

## Come funziona, ad alto livello

```
📱 Telefono (questa pagina)  ──Wi-Fi locale, HTTPS──▶  🖥️ PC (bridge privato)  ──▶  🎮 Hogwarts Legacy
   riconoscimento gesti          WebSocket cifrato         joypad virtuale
   e voce, interfaccia           autenticato a token        (driver ViGEmBus)
```

1. **Il telefono** legge l'orientamento e l'accelerazione con i sensori di
   bordo (giroscopio/accelerometro) per riconoscere un glifo disegnato in
   aria, oppure ascolta il nome dell'incantesimo pronunciato a voce.
2. Il gesto o la parola riconosciuti diventano un messaggio verso **il
   bridge** che gira sul PC di casa, via WebSocket su HTTPS, autenticato
   con un token e protetto da un certificato firmato da un'autorità
   generata in locale (nessun servizio cloud di terzi coinvolto).
3. **Il bridge** (repository privato) decide cosa fare con quel messaggio
   e lo traduce nella pressione di un vero pulsante su un **controller
   DualShock 4 virtuale**, che Windows e il gioco vedono come un pad
   fisico qualsiasi.
4. Un secondo, piccolo processo sempre acceso sul PC permette di accendere
   il bridge vero e proprio da remoto, dal telefono stesso, così l'unico
   link da usare è sempre questo — nessun indirizzo "fresco" da rimandarsi
   a ogni sessione di gioco.

Tutto resta **sulla rete locale**: nessun dato del gioco, dei sensori o
della sessione lascia mai la Wi-Fi di casa. GitHub Pages ospita solo i
file statici del client, esattamente come farebbe con qualunque altro sito.

## Perché il bridge è in un repository separato e privato

Due motivi, non uno solo:

- **Sicurezza**: la logica che parla direttamente con Windows (input
  nativo, driver del controller, gestione dei certificati) non ha motivo
  di essere pubblica, e tenerla separata riduce la superficie di cosa un
  estraneo può anche solo leggere.
- **Igiene architetturale**: il client (questa pagina) e il server
  (il bridge) evolvono con cicli diversi e non condividono segreti — la
  separazione netta tra i due repository rispecchia la stessa separazione
  che esiste già nel protocollo: la pagina non sa nulla di come il bridge
  è implementato, sa solo come parlargli.

## Stack e cosa dimostra questo progetto

Progetto personale, nato per rendere più fisica e immersiva l'esperienza
di gioco — usato come banco di prova per un'architettura "local-first":
tutto gira in casa, niente cloud, niente account, niente dati che
lasciano la rete Wi-Fi.

- **Client**: PWA vanilla (HTML/CSS/JS, nessun framework), sensori di
  movimento del browser, riconoscimento vocale via Web Speech API,
  riconoscimento gesti scritto da zero (normalizzazione e confronto di
  tracciati).
- **Bridge** (repo privato): Python asincrono (aiohttp), WebSocket,
  TLS con autorità di certificazione generata e gestita in locale,
  emulazione di un controller virtuale via driver ViGEmBus, input nativo
  Windows.
- **Ops**: un piccolo pannello di controllo web-based per accendere/
  spegnere il bridge da remoto, log e diagnostica pensati per essere letti
  da chi non è uno sviluppatore, avvio automatico e ripristino dopo un
  crash.

## Nota

Progetto amatoriale, non ufficiale e non affiliato a Avalanche Software,
Warner Bros. Games o Portkey Games. Interagisce col gioco esclusivamente
tramite input equivalenti a quelli di un controller standard — non
modifica file di gioco, non sblocca contenuti e non fornisce alcun
vantaggio competitivo.
