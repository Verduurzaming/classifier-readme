## Prefill route

Je kunt een Quickcheck 'sessie' aanmaken met voor-ingevoerde data middels een POST-request naar: `https://quickcheck.bureauverduurzamen.nl/prefill`.

De request dient een JSON body te bevatten met daarin de gegevens die je wilt doorgeven, bijvoorbeeld:

```json
{
  "meta": {
    "ReturnUrl": "https://example.com/profile/xxxxxxxxidxxxxxxxx",
    "CallbackUrl": "https://example.com/webhooks/qualification/xxxxxxxxidxxxxxxxx"
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
    "LastName": "van Test",
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
    "uuid": "sess-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "url": "https://quickcheck.bureauverduurzamen.nl/sess-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
  }
}
```

## Direct naar checkvragen voor specifieke maatregel

Je kunt de gebruiker direct naar de vragen voor een specifieke maatregel laten navigeren door de `check_interest` parameter toe te voegen aan de URL, met als waarde de naam van de maatregel. Bijvoorbeeld, om direct naar de vragen voor dak te gaan gebruik je:

```
https://quickcheck.bureauverduurzamen.nl/sess-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx?check_interest=Roof
```

## JSON velden

> Tip: PascalCase = veld, camelCase = sectie

> Opmerking: ik stel voor om bij de `ReturnUrl` en `CallbackUrl` een whitelist van toegestane domeinnamen te gebruiken.

|Sectie|Veld|Type|Opties/toelichting|
|---|---|---|---|
|||||
|**meta**|ReturnUrl|`String`|URL waarnaar de gebruiker terugverwezen wordt aan het einde van de kwalificatie.|
||CallbackUrl|`String`|URL waarnaar de resultaten gestuurd moeten worden (webhook).|
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
