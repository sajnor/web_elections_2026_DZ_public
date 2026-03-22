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
   - `gold/zadnji.csv` (informacijsko: zadnji čas zagona)

4. **Prikaz v brskalniku**  
   `index.html` prebere gold CSV datoteke in izriše grafe.

---

## 2) Kako podatke uporablja posamezni graf

Na strani so 4 grafi:

1. **Volitve v Državni Zbor (2026)**  
   Vir: `gold/rezultati.csv`  
   Prikaz: linije po skupinah, seštevek deležev (`rezultati_vsi` in `rezultati_brez`) skozi čas.

2. **Volitve v Državni Zbor (2026) nad 4 % pragom**  
   Vir: `gold/rezultati.csv`  
   Prikaz: enako kot zgoraj, vendar uporablja stolpca `rezultati_vsi_nad4` in `rezultati_brez_nad4`.

3. **Zadnje stanje po volilnih enotah**  
   Vir: `gold/stej.csv`  
   Prikaz: stolpčni (stacked) graf po volilnih enotah (`demokrati`, `ostalo`, preostanek do `vseh`).

4. **Delež predčasnih glasov**  
   Vir: `gold/predcasno.csv`  
   Prikaz: ena linija skozi čas, izračun:  
   `oddanih_glasovnic / volilnih_upravicencev * 100`.

---

## 3) Slovar stolpcev (uporaba po grafih)

### A) `gold/rezultati.csv` (grafa 1 in 2)

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

### C) `gold/predcasno.csv` (graf 4)

| Stolpec | Pomen |
|---|---|
| `time` | Časovni žig točke na grafu. |
| `volilna_enota` | Naziv volilne enote. |
| `volilni_okraj` | Naziv volilnega okraja. |
| `volilnih_upravicencev` | Število volilnih upravičencev (osnova za delež). |
| `oddanih_glasovnic` | Število predčasno oddanih glasovnic. |

### D) `gold/zadnji.csv` (informacijsko)

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
   - spremembe linij v grafih 1, 2 in 4 skozi čas,
   - spremembe razmerij po volilnih enotah v grafu 3.

---

## 5) Kratka opomba za uporabnike

- Če takoj po osvežitvi ne vidiš spremembe, to običajno pomeni, da DVK še ni objavil novega cikla.
- Pri večjih obremenitvah je možen kratek zamik med objavo vira in prikazom na grafu.
