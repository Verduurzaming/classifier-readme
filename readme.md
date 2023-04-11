# API

## Authentication

Provide your API-key via the `X-Access-Token` header. For example:
```
curl -H 'X-Access-Token: xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx' 'https://classifier.bureauverduurzamen.nl/v1/meta/measure'
```

## Routes / endpoints

### Classify building

Een woning 'klassificeren' betekenent: gegevens insturen om op basis van een klasse de mogelijkheden voor verduurzaming terug te krijgen. Op dit moment is verplicht:
* Bouwjaar (`constructionYear`, int)
* Woningtype (`houseType`, string - zie `meta/houseType` voor mogelijke waarden)

To classify a building send a `POST` request to `/v1/classify`, with a JSON body containing both `houseType` and `constructionYear`.

```
https://classifier.bureauverduurzamen.nl/v1/classify
```

CURL voorbeeld voor vrijstaand huis gebouwd in 1922:

```
curl -XPOST -H 'X-Access-Token: xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx' -d '{
 "houseType": "VRIJ",
 "constructionYear": 1922
}' 'https://classifier.bureauverduurzamen.nl/v1/classify'
```

Payload/body:

```json
{
 "houseType": "VRIJ",
 "constructionYear": 1922
}
```

Voorbeeld antwoord:
```json
{
    "id": "VRIJ-1920-1975",
    "construction_year_min": 1920,
    "construction_year_max": 1975,
    "houseType_id": "VRIJ",
    "yearly_gasconsumption": 2600,
    "yearly_energy_requirement": 2700,
    "created_at": "2022-11-23 16:32:13",
    "last_updated": "0000-00-00 00:00:00",
    "measures": [
        {
            "class_id": "VRIJ-1920-1975",
            "measure_id": "GEVEL",
            "quantity": "160.00",
            "investment_per_unit": "150.00",
            "reduction_percentage": "30.00",
            "annual_production": null,
            "available": 1,
            "name": "Gevelisolatie",
            "unit": "m2",
            "subsidy_per_unit": "38.00",
            "subsidy_minimum_quantity": "10.00",
            "subsidy_taxfree": 0
        },
        {
            "class_id": "VRIJ-1920-1975",
            "measure_id": "DAK",
            "quantity": "105.00",
            "investment_per_unit": "100.00",
            "reduction_percentage": "25.00",
            "annual_production": null,
            "available": 1,
            "name": "Dakisolatie",
            "unit": "m2",
            "subsidy_per_unit": "30.00",
            "subsidy_minimum_quantity": "20.00",
            "subsidy_taxfree": 0
        },
        {
            "class_id": "VRIJ-1920-1975",
            "measure_id": "VLOER",
            "quantity": "85.00",
            "investment_per_unit": "50.00",
            "reduction_percentage": "15.00",
            "annual_production": null,
            "available": 1,
            "name": "Vloerisolatie",
            "unit": "m2",
            "subsidy_per_unit": "11.00",
            "subsidy_minimum_quantity": "20.00",
            "subsidy_taxfree": 0
        },
        {
            "class_id": "VRIJ-1920-1975",
            "measure_id": "GLASWOON",
            "quantity": "15.00",
            "investment_per_unit": "140.00",
            "reduction_percentage": "15.00",
            "annual_production": null,
            "available": 1,
            "name": "HR++ glas plaatsen woonruimtes",
            "unit": "m2",
            "subsidy_per_unit": "53.00",
            "subsidy_minimum_quantity": "8.00",
            "subsidy_taxfree": 0
        },
        {
            "class_id": "VRIJ-1920-1975",
            "measure_id": "GLASWOON",
            "quantity": "10.00",
            "investment_per_unit": "140.00",
            "reduction_percentage": "15.00",
            "annual_production": null,
            "available": 1,
            "name": "HR++ glas plaatsen woonruimtes",
            "unit": "m2",
            "subsidy_per_unit": "53.00",
            "subsidy_minimum_quantity": "8.00",
            "subsidy_taxfree": 0
        },
        {
            "class_id": "VRIJ-1920-1975",
            "measure_id": "SOLAR",
            "quantity": "10.00",
            "investment_per_unit": "620.00",
            "reduction_percentage": null,
            "annual_production": 3200,
            "available": 1,
            "name": "Zonnepanelen plaatsen ",
            "unit": "panelen",
            "subsidy_per_unit": "0.00",
            "subsidy_minimum_quantity": "0.00",
            "subsidy_taxfree": 1
        },
        {
            "class_id": "VRIJ-1920-1975",
            "measure_id": "VENTILATIE",
            "quantity": "1.00",
            "investment_per_unit": "3000.00",
            "reduction_percentage": null,
            "annual_production": null,
            "available": 1,
            "name": "Ventilatie aanpassen",
            "unit": "systeem",
            "subsidy_per_unit": "0.00",
            "subsidy_minimum_quantity": "0.00",
            "subsidy_taxfree": 0
        },
        {
            "class_id": "VRIJ-1920-1975",
            "measure_id": "WARMTEPOMP",
            "quantity": "1.00",
            "investment_per_unit": "10000.00",
            "reduction_percentage": "25.00",
            "annual_production": null,
            "available": 1,
            "name": "Warmtepomp plaatsen",
            "unit": "stuk",
            "subsidy_per_unit": "1950.00",
            "subsidy_minimum_quantity": "0.00",
            "subsidy_taxfree": 0
        }
    ]
}
```

#### Loose houseType

Omdat niet alle systemen dezelfde API-namen gebruiken voor de woningtypen (`VRIJ`, `HALFVRIJ`, `TUSSEN`, etc) is er naast het strikte veld `houseType` ook `looseHouseType`. Dit veld kan gebruikt worden in plaats van het eerste veld, en normaliseert op basis van een aantal trefwoorden de input string.

Bijvoorbeeld: `vrijstaande woning` wordt genormaliseerd naar `VRIJ`. `2-onder-1 kap` wordt genormaliseerd naar `HALFVRIJ`. Hierdoor kunnen decentrale 'translation' functies worden vermeden. Let op dat een directe match met de id (API-naam) van een woningtype altijd boven de genormaliseerde waarde gaat. De waarde `vrij` gaat dus _fast-forward_ naar `VRIJ`.

Voorbeeld payload met `looseHouseType`:

```json
{
 "looseHouseType": "vrijstaande woning",
 "constructionYear": 1922
}
```

### Classify building and get specific measure

Naast het categoriseren van een woning en het ontvangen van alle mogelijke `measures` is er ook de mogelijkheid om een specifieke maatregelen te specificeren. Dit is nuttig om de investering en besparing te berekenen wanneer je al weet welke maatregel er toegepast gaat worden.

```
https://classifier.bureauverduurzamen.nl/v1/classify-measure
```

Body:

```json
{
 "houseType": "VRIJ",
 "constructionYear": 1922,
 "measure": "DAK"
}
```

Voorbeeld antwoord:
```json
{
    "id": "VRIJ-1920-1975",
    "construction_year_min": 1920,
    "construction_year_max": 1975,
    "houseType_id": "VRIJ",
    "yearly_gasconsumption": 2600,
    "yearly_energy_requirement": 2700,
    "created_at": "2022-11-23 16:32:13",
    "last_updated": "0000-00-00 00:00:00",
    "measure": {
        "class_id": "VRIJ-1920-1975",
        "measure_id": "DAK",
        "quantity": "105.00",
        "investment_per_unit": "100.00",
        "reduction_percentage": "25.00",
        "annual_production": null,
        "available": 1,
        "name": "Dakisolatie",
        "unit": "m2",
        "subsidy_per_unit": "30.00",
        "subsidy_minimum_quantity": "20.00",
        "subsidy_taxfree": 0
    }
}
```

#### Loose Measure

Net als bij `houseType` kan er hier ook `looseMeasure` worden meegegeven. Dit veld kan gebruikt worden in plaats van het eerste veld, en normaliseert op basis van een aantal trefwoorden de input string.

Bijvoorbeeld:
```json
{
 "houseType": "VRIJ",
 "constructionYear": 1922,
 "looseMeasure": "dakisolatie"
}
```

#### Subsidie berekening

Bij het opvragen van een specifieke maatregel is het ook mogelijk om de subsidie te berekenen. Dit kan door het veld `quantity` mee te geven én `insulationValue` of `ignoreInsulationValue` mee te geven. De subsidie wordt dan berekend op basis van de hoeveelheid _als_ de opgegeven hoeveelheid en isolatiewaarde voldoen aan de subsidievoorwaarden. Daarnaast wordt ook het maximum aantal vierkante meters voor de subsidie meegenomen.

De maximale hoeveelheid subsidie waarvoor deze situatie in aanmerking zou **kunnen** komen wordt teruggegeven in het veld `measure` > `subsidy_total`.

Wanneer `ignoreInsulationValue` wordt gebruikt zal worden aangenomen dat aan de minimaal vereiste isolatiewaarde wordt voldaan.

Bijvoorbeeld:
```json
{
 "houseType": "VRIJ",
 "constructionYear": 1922,
 "looseMeasure": "Dakisolatie",
 "quantity": 50,
 "insulationValue": 3.8
}
```

Of zonder isolatiewaarde:
```json
{
 "houseType": "VRIJ",
 "constructionYear": 1922,
 "looseMeasure": "Dakisolatie",
 "quantity": 50,
 "ignoreInsulationValue": true
}
```

Voorbeeld antwoord:
```json
{
    "id": "VRIJ-1920-1975",
    "construction_year_min": 1920,
    "construction_year_max": 1975,
    "houseType_id": "VRIJ",
    "yearly_gasconsumption": 2600,
    "yearly_energy_requirement": 2700,
    "measure": {
        "class_id": "VRIJ-1920-1975",
        "measure_id": "DAK",
        "quantity": "105.00",
        "investment_per_unit": "75.00",
        "reduction_percentage": "25.00",
        "annual_production": null,
        "available": 1,
        "name": "Dakisolatie",
        "unit": "m2",
        "subsidy_per_unit": "30.00",
        "subsidy_minimum_quantity": "20.00",
        "subsidy_maximum_quantity": "200.00",
        "minimum_insulation_value": "3.50",
        "inverted_minimum_insulation_value": 0,
        "insulation_value_label": "Rd-waarde",
        "subsidy_taxfree": 0,
        "min_reduction_percentage": "20.00",
        "max_reduction_percentage": "25.00",
        "subsidy_total": 1500
    }
}
```

#### Flat structure

Om het gemakkelijker te maken in sommige toepassingen/talen om het antwoord te verwerken is er ook de `structureFlat` optie. Wanneer in de request bij `structureFlat` de waarde `true` wordt meegegeven zal het sub-object `measure` in de parent worden 'gemerged'.

Voorbeeld antwoord met platte structuur:
```json
{
    "id": "VRIJ-1920-1975",
    "construction_year_min": 1920,
    "construction_year_max": 1975,
    "houseType_id": "VRIJ",
    "yearly_gasconsumption": 2600,
    "yearly_energy_requirement": 2700,
    "class_id": "VRIJ-1920-1975",
    "measure_id": "DAK",
    "quantity": "105.00",
    "investment_per_unit": "75.00",
    "reduction_percentage": "25.00",
    "annual_production": null,
    "available": 1,
    "name": "Dakisolatie",
    "unit": "m2",
    "subsidy_per_unit": "30.00",
    "subsidy_minimum_quantity": "20.00",
    "subsidy_maximum_quantity": "200.00",
    "minimum_insulation_value": "3.50",
    "inverted_minimum_insulation_value": 0,
    "insulation_value_label": "Rd-waarde",
    "subsidy_taxfree": 0,
    "min_reduction_percentage": "20.00",
    "max_reduction_percentage": "25.00"
}
```

### Get specific measure, without classification

Het is ook mogelijk om een specifieke maatregel op te vragen zonder dat er een classificatie wordt uitgevoerd. Daarbij is het dan ook mogelijk een berekening te maken voor de subsidie.

Voor subsidie is de werking hetzelfde als bij het classify endpoint. Er kan dus ook `ignoreInsulationValue` of `insulationValue` worden meegegeven.

Voorbeeld request:
```
https://classifier.bureauverduurzamen.nl/v1/measure
```

Body:
```json
{
 "measure": "DAK",
 "quantity": 50,
 "insulationValue": 3.8
}
```

Response:
```json
{
    "name": "Dakisolatie",
    "unit": "m2",
    "subsidy_per_unit": "30.00",
    "subsidy_minimum_quantity": "20.00",
    "subsidy_maximum_quantity": "200.00",
    "minimum_insulation_value": "3.50",
    "inverted_minimum_insulation_value": false,
    "insulation_value_label": "Rd-waarde",
    "subsidy_taxfree": false,
    "min_reduction_percentage": "20.00",
    "max_reduction_percentage": "25.00",
    "subsidy_total": 1500
}
```

### Get all houseTypes

Om een woning te classificeren heb je dus een id van `houseType` nodig om de juiste soort aan te geven. Om alle bestaande typen op te vragen kun je de volgende endpoint bevragen middels een `GET` request:

```
https://classifier.bureauverduurzamen.nl/v1/meta/houseType'
```

### Get all measures

Het is ook mogelijk om alle mogelijke maatregelen op te vragen middels een `GET` request:

```
https://classifier.bureauverduurzamen.nl/v1/meta/measure'
```
