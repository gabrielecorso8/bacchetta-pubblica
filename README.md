# Bacchetta - Hogwarts Bridge (pagina client)

Solo l'interfaccia web della bacchetta (telefono), da ospitare qui con
GitHub Pages. Nessuna logica di gioco/server qui dentro: questa pagina si
collega via WebSocket al bridge che gira sul tuo PC (repository privato
separato) sulla stessa rete Wi-Fi.

Genera/aggiorna `index.html` con `wand/build_standalone_bundle.py` nel
repository principale (privato) - non modificarlo qui a mano, sostituiscilo
e ripubblica.

Per usarla: apri la pagina pubblicata, inserisci l'indirizzo del PC (IP e,
se richiesto, la porta) quando richiesto, assicurati che il telefono sia
sulla stessa Wi-Fi del PC e che il certificato del bridge sia già
installato sul telefono.
