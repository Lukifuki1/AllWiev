
# AllWiev

## Pregled

AllWiev (interno poimenovan **Scan For Webcams / `sfw`**) je Python 3 aplikacija za **odkrivanje, validacijo, obogatitev in po potrebi prikaz javno dostopnih omrežnih kamer**. Sistem združuje iskanje prek Shodana, sintezo URL‑jev glede na protokol (HTTP/MJPEG/RTSP), analizo slikovnih okvirjev, geolokacijsko obogatitev ter semantično razvrščanje scen.

Projekt je zasnovan kot **OSINT‑orodje**. Ne izvaja izkoriščanja ranljivosti, ne obhaja avtentikacije in ne uporablja grobe sile. Deluje izključno nad **javno dostopnimi viri in API‑ji**.

---

## Celotna struktura projekta

```
AllWiev-main/
├── .gitignore
├── CODE_OF_CONDUCT.md
├── LICENSE
├── README.md
├── requirements.txt
├── requirements-places.txt
│
├── .github/
│   ├── dependabot.yml
│   ├── scan-for-webcamBanner.png
│   └── workflows/
│       ├── codeql.yml
│       └── python-app.yml
│
├── .vscode/
│   └── launch.json
│
├── sfw/
│   ├── __init__.py
│   ├── __main__.py
│   ├── cli.py
│   ├── universal.py
│   ├── search.py
│   ├── cam.py
│   ├── crfi.py
│   ├── geoip.py
│   ├── gui.py
│   ├── streaming.py
│   ├── rtsp.py
│   ├── vllm.py
│   ├── windy.py
│   ├── places_mod.py
│   ├── utils.py
│   ├── tmp.py
│   ├── cams.json
│   │
│   ├── places/
│   │   ├── __init__.py
│   │   ├── wideresnet.py
│   │   ├── wideresnet18_places365.pth.tar
│   │   ├── categories_places365.txt
│   │   ├── IO_places365.txt
│   │   ├── labels_sunattribute.txt
│   │   └── W_sceneattribute_wideresnet18.npy
│   │
│   └── rtsp_dict/
│       └── tries.txt
│
└── zh/
    └── README.md
```

---

## Arhitektura in tok podatkov

1. **Odkrivanje (Discovery)**
   Modul `search.py` uporablja Shodan API za iskanje naprav na podlagi vnaprej definiranih ali prilagojenih poizvedb.

2. **Sinteza URL‑jev**
   Na osnovi tipa kamere in protokola (`cams.json`) se generirajo dejanski URL‑ji za zajem slike ali pretoka.

3. **Validacija okvirjev**
   Modul `crfi.py` preveri, ali je slika prazna (črna/bela) ali neuporabna, ter takšne rezultate izloči.

4. **Obogatitev (neobvezno)**

   * `geoip.py`: IP → država, mesto, koordinate
   * Semantična analiza:

     * Clarifai (oblačni API)
     * Places365 (lokalni CNN)
     * Lokalni LLM (`vllm.py`)

5. **Uporaba rezultatov**

   * strukturirani Python objekti
   * izvoz v JSON
   * sprotni prikaz v GUI (`gui.py` + `streaming.py`)

---

## Namestitev

### Sistemske zahteve

* Python **≥ 3.6**
* Dostop do interneta za Shodan API
* GPU ni obvezen (potreben le za Places365 ali lokalni LLM)

### Odvisnosti

```bash
pip install -r requirements.txt
```

Za semantično razvrščanje scen:

```bash
pip install -r requirements-places.txt
```

---

## API ključi

Aplikacija zahteva lokalno datoteko `sfw/.env` z naslednjimi ključi:

* `SHODAN_API_KEY`
* `CLARIFAI_API_KEY`
* `GEOIP_API_KEY`

Če datoteka ne obstaja, CLI ob prvem zagonu samodejno sproži inicialni postopek vnosa.

---

## Uporaba

### Zagon

```bash
python -m sfw
```

### Vnaprej pripravljeno iskanje

```python
CLI().search(
    preset="rtsp",
    check=True,
    tag=True,
    places=False,
    loc=True,
    parallel=True,
)
```

Podprti preseti so definirani v `cams.json`.

### Prilagojeno iskanje

```python
CLI().search_custom(
    camera_type="IP Camera",
    url_scheme="http://{ip}:{port}/snapshot.jpg",
    search_q="webcam"
)
```

### GUI način

```python
CLI().search(preset="rtsp", gui=True)
```

### Predvajanje posamezne kamere

```python
CLI().play("rtsp://...")
```

---

## Strojni vid in semantika

### Preverjanje integritete slike

`crfi.py` uporablja statistično analizo pikslov za izločanje mrtvih ali lažnih tokov.

### Places365

* Model: WideResNet‑18
* Popolnoma lokalno delovanje
* Namenjen klasifikaciji okolja (ulica, notranjost, narava, ipd.)

### Lokalni LLM

`vllm.py` omogoča opisno označevanje scen z lokalnimi modeli brez pošiljanja podatkov v oblak.

---

## Varnost in etika

* Brez obhajanja zaščit
* Brez napadov na gesla
* Brez skritih ali invazivnih tehnik
* Uporaba izključno javno indeksiranih virov

Odgovornost za uporabo nosi izključno uporabnik.

---

## Licenca

Projekt je licenciran v skladu z datoteko `LICENSE`.

---

## Stanje projekta

Koda je modularna, operativna in brez nadomestnih ali simuliranih komponent. Vsak modul ima jasno funkcijo in neposredno vlogo v produkcijskem toku.
