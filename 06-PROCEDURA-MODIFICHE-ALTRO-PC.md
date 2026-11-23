# Guida Operativa: Configurare e Usare Antigravity su un Altro PC

Questa procedura spiega perché sull'altro computer Antigravity ti chiedeva conferme manuali o apriva il browser interno, e come configurarlo per lavorare in modo **completamente automatico**.

---

## 1. Perché l'altro PC si comportava diversamente?

1. **Richiesta di azione manuale continua**:  
   Su quell'installazione di Antigravity i permessi di esecuzione per il terminale e i file sono impostati su modalità con conferma preventiva (*Ask / Prompt before action*).
2. **Apertura del browser interno**:  
   - **Per i link di Google Drive**: Se incolli un URL `drive.google.com`, l'agente prova ad aprirlo con il sub-agente browser integrato, scontrandosi con la schermata di login Google.
   - **Per Git (`git push`)**: Se Git sul nuovo PC non ha ancora memorizzato le credenziali GitHub (Windows Credential Manager), il sistema operativo apre una finestra browser per chiederti il login OAuth.

---

## 2. Configurazione "Una Tantum" sull'altro PC

Fai questi 4 passaggi una sola volta sull'altro computer:

### A. Impostare i permessi in Antigravity (Niente più "Azione Manuale")
1. Apri **Antigravity**.
2. Vai in **Settings** (icona ingranaggio) → **Agent / Permissions** (o **Execution**).
3. Abilita la modalità automatica per:
   - **Terminal commands**: Permetti l'esecuzione automatica (o imposta su *Auto-approve / Full Control*).
   - **File edits**: Permetti la modifica diretta dei file dell'area di lavoro.

### B. Configurare l'autenticazione GitHub automatica (Niente popup browser)
Apri PowerShell sul nuovo computer ed esegui:
```powershell
# 1. Accedi al tuo account GitHub
gh auth login
```
*(Seleziona `GitHub.com` → `HTTPS` → `Authenticate Git with your GitHub credentials? Yes` → effettua il login una volta nel browser)*.

Poi attiva l'helper credenziali per Git:
```powershell
gh auth setup-git
```
Da questo momento in poi, qualsiasi comando `git push` avverrà in background senza mai più aprire finestre di login.

### C. Installare Pillow per la generazione automatica dei PDF
Assicurati che Python abbia la libreria Pillow per compilare le foto in `sopra_menu_.pdf`:
```powershell
pip install pillow
```

### D. Clonare il repository del sito
Crea una cartella di lavoro e scarica il repository:
```powershell
git clone https://github.com/soprannomiosteria-lgtm/soprannomi-osteria-sito.git
```
In Antigravity, apri questa cartella (`soprannomi-osteria-sito`) come **Workspace principale**.

---

## 3. Il Metodo di Lavoro Corretto (Evitare il Browser Interno)

Quando vuoi aggiornare il sito o il menu:

1. **Non incollare link web di Google Drive**:  
   Scarica prima le foto o i file sul computer (oppure dentro un file `.zip`).
2. **Trascina il file ZIP o le foto direttamente nella chat di Antigravity** (oppure salvale nella cartella del progetto).
3. **Usa un prompt chiaro e diretto che indichi cosa fare**:

> *"Ti ho allegato lo zip con le 4 foto del nuovo menu. Leggi le foto, aggiorna `menu.html` con tutti i piatti e prezzi corretti, rigenera il file `sopra_menu_.pdf` unendo le 4 pagine ad alta risoluzione, e infine fai commit e git push su main per pubblicare le modifiche."*

---

## 4. Checklist di Verifica

| Azione | Risultato atteso |
|---|---|
| **Esecuzione modifiche** | L'assistente legge le immagini, aggiorna i testi e genera il PDF in locale. |
| **Git Push** | Esegue `git push origin main` silenziosamente in background. |
| **Deploy online** | In 1–2 minuti [soprannomiosteriacontemporanea.it](https://www.soprannomiosteriacontemporanea.it) è aggiornato. |
