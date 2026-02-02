# AllWiev :camera:

Iskanje javno dostopnih spletnih kamer

[English docs](../README.md)

## Vsebina

- [Uporaba](#uporaba)
- [Namestitev](#namestitev)
- [Demo](#demo)

## Uporaba

- `python sfw search MJPG` : za iskanje javnih [MJPG predvajalnikov](https://github.com/jacksonliam/mjpg-streamer)

- `python sfw search webcamXP` : za iskanje javnih [webcamXP predvajalnikov](http://www.webcamxp.com/)

Program bo izpisal seznam naslovov z obliko: `ip_naslov:vrata`

Če vaš terminal podpira povezave, kliknite povezavo in jo odprite v brskalniku, sicer kopirajte povezavo in jo odprite v brskalniku.

## Namestitev

1. Klonirajte in vstopite v repozitorij:

    `git clone https://github.com/Lukifuki1/AllWiev;cd AllWiev`

2. Namestite zahteve:

    `pip install -r requirements.txt`

3. Nastavite storitev shodan:

    1. Pojdite na [shodan.io](https://shodan.io), registrirajte se in prijavite se, nato pridobite vaš API ključ

    2. Nastavite spremenljivko okolja `SHODAN_API_KEY` na vaš `API ključ`:

        `export "SHODAN_API_KEY"="<vaš API ključ>"`

Nato lahko zaženete program!

## Demo

[](https://asciinema.org/a/349819)![asciicast](https://asciinema.org/a/349819.svg)
