# Format for en forespørsel

Hver forespørsel fra nettsiden lagres som ett JSON-objekt med dette formatet. Kundesvar-agenten (når telefonen ikke blir besvart), snøvarslingen og dashbordet skal bruke det samme formatet, så alt havner på ett sted.

```json
{
  "id": "F-M3K2X9",
  "opprettet": "2026-11-13T08:30:00.000Z",
  "kilde": "nettside",
  "status": "ny",
  "kunde": {
    "type": "privat",
    "navn": "Kari Nordmann",
    "telefon": "91234567",
    "adresse": "Storgata 1, 1607 Fredrikstad"
  },
  "oppdrag": {
    "type": "enkeltoppdrag",
    "areal_m2": 60,
    "stroing": false,
    "omrader": [{ "type": "Innkjørsel", "m2": 48, "kilde": "kart" }, { "type": "Terrasse", "m2": 12, "kilde": "kart" }],
    "naar": { "valg": "snarest", "dato": null },
    "ferdig_innen": "06:45",
    "melding": "Bratt innkjørsel"
  },
  "prisoverslag": {
    "belop_kr": 810,
    "herav_stroing_kr": 0,
    "enhet": "per_gang",
    "prisversjon": "modell-2026-11-13",
    "eksempelpriser": true
  },
  "oppfolging": { "ringt_tilbake": false, "notater": [] }
}
```

## Feltene

| Felt | Verdier | Brukes til |
|---|---|---|
| `kilde` | `nettside`, `telefon` (agent tok imot), `sosiale_medier` | Dashbord: hvor kundene kommer fra |
| `status` | `ny`, `bekreftet`, `utfort`, `avlyst` | Arbeidsliste og dashbord (jobbmengde) |
| `kunde.type` | `privat`, `borettslag` | Skille kundegrupper |
| `oppdrag.type` | `enkeltoppdrag`, `sesongavtale` | Pris og snøvarsling (sesongkunder varsles) |
| `oppdrag.naar.valg` | `snarest`, `i_dag`, `i_morgen`, `dato`, `neste_snofall` | Planlegging |
| `oppdrag.ferdig_innen` | `"HH:MM"` eller `null` | Når kunden drar på jobb. Morgenruta sorteres etter dette |
| `oppdrag.stroing` | `true`, `false` | Om kunden vil ha strøing |
| `oppdrag.omrader` | liste eller `null` | Hva kunden målte på kartsiden (maal.html), per område |
| `prisoverslag.prisversjon` | fritekst | Vite hvilken prisliste kunden så |
| `oppfolging.notater` | liste med `{ "tid": ISO-tid, "tekst": "..." }` | Agenten skriver ned når og hvor, og at vi ringer tilbake |

Inntekter fylles på senere når en jobb er `utfort` (faktisk betalt beløp), slik at dashbordet kan skille prisoverslag fra ekte inntekt.

## Prototype

I prototypen lagres forespørsler bare i nettleseren til den som fyller ut skjemaet. Når nettsiden settes opp på ekte, må skjemaet sende dataene til et sted du kan lese dem (for eksempel en enkel database eller e-post til deg).
