## Prefill route

Je kunt een Quickcheck 'sessie' aanmaken met voor-ingevoerde data middels een POST-request naar: `https://quickcheck.bureauverduurzamen.nl/prefill`.

De request dient een JSON body te bevatten met daarin de gegevens die je wilt doorgeven, bijvoorbeeld:

```json
{
  "meta": {
    "ReturnUrl": false
  },
  "interests": {
    "Interests": [
      "Roof",
      "Cavity Wall",
      "Glazing",
      "Floor"
    ]
  },
  "contact": {
    "FirstName": "Ralph",
    "LastName": "van TEST",
    "Email": "admin@bureauverduurzamen.nl",
    "Phone": "0503600322"
  },
  "address": {
    "Street": "Jeverweg",
    "StreetNumber": "2-2",
    "PostalCode": "9723JE",
    "City": "Groningen"
  },
  "ownership": {
    "PropertyOwner": true,
    "HoaMember": false,
    "HoaPermission": null,
    "HoaRepName": null,
    "HoaRepPhone": null,
    "HoaRepEmail": null
  },
  "property": {
    "ConstructionYear": 1975,
    "GasConsumption": 2000,
    "HouseType": "Twee onder één kap"
  }
}
```

Vervolgens zal de route een record aanmaken in de database en krijg je een response met de URL om de Quickcheck te starten met de juiste gegevens:
```json
{
  "status": "success",
  "data": {
    "uuid": "sess-dfb1e641-c6c6-4d2e-8629-xxxxxxxxxxxx",
    "url": "https://quickcheck.bureauverduurzamen.nl/sess-dfb1e641-c6c6-4d2e-8629-xxxxxxxxxxxx"
  }
}
```

## JSON velden

> Tip: PascalCase = veld, camelCase = sectie

|Sectie|Veld|Type|Opties/toelichting|
|---|---|---|---|
|||||
|**meta**|ReturnUrl|`String`|URL waarnaar de gebruiker terugverwezen wordt aan het einde van de kwalificatie.|
|**interests**|Interests|`Array<String>`|`Roof`|
||||`Cavity Wall`|
||||`Floor`|
||||`Glazing`|
||||*in de toekomst meer*|
|**contact**|FirstName|`String`||
||LastName|`String`||
||Email|`String`||
||Phone|`String`||
|**address**|Street|`String`||
||StreetNumber|`String`||
||PostalCode|`String`||
||City|`String`||
|**ownership**|PropertyOwner|`String`|Of de aanvrager de eigenaar is.|
||HoaMember|`String`|Of aanvrager lid is van VVE.|
||HoaPermission|`String`|Zo ja, of er toestemming is.|
||HoaRepName|`String`|Zo ja, contactgegevens van aanspreekpunt VVE.|
||HoaRepPhone|`String`||
||HoaRepEmail|`String`||
|**property**|ConstructionYear|`Integer`||
||GasConsumption|`Integer`|Verbruik per jaar in m3|
||HouseType|`String`|`Vrijstaande woning`|
||||`Geschakelde woning`|
||||`Hoekwoning`|
||||`Tussenwoning`|
||||`Twee onder één kap`|
||||`Appartement`|
||||`Bungalow`|
||||`Garage`|
||||`Anders`|