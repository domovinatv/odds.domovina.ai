# odds.domovina.ai — *od danas do sutra*

Dokumentacija i vizualizacija dvaju društvenih problema u Hrvatskoj i prijedlog
rješenja koje služi općem dobru.

## Vizija

Pristup likvidnosti je **javna infrastruktura**, kao ceste, struja i internet:
građanin može izravno, bez posrednika koji izvlači rentu, posuditi novac od
zajednice građana za svoju investiciju, a kamata ostaje u toj zajednici.

## Misija

Riješiti dva problema koja su zapravo jedan:

1. **P2P pristup likvidnosti.** Danas novac građana (porezi isplaćeni kao plaće,
   mirovine i potpore, pa štednja) završava kao depozit u bankama koje su oko
   90 % u stranom vlasništvu. One ga plasiraju natrag istim građanima kao
   kredit, a kamatnu maržu izvlače kao dobit i dividendu.
2. **1 osoba = 1 glas.** Uz prag od 5 % po izbornoj jedinici i D'Hondta, glas
   na parlamentarnim izborima vrijedi između 0 i ~20 proporcionalnih glasova:
   lista s 5,0048 % glasova može dobiti 14/14 mandata jedinice. Taj Sabor
   4 godine upravlja cijelim proračunom i imenuje Savjet HNB-a. U bankama
   glas ima kapital (jedna dionica, jedan glas), a kad su građani pokušali
   osnovati vlastitu zadružnu banku po načelu 1 član = 1 glas (ZEF / Ebanka,
   2016.–2018.), nisu dobili odobrenje za rad.

## Dokumenti

| # | Dokument | Sadržaj |
|---|----------|---------|
| 1 | [Trenutno stanje](docs/01-trenutno-stanje.md) | Činjenični lanac korak po korak: narod → Sabor → Savjet HNB-a → licenca; porezi → proračun → depoziti → strane banke → dobit |
| 2 | [Rješenje: 1 osoba = 1 glas](docs/02-rjesenje-1-osoba-1-glas.md) | Razmjeran izborni sustav, digitalni identitet bez duplikata, provjerljivo glasanje, upravljanje javnom infrastrukturom |
| 3 | [Rješenje: P2P likvidnost](docs/03-rjesenje-p2p-likvidnost.md) | P2P banka kao javna infrastruktura: načela, slojevi, tok novca, pravni put u fazama, rizici |
| 4 | [Gradivni blokovi](docs/04-gradivni-blokovi.md) | Što već postoji u ekosustavu i kako se poopćuje |
| 5 | [Izvori](docs/05-izvori.md) | Svi izvori s datumima |
| 6 | [Iteracije](docs/06-iteracije.md) | Otvorena pitanja i dnevnik promjena po iteracijama |

## Što ovaj repo jest

Vizija i blueprint, ne implementacija. Nema koda (osim landinga u `site/`): samo markdown i Mermaid
dijagrami koji opisuju problem i rješenje. Iteriramo N puta; svaka iteracija
je zapisana u [docs/06-iteracije.md](docs/06-iteracije.md).

## Pravilo pisanja

Činjenice i stavovi su odvojeni. Svaka činjenica ima izvor u
[docs/05-izvori.md](docs/05-izvori.md). Gdje je nešto stav ili procjena, tako
piše. Brojke koje nisu provjerene označene su s **[provjeriti]**.

## Web: odds.domovina.ai

Landing s videom *od danas do sutra* (92 s) živi u [`site/`](site/): statični
`site/public/index.html` i Cloudflare Worker na računu D.O.M., s custom
domenom `odds.domovina.ai`. Video se ne poslužuje iz statičkih asseta (oni ne
podržavaju Range, pa ga Safari/iOS ne bi pustio), nego iz R2 bucketa
`odds-domovina-ai` preko `site/src/index.js`.

Video se renderira u privatnom repou `domovinatv/hyperframes-domovina-ai`
(`videos/odds-blueprint/renders/video.mp4`) i nije u gitu. Novi render:

```bash
cd site
ffmpeg -i <render>.mp4 -c copy -movflags +faststart media/od-danas-do-sutra.mp4
npx wrangler r2 object put odds-domovina-ai/od-danas-do-sutra.mp4 \
  --file media/od-danas-do-sutra.mp4 --content-type video/mp4 \
  --cache-control "public, max-age=86400" --remote
npx wrangler deploy   # samo ako se mijenja index.html, poster ili Worker
```
