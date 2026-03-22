# README_web.MD

## Namen dokumenta

Ta dokument je namenjen uporabnikom grafov na spletni strani volitev.
Fokus je izključno na:

- od kod pridejo podatki,
- kako se obdelajo,
- kako jih uporablja posamezni graf,
- kaj pomenijo stolpci (Slovar),
- kako pravilno uporabljati stran (Navodila).

---

## 1) Kako nastanejo podatki za grafe

Podatkovni tok teče po korakih:

1. **DVK API (vir podatkov)**  
   Sistem periodično bere JSON datoteke DVK za vse kombinacije volilnih enot in okrajev.

2. **Landing / Bronze / Silver (vmesna obdelava)**  
   Podatki se najprej shranijo kot surovi zapisi (landing), nato združijo (bronze) in pripravijo za analitiko (silver).

3. **Gold CSV (datoteke za prikaz grafov)**  
   Za spletno stran se pripravijo CSV datoteke:
   - `gold/rezultati.csv`
   - `gold/stej.csv`
   - `gold/predcasno.csv`
   - `gold/glasovi.csv`
   - `gold/povzetek.csv`
   - `gold/zadnji.csv` (informacijsko: zadnji čas zagona)

4. **Prikaz v brskalniku**  
   `index.html` prebere gold CSV datoteke in izriše grafe.

---

## 2) Kako podatke uporablja posamezni graf

Na strani je 5 grafov in 2 tabeli:

1. **Delež vseh glasov**  
   Vir: `gold/rezultati.csv`  
   Prikaz: linije po skupinah, seštevek deležev (`rezultati_vsi` in `rezultati_brez`) skozi čas.

2. **Delež vseh predčasnih glasov**  
   Vir: `gold/predcasno.csv`  
   Prikaz: ena linija skozi čas, izračun:  
   `oddanih_glasovnic / volilnih_upravicencev * 100`.

3. **Prešteti glasovi po Volilnih enotah (zadnje stanje)**  
   Vir: `gold/stej.csv`  
   Prikaz: stolpčni (stacked) graf po volilnih enotah (`demokrati`, `ostalo`, preostanek do `vseh`).

4. **Prejeti glasovi po Strankah (zadnje stanje)**  
   Vir: `gold/glasovi.csv`  
   Prikaz: stolpčni graf glasov po strankah, sortiran padajoče (največja stranka na levi).

5. **Delež vseh glasov (%) - nad pragom 4%**  
   Vir: `gold/glasovi.csv`  
   Prikaz: stolpčni graf deleža vseh glasov (%) po strankah.  
   Prikazane so samo stranke z deležem nad 4 %. Na vrhu vsakega stolpca je izpisana vrednost na 1 decimalko.

6. **Tabela: TOP 20 okrajev – demokrati najbolj uspešni**  
   Vir: `gold/povzetek.csv`  
   Prikaz: 20 volilnih okrajev z najvišjo uspešnostjo demokratov (glasovi / volilni upravičenci), sortirano padajoče (DESC).

7. **Tabela: TOP 20 okrajev – demokrati najmanj uspešni**  
   Vir: `gold/povzetek.csv`  
   Prikaz: 20 volilnih okrajev z najnižjo uspešnostjo demokratov, sortirano naraščajoče (ASC).

---

## 3) Slovar stolpcev (uporaba po grafih)

### A) `gold/rezultati.csv` (graf 1)

| Stolpec | Pomen |
|---|---|
| `time` | Časovni žig točke na grafu. |
| `stranka` | Naziv stranke v zapisu. |
| `skupina` | Skupina stranke (`democrats` ali `others`). |
| `rezultati_vsi` | Delež (%) z vsemi glasovi. |
| `rezultati_vsi_nad4` | Delež (%) z vsemi glasovi, pod 4 % je nastavljen na 0. |
| `rezultati_brez` | Delež (%) brez predčasnih glasov. |
| `rezultati_brez_nad4` | Delež (%) brez predčasnih, pod 4 % je nastavljen na 0. |

### B) `gold/stej.csv` (graf 3)

| Stolpec | Pomen |
|---|---|
| `time` | Časovni žig zadnjega stanja. |
| `en_naziv` | Naziv volilne enote. |
| `vseh` | Skupno število volilnih upravičencev v enoti. |
| `ostalo` | Seštevek glasov skupine `others`. |
| `demokrati` | Seštevek glasov skupine `democrats`. |

### C) `gold/predcasno.csv` (graf 2)

| Stolpec | Pomen |
|---|---|
| `time` | Časovni žig točke na grafu. |
| `volilna_enota` | Naziv volilne enote. |
| `volilni_okraj` | Naziv volilnega okraja. |
| `volilnih_upravicencev` | Število volilnih upravičencev (osnova za delež). |
| `oddanih_glasovnic` | Število predčasno oddanih glasovnic. |

### D) `gold/glasovi.csv` (grafa 4 in 5 – zamenjano zaporedje)

| Stolpec | Pomen |
|---|---|
| `stranka` | Naziv stranke. |
| `skupina` | Skupina stranke (`democrats` ali `others`). |
| `glasovi` | Skupno število glasov stranke. |

### E) `gold/povzetek.csv` (tabeli uspešnosti)

| Stolpec | Pomen |
|---|---|
| `job_timestamp` | Časovni žig zagona. |
| `enota_koda` | Koda volilne enote. |
| `enota_naziv` | Naziv volilne enote. |
| `okraj_koda` | Koda volilnega okraja. |
| `okraj_naziv` | Naziv volilnega okraja. |
| `vol_st` | Zaporedna številka volišča. |
| `volilnih_upravicencev` | Število volilnih upravičencev. |
| `kanal` | Tip volitev (`redni` ali `predčasni`). |
| `demokrati` | Seštevek glasov skupine `democrats`. |
| `ostalo` | Seštevek glasov skupine `others`. |

### F) `gold/zadnji.csv` (informacijsko)

| Stolpec | Pomen |
|---|---|
| `job_time` | Čas zadnjega zagona priprave podatkov. |

---

## 4) Navodila za uporabo

1. Odpri spletno stran:
   - **https://sajnor.github.io/web_elections_2026/**
2. Stran pusti odprto; grafi se osvežujejo samodejno.
3. DVK podatke praviloma osvežuje v približno 5-minutnem ciklu, zato za novo stanje počakaj naslednji cikel.
4. Za primerjavo trendov spremljaj:
   - spremembe linij v grafih 1 in 2 skozi čas,
   - spremembe razmerij po volilnih enotah v grafu 3,
   - glasove po strankah v grafu 4,
   - deleže glasov po strankah (nad 4 %) v grafu 5,
   - uspešnost demokratov po okrajih v tabelah 6 in 7.

---

## 5) Kratka opomba za uporabnike

- Če takoj po osvežitvi ne vidiš spremembe, to običajno pomeni, da DVK še ni objavil novega cikla.
- Pri večjih obremenitvah je možen kratek zamik med objavo vira in prikazom na grafu.
