# AllWiev :camera:

![AllWievBanner](./.github/AllWievBanner.png)

![PyPI - License](https://img.shields.io/pypi/l/AllWiev?style=flat-square)
[![Discord server invite](https://discordapp.com/api/guilds/974876463797006356/embed.png)](https://discord.gg/JCepvsHNqW)

[中文文档](/zh/README.md)

## Opomba
Prešel sem na novo metodo [namestitve](#Namestitev) tega programa
za boljšo izkušnjo razvijalcev in boljšo prilagodljivost.

Kot rezultat, PYPI paket `AllWiev` ni več vzdrževan in
bi moral biti smatrano za zastarelega.

btw: imamo tudi Discord kanal! [Pridruži se](https://discord.gg/JCepvsHNqW)

## Vsebina
- [Opomba](#opomba)
- [Vsebina](#vsebina)
- [Uporaba](#uporaba)
- [Namestitev](#namestitev)
- [Podpora za eksperimentalne modeli vidnega jezika](#podpora-za-eksperimentalne-modeli-vidnega-jezika)
- [Places365: Klasifikacija posnetkov na napravi](#places365-klasifikacija-posnetkov-na-napravi)
- [Grafični vmesnik](#graficni-vmesnik)
- [Demo](#demo)

## Uporaba

* `python sfw search MJPG` : za javne [MJPG streamere](https://github.com/jacksonliam/mjpg-streamer)

* `python sfw search webcamXP` : za javne [webcamXP streamere](http://www.webcamxp.com/)

* `python sfw search yawCam`: za javne [yawCam streamere](https://www.yawcam.com/)

* `python sfw search hipcam`: za javne hipcam streamere

* `python sfw search rtsp`: **NEVARNO** išče rtsp strežnike na shodanu in izvaja enumeracijo na njih, da bi poiskal pretoke

* `python sfw play {url}`: predvaja pretok kamere
  * za `rtsp://` pretoke, sfw bo predvajal v grafičnem pregledovalniku (glej `sfw/rtsp.py`)
  * za vse druge pretoke, sfw bo odprl v privzetem brskalniku.

* `python sfw search ... --gui`: prikaže preiskane kamere v mreži vizualno.

* `python sfw search --help`: za več možnosti in pomoč

Program bo izpisal seznam povezav z obliko `ip_naslov:vrata`, in opise slike pod njo.

Če vaš terminal podpira povezave, kliknite povezavo in jo odprite v brskalniku, sicer kopirajte povezavo in jo odprite v brskalniku.

## Namestitev
1. Klonirajte to repozitorij: `git clone https://github.com/Lukifuki1/AllWiev`

2. namestite requirements.txt: `pip install -r requirements.txt`

3. nastavite shodan:
   pojdite na [shodan.io](https://shodan.io), registrirajte se/prijavite in pridobite vaš API ključ

4. nastavite clarifai:
   pojdite na [clarifai.com](https://clarifai.com), registrirajte se/prijavite, ustvarite aplikacijo in pridobite vaš API ključ.
   Alternativno, uporabite lokalni [places365](#places365-klasifikacija-posnetkov-na-napravi) model.

5. nastavite geoip:
   pojdite na [geo.ipify.org](https://geo.ipify.org), registrirajte se/prijavite in pridobite vaš API ključ
   
6. Dodajte API ključe:
   1. zaženite `python sfw setup`
   2. vnesite vaše shodan, clarifai in geoip API ključe

In nato lahko [zaženete](#uporaba) program!

## Podpora za eksperimentalne modeli vidnega jezika
Delamo na podpori za generiranje opisov posnetkov
preko modelov vidnega jezika, ki lahko generirajo opise slik v naravnem jeziku,
namesto le oznak, in se lahko izvajajo na napravi.
Trenutno podpiramo `llama.cpp` osnovan `llava 7b` model.

Za uporabo, zaženite naslednje ukaze:

```shell
pip install -U --no-cache-dir llama-cpp-python@git+https://github.com/damian0815/llama-cpp-python/@4ec35390d72faba70942b9605dfcbde2bda0bdad
pip install huggingface_hub wurlitzer
python sfw search webcamXP --vllm=True --parallel=False # or any other search command with the latter two flags
```

Opomba: naša trenutna integracija VLLM je eksperimentalna in morda ne bo delovala kot pričakovano. 
Vključno z zdajšnjo podporo za vzporedni način, mora biti onemogočen za VLLM.

## Places365: Klasifikacija posnetkov na napravi
Zdaj je mogoče zagnati [Places365 model](https://github.com/CSAILVision/places365),
model prilagojen za realna mesta,
da bi dobili informacije o posnetkih kamero.

Za uporabo tega modela, morate namestiti naslednje pakete:
```bash
pip install -r requirements-places.txt
```

Nato lahko zaženete program z zastavico `--places`:
```
python sfw search MJPG --tag=False --places
```

Model bo trajal nekaj časa za prenos in bo predpomnjen za prihodnjo uporabo.

## Grafični vmesnik

Za prikaz živih pretokov vseh preiskanih kamero, preprosto dodajte možnost `--gui` k vaši ukazni vrstici.

## Demo

[![asciicast](https://asciinema.org/a/494164.svg)](https://asciinema.org/a/494164)