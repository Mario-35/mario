# Pour rennes metropole

nouvelle URL : https://api.geosas.fr/rennesmetro/v1.0/


## datastream 1

https://api.geosas.fr/lora/v1.0/MultiDatastreams(1)?$expand=Loras

```json
{
   "@iot.id":1,
   "@iot.selfLink":"https://api.geosas.fr/lora/v1.0/MultiDatastreams(1)",
   "name":"THR_Quince",
   "description":"Stream Station Campbell CS650",
   "unitOfMeasurements":[
      {
         "name":"Volumetric water content",
         "symbol":"3/m3",
         "definition":"Volumetric water content"
      },
      {
         "name":"electrical conductivity",
         "symbol":"dS/m",
         "definition":"Bulk electrical conductivity"
      },
      {
         "name":"Temperature",
         "symbol":"°",
         "definition":"Soil temperature"
      }
   ],
   "observationType":null,
   "multiObservationDataTypes":[
      "Volumetric",
      "conductivity",
      "Temperature"
   ],
   "observedArea":null,
   "phenomenonTime":"2021-09-1T14:M1:36Z/2023-01-1T18:M1:42Z",
   "resultTime":"2021-09-1T14:M1:36Z/2023-01-1T18:M1:42Z",
   "properties":null,
   "Thing@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/MultiDatastreams(1)/Thing",
   "Sensor@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/MultiDatastreams(1)/Sensor",
   "Observations@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/MultiDatastreams(1)/Observations",
   "ObservedProperties@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/MultiDatastreams(1)/ObservedProperties",
   "Loras":[
      {
         "@iot.id":1,
         "@iot.selfLink":"https://api.geosas.fr/lora/v1.0/Loras(1)",
         "name":"THR_AGRO_01C02D2D",
         "description":"Station Campbell CS650",
         "properties":null,
         "deveui":"8cf9574000002d2d",
         "Datastreams@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/Loras(1)/Datastreams",
         "MultiDatastream@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/Loras(1)/MultiDatastream",
         "Decoder@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/Loras(1)/Decoder"
      }
   ]
}
```

```sql
select return::jsonb->>'@iot.id' as id, (datas->>'data')::jsonb->>'battery' as battery, (datas->>'data')::jsonb->>'humidity' as humidity, (datas->>'data')::jsonb->>'temperature' as temperature from log_request where da```tas->>'deveui'= '8cf9574000002d2d' and method='POST' and code='201'
```
on remarque que seul la temperature est pris en compte alors qu'il y a d'autres donnees.

JE VAIS RAPIDEMENT CREER UN SCRIPT DE CORRECTION POUR REPRENDRE LES DONNEES ISSUES DES LOGS (pas trop complique)
MAIS AVANT JORRIGER l'API pour que cela ne se reproduise pas.

## datastream 2

https://api.geosas.fr/lora/v1.0/MultiDatastreams(2)?$expand=Loras

```json
{
   "@iot.id":2,
   "@iot.selfLink":"https://api.geosas.fr/lora/v1.0/MultiDatastreams(2)",
   "name":"Lora ST4_75",
   "description":"Streams Lora ST4_75",
   "unitOfMeasurements":[
      {
         "name":"Temperature",
         "symbol":"°",
         "definition":"Temperature measurement in degrees Celsius"
      },
      {
         "name":"humidity",
         "symbol":"%",
         "definition":"Measurement of humidity in the air as a percentage "
      },
      {
         "name":"Battery",
         "symbol":"%",
         "definition":"Percentage remaining battery charge measurement"
      }
   ],
   "observationType":null,
   "multiObservationDataTypes":[
      "Humidity",
      "Température",
      "Battery"
   ],
   "observedArea":null,
   "phenomenonTime":"2021-10-2T15:M2:18Z/2023-01-2T20:M2:47Z",
   "resultTime":"2021-10-2T15:M2:18Z/2023-01-2T20:M2:47Z",
   "properties":null,
   "Thing@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/MultiDatastreams(2)/Thing",
   "Sensor@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/MultiDatastreams(2)/Sensor",
   "Observations@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/MultiDatastreams(2)/Observations",
   "ObservedProperties@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/MultiDatastreams(2)/ObservedProperties",
   "Loras":[
      {
         "@iot.id":2,
         "@iot.selfLink":"https://api.geosas.fr/lora/v1.0/Loras(2)",
         "name":"ST4_75",
         "description":"Lora ST4_75",
         "properties":null,
         "deveui":"2cf7f12031500125",
         "Datastreams@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/Loras(2)/Datastreams",
         "MultiDatastream@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/Loras(2)/MultiDatastream",
         "Decoder@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/Loras(2)/Decoder"
      }
   ]
}
```

on remarque une incoherence entre multiObservationDataTypes et unitOfMeasurements

https://api.geosas.fr/lora/v1.0/MultiDatastreams(10)/Observations?$resultFormat=csv&$splitResult=all&$top=200000

| id | phenomenonTime | resultnumber | resultnumbers | resultTime | resultQuality | validTime | parameters | atastream_id | multidatastream_id | featureofinterest_id | temperature | humidity | battery |
| - | - | - | - | - | - | - | - | - | - | - | - | - | - |
| 31662 | 2021-11-17 08:54:49.000 +0100 | {0.2,13.532,NULL} | 2021-11-17 08:54:49.000 +0100 | 2 | 3 | 0.2 | 13.532 |
| 512955 | 2023-01-18 11:44:32.000 +0100 | {18.203,0.2,NULL} | 2023-01-18 11:44:32.000 +0100 | 2 | 3 | 18.203 | 0.2 |

LE CHANGEMENT DE NOM DU CAPTEUR EST STUPIDE LES DONNEES NE SUIVENT PAS ... ET C'EST NORMAL

IL VA FALOIR FUSIONNER le multiDatastream 2 et le multiDatastream 11 par chance le 11 sera a supprimer JE VAIS FAIRE DES TESTS AVANT MAIS NORMALEMENT CA DEVRAIT SE CORRIGER

```sql
SELECT *,
        "resultnumbers"[(select position from  multidatastream, jsonb_array_elements("multidatastream"."unitOfMeasurements") with ordinality arr(elem, 
position) where id = "multidatastream_id" and elem->>'name' = 'Temperature')] as "temperature",
        "resultnumbers"[(select position from  multidatastream, jsonb_array_elements("multidatastream"."unitOfMeasurements") with ordinality arr(elem, 
position) where id = "multidatastream_id" and elem->>'name' = 'humidity')] as "humidity",
        "resultnumbers"[(select position from  multidatastream, jsonb_array_elements("multidatastream"."unitOfMeasurements") with ordinality arr(elem, 
position) where id = "multidatastream_id" and elem->>'name' = 'Battery')] as "battery"
 FROM "observation"
 WHERE "observation"."id" in (select "observation"."id" from "observation" where "observation"."multidatastream_id" = 11)
ORDER BY "phenomenonTime" ASC
```


premiere correction :

remetre les id en places ;)

```sql
UPDATE "observation" SET "multidatastream_id" = 11 where "observation"."multidatastream_id" = 2 AND "observation"."id" >= 512955;
```

512955

## datastream 3

https://api.geosas.fr/lora/v1.0/MultiDatastreams(3)?$expand=Loras

```json
{
   "@iot.id":3,
   "@iot.selfLink":"https://api.geosas.fr/lora/v1.0/MultiDatastreams(3)",
   "name":"Lora ST4_50",
   "description":"Streams Lora ST4_50",
   "unitOfMeasurements":[
      {
         "name":"Temperature",
         "symbol":"°",
         "definition":"Temperature measurement in degrees Celsius"
      },
      {
         "name":"humidity",
         "symbol":"%",
         "definition":"Measurement of humidity in the air as a percentage "
      },
      {
         "name":"Battery",
         "symbol":"%",
         "definition":"Percentage remaining battery charge measurement"
      }
   ],
   "observationType":null,
   "multiObservationDataTypes":[
      "Humidity",
      "Température",
      "Battery"
   ],
   "observedArea":null,
   "phenomenonTime":"2021-10-3T11:M3:42Z/2023-01-3T20:M3:27Z",
   "resultTime":"2021-10-3T11:M3:42Z/2023-01-3T20:M3:27Z",
   "properties":null,
   "Thing@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/MultiDatastreams(3)/Thing",
   "Sensor@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/MultiDatastreams(3)/Sensor",
   "Observations@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/MultiDatastreams(3)/Observations",
   "ObservedProperties@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/MultiDatastreams(3)/ObservedProperties",
   "Loras":[
      {
         "@iot.id":3,
         "@iot.selfLink":"https://api.geosas.fr/lora/v1.0/Loras(3)",
         "name":"ST4_50",
         "description":"Lora ST4_50",
         "properties":null,
         "deveui":"2cf7f12031500113",
         "Datastreams@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/Loras(3)/Datastreams",
         "MultiDatastream@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/Loras(3)/MultiDatastream",
         "Decoder@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/Loras(3)/Decoder"
      }
   ]
}
```

on remarque une incoherence entre multiObservationDataTypes et unitOfMeasurements

https://api.geosas.fr/lora/v1.0/MultiDatastreams(5)/Observations?$resultFormat=csv&$splitResult=all&$top=200000

inversion à partir de 2021-11-10 09:59:56.000 +0100 id => 24903 MAIS les valeurs precedent cette dates sont incoherentes par rapport a celle qui suivent meme en inversant.

retour : 2022-09-05 13:45:50.000 +0200 id => 326232

borne ( >= 24903 AND < 326232)

```sql
UPDATE "observation"
SET resultnumbers = ARRAY["resultnumbers"[2], "resultnumbers"[1], "resultnumbers"[3]]
WHERE "observation"."id" in (select "observation"."id" from "observation" where "observation"."multidatastream_id" = 3) 
AND "observation"."id" >= 24903 
AND "observation"."id" < 326232;
```
resultats:

https://api.geosas.fr/sensorthings/v1.0/MultiDatastreams(3)/Observations?$resultFormat=graph



## datastream 4

https://api.geosas.fr/lora/v1.0/MultiDatastreams(4)?$expand=Loras

```json
{
   "@iot.id":4,
   "@iot.selfLink":"https://api.geosas.fr/lora/v1.0/MultiDatastreams(4)",
   "name":"Lora ST4_25",
   "description":"Streams Lora ST4_25",
   "unitOfMeasurements":[
      {
         "name":"Temperature",
         "symbol":"°",
         "definition":"Temperature measurement in degrees Celsius"
      },
      {
         "name":"humidity",
         "symbol":"%",
         "definition":"Measurement of humidity in the air as a percentage "
      },
      {
         "name":"Battery",
         "symbol":"%",
         "definition":"Percentage remaining battery charge measurement"
      }
   ],
   "observationType":null,
   "multiObservationDataTypes":[
      "Humidity",
      "Température",
      "Battery"
   ],
   "observedArea":null,
   "phenomenonTime":"2021-11-4T15:M4:20Z/2023-01-4T14:M4:26Z",
   "resultTime":"2021-11-4T15:M4:20Z/2023-01-4T14:M4:26Z",
   "properties":null,
   "Thing@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/MultiDatastreams(4)/Thing",
   "Sensor@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/MultiDatastreams(4)/Sensor",
   "Observations@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/MultiDatastreams(4)/Observations",
   "ObservedProperties@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/MultiDatastreams(4)/ObservedProperties",
   "Loras":[
      {
         "@iot.id":11,
         "@iot.selfLink":"https://api.geosas.fr/lora/v1.0/Loras(11)",
         "name":"ST4_25",
         "description":"Lora ST4_25",
         "properties":null,
         "deveui":"2cf7f1203150012a",
         "Datastreams@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/Loras(11)/Datastreams",
         "MultiDatastream@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/Loras(11)/MultiDatastream",
         "Decoder@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/Loras(11)/Decoder"
      }
   ]
}
```

on remarque une incoherence entre multiObservationDataTypes et unitOfMeasurements

https://api.geosas.fr/lora/v1.0/MultiDatastreams(5)/Observations?$resultFormat=csv&$splitResult=all

normalement les 2 reqouettes font la meme chose MAIS pour eviter tout pb lie au code de l'API : 

```sql
SELECT *,
        "resultnumbers"[(select position from  multidatastream, jsonb_array_elements("multidatastream"."unitOfMeasurements") with ordinality arr(elem, 
position) where id = "multidatastream_id" and elem->>'name' = 'Temperature')] as "temperature",
        "resultnumbers"[(select position from  multidatastream, jsonb_array_elements("multidatastream"."unitOfMeasurements") with ordinality arr(elem, 
position) where id = "multidatastream_id" and elem->>'name' = 'humidity')] as "humidity",
        "resultnumbers"[(select position from  multidatastream, jsonb_array_elements("multidatastream"."unitOfMeasurements") with ordinality arr(elem, 
position) where id = "multidatastream_id" and elem->>'name' = 'Battery')] as "battery"
 FROM "observation"
 WHERE "observation"."id" in (select "observation"."id" from "observation" where "observation"."multidatastream_id" = 4)
ORDER BY "phenomenonTime" ASC
```

Inversion : 2022-09-05 13:53:45.000 +0200 id => 326239

borne ( >= 326239)

```sql
UPDATE "observation" SET resultnumbers = ARRAY["resultnumbers"[2], "resultnumbers"[1], "resultnumbers"[3]] WHERE "observation"."id" in (select "observation"."id" from "observation" where "observation"."multidatastream_id" = 4) AND "observation"."id" >= 326239; 
```
resultats:

https://api.geosas.fr/sensorthings/v1.0/MultiDatastreams(4)/Observations?$resultFormat=graph


Arret depuis : 2023-01-17 14:33:26.000 +0100  ????

Je n'ai aucune trace dans les logs non plus :

```sql
select * from log_request where "database" = 'lora' AND datas->>'deveui' = '2cf7f1203150012a' order by id desc
```
 cela tends a croire que rennes metropole n'envoi plus cette entité .....

 TODO => créer des metriques pour tester ce genre d'arret

## datastream 5

https://api.geosas.fr/lora/v1.0/MultiDatastreams(5)?$expand=Loras

```json
{
   "@iot.id":5,
   "@iot.selfLink":"https://api.geosas.fr/lora/v1.0/MultiDatastreams(5)",
   "name":"Lora ST2_75",
   "description":"Streams Lora ST3_75",
   "unitOfMeasurements":[
      {
         "name":"Temperature",
         "symbol":"°",
         "definition":"Temperature measurement in degrees Celsius"
      },
      {
         "name":"humidity",
         "symbol":"%",
         "definition":"Measurement of humidity in the air as a percentage "
      },
      {
         "name":"Battery",
         "symbol":"%",
         "definition":"Percentage remaining battery charge measurement"
      }
   ],
   "observationType":null,
   "multiObservationDataTypes":[
      "Humidity",
      "Température",
      "Battery"
   ],
   "observedArea":null,
   "phenomenonTime":"2021-10-5T11:M5:39Z/2023-01-5T11:M5:27Z",
   "resultTime":"2021-10-5T11:M5:39Z/2023-01-5T11:M5:27Z",
   "properties":null,
   "Thing@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/MultiDatastreams(5)/Thing",
   "Sensor@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/MultiDatastreams(5)/Sensor",
   "Observations@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/MultiDatastreams(5)/Observations",
   "ObservedProperties@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/MultiDatastreams(5)/ObservedProperties",
   "Loras":[
      {
         "@iot.id":4,
         "@iot.selfLink":"https://api.geosas.fr/lora/v1.0/Loras(4)",
         "name":"ST3_75",
         "description":"Lora ST3_75",
         "properties":null,
         "deveui":"2cf7f1202520017e",
         "Datastreams@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/Loras(4)/Datastreams",
         "MultiDatastream@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/Loras(4)/MultiDatastream",
         "Decoder@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/Loras(4)/Decoder"
      }
   ]
}
```

on remarque une incoherence entre multiObservationDataTypes et unitOfMeasurements

https://api.geosas.fr/lora/v1.0/MultiDatastreams(5)/Observations?$resultFormat=csv&$splitResult=all&$top=200000

inversion à partir de 2021-11-10T08:22:02+01:00 id => 24837 MAIS les valeurs precedent cette dates sont incoherentes par rapport a celle qui suivent meme en inversant.

retour : 2022-09-05T13:50:33+02:00 id => 326237

borne ( >= 24837 AND < 326237)

```sql
UPDATE "observation" SET resultnumbers = ARRAY["resultnumbers"[2], "resultnumbers"[1], "resultnumbers"[3]] WHERE "observation"."id" in (select "observation"."id" from "observation" where "observation"."multidatastream_id" = 5) AND "observation"."id" >= 24952 AND "observation"."id" < 326237;
```
resultats:

https://api.geosas.fr/sensorthings/v1.0/MultiDatastreams(6)/Observations?$resultFormat=graph


## datastream 6

https://api.geosas.fr/lora/v1.0/MultiDatastreams(6)?$expand=Loras

```json
{
   "@iot.id":6,
   "@iot.selfLink":"https://api.geosas.fr/lora/v1.0/MultiDatastreams(6)",
   "name":"Lora ST2_50",
   "description":"Streams Lora ST3_50",
   "unitOfMeasurements":[
      {
         "name":"Temperature",
         "symbol":"°",
         "definition":"Temperature measurement in degrees Celsius"
      },
      {
         "name":"humidity",
         "symbol":"%",
         "definition":"Measurement of humidity in the air as a percentage "
      },
      {
         "name":"Battery",
         "symbol":"%",
         "definition":"Percentage remaining battery charge measurement"
      }
   ],
   "observationType":null,
   "multiObservationDataTypes":[
      "Humidity",
      "Température",
      "Battery"
   ],
   "observedArea":null,
   "phenomenonTime":"2021-10-6T11:M6:37Z/2023-01-6T13:M6:53Z",
   "resultTime":"2021-10-6T11:M6:37Z/2023-01-6T13:M6:53Z",
   "properties":null,
   "Thing@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/MultiDatastreams(6)/Thing",
   "Sensor@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/MultiDatastreams(6)/Sensor",
   "Observations@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/MultiDatastreams(6)/Observations",
   "ObservedProperties@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/MultiDatastreams(6)/ObservedProperties",
   "Loras":[
      {
         "@iot.id":5,
         "@iot.selfLink":"https://api.geosas.fr/lora/v1.0/Loras(5)",
         "name":"ST3_50",
         "description":"Lora ST3_50",
         "properties":null,
         "deveui":"2cf7f12025200178",
         "Datastreams@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/Loras(5)/Datastreams",
         "MultiDatastream@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/Loras(5)/MultiDatastream",
         "Decoder@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/Loras(5)/Decoder"
      }
   ]
}
```

on remarque une incoherence entre multiObservationDataTypes et unitOfMeasurements

https://api.geosas.fr/lora/v1.0/MultiDatastreams(6)/Observations?$resultFormat=csv&$splitResult=all&$top=200000

inversion à partir de 2021-11-10 11:10:28.000 +0100 id => 24952 MAIS les valeurs precedent cette dates sont incoherentes par rapport a celle qui suivent meme en inversant.

retour : 2022-09-05T13:50:33+02:00 id => 326240

borne ( >= 24952 AND < 326240)

```sql
UPDATE "observation" SET resultnumbers = ARRAY["resultnumbers"[2], "resultnumbers"[1], "resultnumbers"[3]] WHERE "observation"."id" in (select "observation"."id" from "observation" where "observation"."multidatastream_id" = 6) AND "observation"."id" >= 24952 AND "observation"."id" < 326240;
```
resultats:

https://api.geosas.fr/sensorthings/v1.0/MultiDatastreams(6)/Observations?$resultFormat=graph

## datastream 7

https://api.geosas.fr/lora/v1.0/MultiDatastreams(7)?$expand=Loras

```json
{
   "@iot.id":7,
   "@iot.selfLink":"https://api.geosas.fr/lora/v1.0/MultiDatastreams(7)",
   "name":"Lora ST2_25",
   "description":"Streams Lora ST3_25",
   "unitOfMeasurements":[
      {
         "name":"Temperature",
         "symbol":"°",
         "definition":"Temperature measurement in degrees Celsius"
      },
      {
         "name":"humidity",
         "symbol":"%",
         "definition":"Measurement of humidity in the air as a percentage "
      },
      {
         "name":"Battery",
         "symbol":"%",
         "definition":"Percentage remaining battery charge measurement"
      }
   ],
   "observationType":null,
   "multiObservationDataTypes":[
      "Humidity",
      "Température",
      "Battery"
   ],
   "observedArea":null,
   "phenomenonTime":"2021-10-7T11:M7:59Z/2023-01-7T16:M7:38Z",
   "resultTime":"2021-10-7T11:M7:59Z/2023-01-7T16:M7:38Z",
   "properties":null,
   "Thing@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/MultiDatastreams(7)/Thing",
   "Sensor@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/MultiDatastreams(7)/Sensor",
   "Observations@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/MultiDatastreams(7)/Observations",
   "ObservedProperties@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/MultiDatastreams(7)/ObservedProperties",
   "Loras":[
      {
         "@iot.id":6,
         "@iot.selfLink":"https://api.geosas.fr/lora/v1.0/Loras(6)",
         "name":"ST3_25",
         "description":"Lora ST3_25",
         "properties":null,
         "deveui":"2cf7f1202520013b",
         "Datastreams@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/Loras(6)/Datastreams",
         "MultiDatastream@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/Loras(6)/MultiDatastream",
         "Decoder@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/Loras(6)/Decoder"
      }
   ]
}
```

on remarque une incoherence entre multiObservationDataTypes et unitOfMeasurements

https://api.geosas.fr/lora/v1.0/MultiDatastreams(7)/Observations?$resultFormat=csv&$splitResult=all&$top=200000

inversion à partir de 2021-11-10 10:34:33.000 +0100 id => 24932 MAIS les valeurs precedent cette dates sont incoherentes par rapport a celle qui suivent meme en inversant.

retour : 2022-09-05 13:48:19.000 +0200 id => 326236

borne ( >= 24932 AND < 326236)

```sql
UPDATE "observation" SET resultnumbers = ARRAY["resultnumbers"[2], "resultnumbers"[1], "resultnumbers"[3]] WHERE "observation"."id" in (select "observation"."id" from "observation" where "observation"."multidatastream_id" = 7) AND "observation"."id" >= 24932 AND "observation"."id" < 326236;
```
resultats:

https://api.geosas.fr/sensorthings/v1.0/MultiDatastreams(7)/Observations?$resultFormat=graph

## datastream 8

https://api.geosas.fr/lora/v1.0/MultiDatastreams(8)?$expand=Loras

```json
{
   "@iot.id":8,
   "@iot.selfLink":"https://api.geosas.fr/lora/v1.0/MultiDatastreams(8)",
   "name":"Lora ST1_75",
   "description":"Streams Lora ST1_75",
   "unitOfMeasurements":[
      {
         "name":"Temperature",
         "symbol":"°",
         "definition":"Temperature measurement in degrees Celsius"
      },
      {
         "name":"humidity",
         "symbol":"%",
         "definition":"Measurement of humidity in the air as a percentage "
      },
      {
         "name":"Battery",
         "symbol":"%",
         "definition":"Percentage remaining battery charge measurement"
      }
   ],
   "observationType":null,
   "multiObservationDataTypes":[
      "Humidity",
      "Température",
      "Battery"
   ],
   "observedArea":null,
   "phenomenonTime":"2021-10-8T11:M8:33Z/2023-01-8T16:M8:04Z",
   "resultTime":"2021-10-8T11:M8:33Z/2023-01-8T16:M8:04Z",
   "properties":null,
   "Thing@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/MultiDatastreams(8)/Thing",
   "Sensor@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/MultiDatastreams(8)/Sensor",
   "Observations@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/MultiDatastreams(8)/Observations",
   "ObservedProperties@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/MultiDatastreams(8)/ObservedProperties",
   "Loras":[
      {
         "@iot.id":7,
         "@iot.selfLink":"https://api.geosas.fr/lora/v1.0/Loras(7)",
         "name":"ST1_75",
         "description":"Lora ST1_75",
         "properties":null,
         "deveui":"2cf7f1202520010a",
         "Datastreams@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/Loras(7)/Datastreams",
         "MultiDatastream@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/Loras(7)/MultiDatastream",
         "Decoder@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/Loras(7)/Decoder"
      }
   ]
}
```

on remarque une incoherence entre multiObservationDataTypes et unitOfMeasurements

https://api.geosas.fr/lora/v1.0/MultiDatastreams(8)/Observations?$resultFormat=csv&$splitResult=all&$top=200000

inversion à partir de 2021-11-10 12:03:26.000 +0100 id => 24988 MAIS les valeurs precedent cette dates sont incoherentes par rapport a celle qui suivent meme en inversant.

retour : 2022-09-05 13:40:57.000 +0200 id => 326230

borne ( >= 24988 AND < 326230)

```sql
UPDATE "observation" SET resultnumbers = ARRAY["resultnumbers"[2], "resultnumbers"[1], "resultnumbers"[3]] WHERE "observation"."id" in (select "observation"."id" from "observation" where "observation"."multidatastream_id" = 8) AND "observation"."id" >= 24988 AND "observation"."id" < 326230;
```
resultats:

https://api.geosas.fr/sensorthings/v1.0/MultiDatastreams(8)/Observations?$resultFormat=graph

## datastream 9

https://api.geosas.fr/lora/v1.0/MultiDatastreams(9)?$expand=Loras

```json
{
   "@iot.id":9,
   "@iot.selfLink":"https://api.geosas.fr/lora/v1.0/MultiDatastreams(9)",
   "name":"Lora ST1_50",
   "description":"Streams Lora ST1_50",
   "unitOfMeasurements":[
      {
         "name":"Temperature",
         "symbol":"°",
         "definition":"Temperature measurement in degrees Celsius"
      },
      {
         "name":"humidity",
         "symbol":"%",
         "definition":"Measurement of humidity in the air as a percentage "
      },
      {
         "name":"Battery",
         "symbol":"%",
         "definition":"Percentage remaining battery charge measurement"
      }
   ],
   "observationType":null,
   "multiObservationDataTypes":[
      "Humidity",
      "Température",
      "Battery"
   ],
   "observedArea":null,
   "phenomenonTime":"2021-10-9T11:M9:50Z/2023-01-9T16:M9:04Z",
   "resultTime":"2021-10-9T11:M9:50Z/2023-01-9T16:M9:04Z",
   "properties":null,
   "Thing@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/MultiDatastreams(9)/Thing",
   "Sensor@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/MultiDatastreams(9)/Sensor",
   "Observations@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/MultiDatastreams(9)/Observations",
   "ObservedProperties@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/MultiDatastreams(9)/ObservedProperties",
   "Loras":[
      {
         "@iot.id":8,
         "@iot.selfLink":"https://api.geosas.fr/lora/v1.0/Loras(8)",
         "name":"ST1_50",
         "description":"Lora ST1_50",
         "properties":null,
         "deveui":"2cf7f120252000df",
         "Datastreams@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/Loras(8)/Datastreams",
         "MultiDatastream@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/Loras(8)/MultiDatastream",
         "Decoder@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/Loras(8)/Decoder"
      }
   ]
}
```

on remarque une incoherence entre multiObservationDataTypes et unitOfMeasurements

https://api.geosas.fr/lora/v1.0/MultiDatastreams(9)/Observations?$resultFormat=csv&$splitResult=all&$top=200000

inversion à partir de 2021-11-10 12:06:00.000 +0100 +0100 id => 24990 MAIS les valeurs precedent cette dates sont incoherentes par rapport a celle qui suivent meme en inversant.

retour : 2022-09-05 13:43:18.000 +0200 id => 326231

borne ( >= 24990 AND < 326231)

```sql
UPDATE "observation" SET resultnumbers = ARRAY["resultnumbers"[2], "resultnumbers"[1], "resultnumbers"[3]] WHERE "observation"."id" in (select "observation"."id" from "observation" where "observation"."multidatastream_id" = 9) AND "observation"."id" >= 24990 AND "observation"."id" < 326231;
```
resultats:

https://api.geosas.fr/sensorthings/v1.0/MultiDatastreams(9)/Observations?$resultFormat=graph


## datastream 10

https://api.geosas.fr/lora/v1.0/MultiDatastreams(10)?$expand=Loras

```json
{
   "@iot.id":10,
   "@iot.selfLink":"https://api.geosas.fr/lora/v1.0/MultiDatastreams(10)",
   "name":"Lora ST1_25",
   "description":"Streams Lora ST1_25",
   "unitOfMeasurements":[
      {
         "name":"Temperature",
         "symbol":"°",
         "definition":"Temperature measurement in degrees Celsius"
      },
      {
         "name":"humidity",
         "symbol":"%",
         "definition":"Measurement of humidity in the air as a percentage "
      },
      {
         "name":"Battery",
         "symbol":"%",
         "definition":"Percentage remaining battery charge measurement"
      }
   ],
   "observationType":null,
   "multiObservationDataTypes":[
      "Humidity",
      "Température",
      "Battery"
   ],
   "observedArea":null,
   "phenomenonTime":"2021-10-10T11:M10:08Z/2023-01-10T18:M10:43Z",
   "resultTime":"2021-10-10T11:M10:08Z/2023-01-10T18:M10:43Z",
   "properties":null,
   "Thing@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/MultiDatastreams(10)/Thing",
   "Sensor@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/MultiDatastreams(10)/Sensor",
   "Observations@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/MultiDatastreams(10)/Observations",
   "ObservedProperties@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/MultiDatastreams(10)/ObservedProperties",
   "Loras":[
      {
         "@iot.id":9,
         "@iot.selfLink":"https://api.geosas.fr/lora/v1.0/Loras(9)",
         "name":"ST1_25",
         "description":"Lora ST1_25",
         "properties":null,
         "deveui":"2cf7f120252000c3",
         "Datastreams@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/Loras(9)/Datastreams",
         "MultiDatastream@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/Loras(9)/MultiDatastream",
         "Decoder@iot.navigationLink":"https://api.geosas.fr/lora/v1.0/Loras(9)/Decoder"
      }
   ]
}
```

on remarque une incoherence entre multiObservationDataTypes et unitOfMeasurements

https://api.geosas.fr/lora/v1.0/MultiDatastreams(10)/Observations?$resultFormat=csv&$splitResult=all&$top=200000

inversion à partir de 2021-11-10 12:00:57.000 +0100 id => 24986 MAIS les valeurs precedent cette dates sont incoherentes par rapport a celle qui suivent meme en inversant.

retour : 2022-09-05 13:46:17.000 +0200 id => 326235

borne ( >= 24986 AND < 326235)

```sql
UPDATE "observation" SET resultnumbers = ARRAY["resultnumbers"[2], "resultnumbers"[1], "resultnumbers"[3]] WHERE "observation"."id" in (select "observation"."id" from "observation" where "observation"."multidatastream_id" = 10) AND "observation"."id" >= 24986 AND "observation"."id" < 326235;
```
resultats:

https://api.geosas.fr/sensorthings/v1.0/MultiDatastreams(9)/Observations?$resultFormat=graph


# ALL SQL CORRECTIONS

```sql
UPDATE "observation" SET resultnumbers = ARRAY["resultnumbers"[2], "resultnumbers"[1], "resultnumbers"[3]] WHERE "observation"."id" in (select "observation"."id" from "observation" where "observation"."multidatastream_id" = 3) AND "observation"."id" >= 24903 AND "observation"."id" < 326232;

UPDATE "observation" SET resultnumbers = ARRAY["resultnumbers"[2], "resultnumbers"[1], "resultnumbers"[3]] WHERE "observation"."id" in (select "observation"."id" from "observation" where "observation"."multidatastream_id" = 4) AND "observation"."id" >= 326239;

UPDATE "observation" SET resultnumbers = ARRAY["resultnumbers"[2], "resultnumbers"[1], "resultnumbers"[3]] WHERE "observation"."id" in (select "observation"."id" from "observation" where "observation"."multidatastream_id" = 5) AND "observation"."id" >= 24952 AND "observation"."id" < 326237;

UPDATE "observation" SET resultnumbers = ARRAY["resultnumbers"[2], "resultnumbers"[1], "resultnumbers"[3]] WHERE "observation"."id" in (select "observation"."id" from "observation" where "observation"."multidatastream_id" = 6) AND "observation"."id" >= 24952 AND "observation"."id" < 326240;

UPDATE "observation" SET resultnumbers = ARRAY["resultnumbers"[2], "resultnumbers"[1], "resultnumbers"[3]] WHERE "observation"."id" in (select "observation"."id" from "observation" where "observation"."multidatastream_id" = 7) AND "observation"."id" >= 24932 AND "observation"."id" < 326236;

UPDATE "observation" SET resultnumbers = ARRAY["resultnumbers"[2], "resultnumbers"[1], "resultnumbers"[3]] WHERE "observation"."id" in (select "observation"."id" from "observation" where "observation"."multidatastream_id" = 8) AND "observation"."id" >= 24988 AND "observation"."id" < 326230;

UPDATE "observation" SET resultnumbers = ARRAY["resultnumbers"[2], "resultnumbers"[1], "resultnumbers"[3]] WHERE "observation"."id" in (select "observation"."id" from "observation" where "observation"."multidatastream_id" = 9) AND "observation"."id" >= 24990 AND "observation"."id" < 326231;

UPDATE "observation" SET resultnumbers = ARRAY["resultnumbers"[2], "resultnumbers"[1], "resultnumbers"[3]] WHERE "observation"."id" in (select "observation"."id" from "observation" where "observation"."multidatastream_id" = 10) AND "observation"."id" >= 24986 AND "observation"."id" < 326235;
```

