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
UPDATE "observation"
SET resultnumbers = ARRAY["resultnumbers"[2], "resultnumbers"[1], "resultnumbers"[3]]
WHERE "observation"."id" in (select "observation"."id" from "observation" where "observation"."multidatastream_id" = 4) 
AND "observation"."id" >= 326239 
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
UPDATE "observation"
SET resultnumbers = ARRAY["resultnumbers"[2], "resultnumbers"[1], "resultnumbers"[3]]
WHERE "observation"."id" in (select "observation"."id" from "observation" where "observation"."multidatastream_id" = 5) 
AND "observation"."id" >= 24952 
AND "observation"."id" < 326237
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
UPDATE "observation"
SET resultnumbers = ARRAY["resultnumbers"[2], "resultnumbers"[1], "resultnumbers"[3]]
WHERE "observation"."id" in (select "observation"."id" from "observation" where "observation"."multidatastream_id" = 6) 
AND "observation"."id" >= 24952 
AND "observation"."id" < 326240
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
UPDATE "observation"
SET resultnumbers = ARRAY["resultnumbers"[2], "resultnumbers"[1], "resultnumbers"[3]]
WHERE "observation"."id" in (select "observation"."id" from "observation" where "observation"."multidatastream_id" = 7) 
AND "observation"."id" >= 24932 
AND "observation"."id" < 326236
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
UPDATE "observation"
SET resultnumbers = ARRAY["resultnumbers"[2], "resultnumbers"[1], "resultnumbers"[3]]
WHERE "observation"."id" in (select "observation"."id" from "observation" where "observation"."multidatastream_id" = 7) 
AND "observation"."id" >= 24988 
AND "observation"."id" < 326230
```
resultats:

https://api.geosas.fr/sensorthings/v1.0/MultiDatastreams(8)/Observations?$resultFormat=graph






























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
UPDATE "observation"
SET resultnumbers = ARRAY["resultnumbers"[2], "resultnumbers"[1], "resultnumbers"[3]]
WHERE "observation"."id" in (select "observation"."id" from "observation" where "observation"."multidatastream_id" = 7) 
AND "observation"."id" >= 24988 
AND "observation"."id" < 326230
```
resultats:

https://api.geosas.fr/sensorthings/v1.0/MultiDatastreams(8)/Observations?$resultFormat=graph

