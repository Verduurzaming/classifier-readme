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