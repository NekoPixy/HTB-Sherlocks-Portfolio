# 🛡️ Hack The Box - Sherlock: Brutus

> **Categoria:** Digital Forensics & Incident Response (DFIR) 
> **Difficoltà:** Very Easy  
> **Piattaforma:** Hack The Box  
> **Stato:** Completato (100%)

---

## 🎯 Introduzione Personale
Questo lab mi ha permesso di migliorare le mie competenze nell'analisi dei log di sistema Unix (`auth.log` e `wtmp`), focalizzandomi sulle tecniche di investigazione difensiva (Blue Team). Affrontare casi reali di compromissione aiuta a sviluppare un occhio critico per tracciare il ciclo di vita di un attacco informatico: dal brute-force iniziale fino alla persistenza e all'esecuzione di script malevoli.

---

## 🕵️ Scenario dell'Indagine
In questo scenario investigativo, un server esposto (con servizio Confluence e SSH attivo) è stato bersaglio di un attacco di forza bruta contro il servizio SSH. L'obiettivo dell'indagine è stato analizzare i log di autenticazione e i file binari di sessione per ricostruire le azioni dell'attaccante: l'accesso iniziale, l'escalation dei privilegi, le strategie di persistenza e i comandi eseguiti con privilegi amministrativi.

---

## 🛠️ Tool & Skill Applicate
* **CLI Linux / Bash:** Utilizzo di comandi di filtraggio e ricerca avanzata (`grep`, pipe, analisi dei log).
* **Analisi dei Log di Autenticazione (`auth.log`):** Tracciamento dei tentativi di accesso falliti e riusciti, degli eventi PAM e delle sessioni `systemd-logind`.
* **Analisi dei File di Sessione (`wtmp` / script Python personalizzati):** Estrazione e decodifica dei record binari storici di login per correlare i timestamp di accesso interattivo.
* **Framework MITRE ATT&CK:** Mappatura delle tecniche e sotto-tecniche di persistenza utilizzate dall'avversario.

---

## 🔍 Il Mio Approccio e la Mia Soluzione (Passo dopo Passo)
1. **Analisi Iniziale (Brute-Force):** Ho iniziato esaminando il file `auth.log` per individuare i tentativi ripetuti di accesso e isolare l'indirizzo IP di origine dell'attaccante.
2. **Identificazione Account Compromesso:** Attraverso il filtraggio dei log di autenticazione SSH, ho individuato l'account di sistema preso di mira e violato con successo durante l'ondata di tentativi.
3. **Correlazione Temporale e Sessioni (`wtmp` & `auth.log`):** Ho approfondito la differenza tra il timestamp di autenticazione di rete e l'effettivo avvio della sessione interattiva terminale, analizzando i record binari e identificando l'ID di sessione assegnato dal sistema operativo (`systemd-logind`).
4. **Individuazione della Persistenza:** Esaminando le attività successive, ho scoperto che l'attaccante aveva creato un nuovo account utente locale per assicurarsi un accesso duraturo, mappando questa attività sulla sotto-tecnica corrispondente del framework MITRE ATT&CK.
5. **Analisi dei Comandi Privilegiati (`Sudo`):** Infine, analizzando i log delle sessioni sudo dell'account di backdoor, ho tracciato il comando esatto utilizzato per scaricare risorse esterne tramite utility di rete.

---

## 📋 Riepilogo Task
* **Task 1:** Analisi dell'IP sorgente dell'attaccante — **Task Trovata**
* **Task 2:** Identificazione dell'account compromesso — **Task Trovata**
* **Task 3:** Estrazione del timestamp UTC della sessione terminale (`wtmp`) — **Task Trovata**
* **Task 4:** Rilevamento del numero di sessione assegnato dal sistema — **Task Trovata**
* **Task 5:** Identificazione dell'account di persistenza creato dall'attaccante — **Task Trovata**
* **Task 6:** Mappatura della sotto-tecnica MITRE ATT&CK (`Create Account`) — **Task Trovata**
* **Task 7:** Determinazione dell'orario di chiusura della prima sessione SSH — **Task Trovata**
* **Task 8:** Estrazione del comando completo eseguito tramite `sudo` — **Task Trovata**

---

## 💡 Key Takeaways (Cosa mi porto a casa)
Questo laboratorio ha rafforzato la mia comprensione di come i registri di sistema (`auth.log` e i log di sessione) parlino chiaramente quando si sa dove guardare. Saper correlare un indirizzo IP, un PID di processo SSH e un ID di sessione `systemd-logind` è un'abilità fondamentale per un SOC Analyst che si trova a triare un incidente in tempo reale o durante un'analisi forense post-compromissione.
