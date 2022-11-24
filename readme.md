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
    "measures": [
        {
            "class_id": "VRIJ-1920-1975",
            "measure_id": "GEVEL",
            "quantity": "160.00",
            "investment_per_unit": "150.00",
            "reduction_percentage": "30.00",
            "annual_production": null,
            "available": 1
        },
        {
            "class_id": "VRIJ-1920-1975",
            "measure_id": "DAK",
            "quantity": "105.00",
            "investment_per_unit": "100.00",
            "reduction_percentage": "25.00",
            "annual_production": null,
            "available": 1
        },
        {
            "class_id": "VRIJ-1920-1975",
            "measure_id": "VLOER",
            "quantity": "85.00",
            "investment_per_unit": "50.00",
            "reduction_percentage": "15.00",
            "annual_production": null,
            "available": 1
        },
        {
            "class_id": "VRIJ-1920-1975",
            "measure_id": "GLASWOON",
            "quantity": "15.00",
            "investment_per_unit": "140.00",
            "reduction_percentage": "15.00",
            "annual_production": null,
            "available": 1
        },
        {
            "class_id": "VRIJ-1920-1975",
            "measure_id": "GLASWOON",
            "quantity": "10.00",
            "investment_per_unit": "140.00",
            "reduction_percentage": "15.00",
            "annual_production": null,
            "available": 1
        },
        {
            "class_id": "VRIJ-1920-1975",
            "measure_id": "SOLAR",
            "quantity": "10.00",
            "investment_per_unit": "620.00",
            "reduction_percentage": null,
            "annual_production": 3200,
            "available": 1
        },
        {
            "class_id": "VRIJ-1920-1975",
            "measure_id": "VENTILATIE",
            "quantity": "1.00",
            "investment_per_unit": "3000.00",
            "reduction_percentage": null,
            "annual_production": null,
            "available": 1
        },
        {
            "class_id": "VRIJ-1920-1975",
            "measure_id": "WARMTEPOMP",
            "quantity": "1.00",
            "investment_per_unit": "10000.00",
            "reduction_percentage": "25.00",
            "annual_production": null,
            "available": 1
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