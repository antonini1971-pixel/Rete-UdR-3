# Sito della Rete UdR 3

Sito con mappa interattiva e quadri orari della rete, generato dal GTFS. È un unico file HTML autonomo
(la libreria delle mappe Leaflet è inclusa): si apre con un doppio clic o si pubblica così com'è.

## Funzioni

- **Elenco linee per comune** (dal prefisso del codice linea: ANGN Anagni, ARTN Artena, CLFR Colleferro, CV Cave,
  FGGI Fiuggi, GNZZ Genazzano, PLN Paliano, PLST Palestrina, SCSR San Cesareo, SGNI Segni, SGRG Sgurgola,
  VLMN Valmontone, ZGRL Zagarolo; le LA sono le linee intercomunali) con ricerca per linea, comune o fermata e
  numero di corse nel giorno scelto.
- **Giorno**: si sceglie una data; linee, corse e partenze seguono i calendari.
- **Calendari**: i giorni della settimana vengono da `calendar.txt`, la validità è imposta dal **14/09/2026
  all'08/06/2027** (`VALIDITA` in testa a `build_data.py`) e si applicano le chiusure scolastiche di
  `calendar_dates.txt` (8/12, vacanze di Natale e di Pasqua, 1/5, 2/6), indicate sotto ogni quadro orario.
- **Mappa** (sfondi Esri: stradale, topografica, grigio chiaro, satellite): percorsi della linea con frecce del
  senso di marcia, capolinea P/A, varianti di percorso evidenziabili, filtro per zone, schermo intero, stampa.
- **Quadro orario** per calendario e direzione, con fermate principali o tutte; clic su una corsa per vederla
  sulla mappa, clic su una fermata per le sue partenze del giorno.
- **Vicino a me** e **Bus in viaggio** (posizioni stimate dagli orari programmati, non in tempo reale).
- Orari sempre nel formato **hh:mm**: gli orari del GTFS con i secondi (es. 06:54:30) sono troncati al minuto.

Il GTFS non indica davvero le fermate principali (`timepoint` è quasi sempre 1): sono considerate principali i
capolinea, la prima fermata in ogni comune, i nodi serviti da almeno 4 altre linee e una fermata almeno ogni 5
minuti di viaggio (parametri in testa a `build_data.py`).

I nomi delle fermate sono ripuliti (tolti i codici `# f1234`, il comune passa dal prefisso `COMUNE |` a una
colonna a parte); le fermate senza comune nel nome prendono quello della fermata con comune più vicina.

## File

- `template.html`: grafica e funzioni del sito, con i segnaposto per i dati.
- `build_data.py`: legge il GTFS e scrive il sito completo in `sito-mappe/index.html` (pubblicato online) e
  `index.html` nella radice del repo.

## Aggiornare con un nuovo GTFS

**Da GitHub**: *Add file → Upload files*, trascina il nuovo zip GTFS e fai *Commit changes* su `main`.
Il workflow *Pubblica sito mappe* rigenera e ripubblica il sito in un paio di minuti (scheda *Actions*).
Il sito usa lo zip più recente presente nella radice del repo.

**Dal computer** (serve solo Python 3):

```bash
python3 sito-mappe/build_data.py [percorso/GTFS.zip]
```
