# Datastandaard fietsparkeren

Dit document beschrijft het dataformaat van de Datastandaard Fietsparkeren. De eerste versie is een ontwerp, gebaseerd op het SPDP-formaat, dat beoogt voor zowel bewaakte stallingen als straattellingen te kunnen worden gebruikt.

### Metadata request
| Field               | Type                | Required               | Description                                                  |
| ------------------- | ------------------- | ---------------------- | ------------------------------------------------------------ |
| timestamp           | ISO8601 timestamp   | yes                    | UTC timestamp van request                                    |
| surveyid            | string              | yes                    | Een uuid, random of eventueel samengesteld                   |

vrije velden, al naar gelang beschikbaar is:
| opdachtegever       | string              | no                     |                                                              |
| uitvoerder          | string              | no                     |                                                              |
| startdate           | ISO8601 timestamp   | no                     | Startdatum van het onderzoek                                 |
| enddate             | ISO8601 timestamp   | no                     | Einddatum van het onderzoek                                  |

### Area object
| Field                   | Type                | Required               | Description                                              |
| ----------------------- | ------------------- | ---------------------- | -------------------------------------------------------- |
| id                      | string              | yes                    | Een uuid, random of eventueel samengesteld               |
| timestamp               | ISO8601 timestamp   | yes                    | UTC timestamp, bijvoorbeeld 2020-07-02T11:14:00Z         |
| parkingCapacity         | number              | no                     | Totaal aantal plekken                                    |
| parkingCapacityTimestamp| ISO8601 timestamp   | no                     | Meetdatum aantal plekken                                 |
| sections                | Section[]           | no                     | Verzameling van section objecten                         |
| notes                   | Note object         | no                     | Notities over de meting in deze area                     |
| metadata                | string              | no                     | Vrije tekst met extra info over de area                  |
| vacantSpaces            | number              | no                     | Aantal vrije plekken                                     |
| occupiedSpaces          | number              | no                     | Aantal bezette plekken                                   |
| occupation              | Occupation[]        | no                     | Verzameling van Occupation-objecten                      |

### Note object
| Field               | Type                | Required               | Description                                                  |
| ------------------- | ------------------- | ---------------------- | ------------------------------------------------------------ |
| open                | boolean             | no                     | is deze area, bijv. een stalling geopend?                    |
| holiday             | boolean             | no                     | Vakantie?                                                    |
| event               | boolean             | no                     | Evenement (markt, kermis, ...)?                              |
| underConstruction   | boolean             | no                     | Werkzaamheden?                                               |
| remark              | string              | no                     | Vrij tekstveld                                               |
| ?                   | ?                   | no                     | Nader te bepalen                                             |

### Section object - een deelgebied van een area, bijvoorbeeld 'Dorpsstraat oneven-zijde stoep' of 'Stationsstalling 1e verdieping'
| Field               | Type                | Required               | Description                                                  |
| ------------        | ------------------- | ---------------------- | ----------------------------------------------------------   |
| id                  | string              | yes                    | Binnen area een unieke id                                    |
| space               | Space Object        | no                     |                                                              |
| units               | Unit[]              | no                     | Verzameling van voorzieningen binnen Sections-objecten       |
| parkingCapacity     | number              | no                     | Totaal aantal plekken                                        |
| vacantSpaces        | number              | no                     |                                                              |
| occupiedSpaces      | number              | no                     | Aantal bezette plekken                                       |
| occupation          | Occupation[]        | no                     | Verzameling van Occupation-objecten                          |

### Unit object - een parkeervoorziening, bijvoorbeeld een verzameling nietjes
| Field               | Type                | Required               | Description                                                  |
| ------------        | ------------------- | ---------------------- | ----------------------------------------------------------   |
| id                  | string              | no                     | Binnen unit een unieke id                                    |
| space               | Space Object        | no                     |                                                              |
| subUnits            | SubUnit[]           | no                     | Verzameling van delen van een unit, bijv. onderrek / bovenrek|
| parkingCapacity     | number              | no                     | Totaal aantal plekken                                        |
| vacantSpaces        | number              | no                     |                                                              |
| occupiedSpaces      | number              | no                     | Aantal bezette plekken                                       |
| occupation          | Occupation[]        | no                     | Verzameling van Occupation-objecten                          |

### SubUnit object - een parkeervoorziening, bijvoorbeeld een verzameling nietjes
| Field               | Type                | Required               | Description                                                  |
| ------------        | ------------------- | ---------------------- | ----------------------------------------------------------   |
| id                  | string              | no                     | Binnen unit een unieke id, bijv. 'bovenrek'                  |
| space               | Space Object        | no                     |                                                              |
| parkingCapacity     | number              | no                     | Totaal aantal plekken                                        |
| vacantSpaces        | number              | no                     |                                                              |
| occupiedSpaces      | number              | no                     | Aantal bezette plekken                                       |
| occupation          | Occupation[]        | no                     | Verzameling van Occupation-objecten                          |

### Space object - definiëring van een plek aan de hand van properties
| Field                | Type               | Required                | Description                                                   |
| -------------------- | ------------------ | ----------------------- | ------------------------------------------------------------- |
| type                 | number             | no                      | SpaceTypeID (zie onder);Tenminste 1 veld dient gegeven te zijn|
| level                | number             | no                      | 0=laag, 1=hoog                                                |
| ?                    | ?                  | no                      | Nieuw te definiëren plekeigenschappen                         |
| ?                    | ?                  | no                      | Nieuw te definiëren plekeigenschappen                         |

### Occupation object
| Field                | Type               | Required               | Description                                                  |
| -------------------- | ------------------ | ---------------------- | ------------------------------------------------------------ |
| vehicle              | Vehicle object     | no                     | Leeg betekent dat het voertuigtype niet bekend is            |
| numberOfVehicles     | number             | yes                    | Aantal gestalde voertuigen                                   |

### Vehicle object
| Field                | Type               | Required               | Description                                                  |
| -------------------- | ------------------ | ---------------------- | ------------------------------------------------------------ |
| type                 | number             | no                     | Voertuigtype; Tenminste 1 veld dient gegeven te zijn         |
| propulsion           | number             | no                     | Aandrijving                                                  |
| state                | string             | no                     | Bijv.  'wrak', 'puncture'                                    |
| accessoires          | string[]           | no                     | Verzameling asseccoires, bijv: ['voorkrat', 'achterzitje', 'fietstassen'] |
| parkstate            | string[]           | no                     | Verzameling parkeerdetails, bijv: ['naast rek', 'dubbel in nietje']       |
| owner                | number             | no                     | Owner type            |
| ?                    | ?                  | no                     | Nieuw te definiëren plekeigenschappen                        |
| ?                    | ?                  | no                     | Nieuw te definiëren plekeigenschappen                        |

---

### Voetuigeigenschappen volgens wettelijke voettuigcategorie, naar type aandrijving en naar breedte, zoals beschreven in 2019.09.24dataformaatfietstellingenv2.10_fietstypen.pdf pagina's 16 en 17

#### vehicle.type
| ID | Voertuigtype          | Omschrijving                                                                   |
| -- | --------------------- | ----------------- ------------------------------------------------------------ |
| 1  | Fiets                 | Geen kenteken en geen verzekeringsplaatje                                      |
| 2  | Snorfiets             | kentekenplaat is blauw, met een wit kader, wit opschrift en een hologram       |
| 3  | Bromfiets             | kentekenplaat is geel, met een zwart kader, zwart opschrift en een hologram    |
| 4  | Motor                 | NL geel of blauw, internationaal anders                                        |
| 5  | Gehandicaptenvoertuig | geen kenteken, wel verzekeringsplaatje                                         |
| 99 | Overig                |                                                                                |


#### vehicle.owner
| ID | Voertuigtype          | Omschrijving                                                                   |
| -- | --------------------- | ----------------- ------------------------------------------------------------ |
| 1  | Private               | Privéfiets                                                                     |
| 2  | Lease                 | Leasefiets, zoals Swap Bikes                                                   |
| 3  | Rent                  | Huurfiets, zoals OV-fiets                                                      |

#### vehicle.propulsion
| ID | Aandrijving           | Omschrijving                                                                   |
| -- | --------------------- | ----------------- ------------------------------------------------------------ |
| 1  | Spierkracht           | bv traditionele fiets of voetganger                                            |
| 2  | Elektrische hulpmotor | bv e-fiets, speed pedelec                                                      |
| 3  | Alleen elektrisch     | bv e-bromfiets                                                                 |
| 4  | Brandstof hulpmotor   | bv Sparta-met                                                                  |
| 5  | alleen brandstof      | bv traditionele bromfiets                                                      | 

#### spaceTypeIDs - indeling volgens Trajan 'Whitepaper fietsparkeerdrukonderzoek 1.0.pdf' p. 10
| ID | spaceType             |
| -- | --------------------- |
| 0  | Buiten voorziening    |
| 1  | Rek                   |
| 2  | Nietjes               |
| 3  | Vak                   |
| 4  | Gemengd vak           |
| 5  | Bromfietsvak          |
| 6  | Voor fietsenwinkel    |
| 99 | Overig                |

---
De velden vacantSpaces, occupiedSpaces en occupation zijn alleen aanwezig aan de bladeren van de boom, m.a.w.:  
* Een Area-object heeft alleen vacantSpaces, occupiedSpaces en occupation als er geen Sections zijn
* Een Section-object heeft alleen vacantSpaces, occupiedSpaces en occupation als er geen Units zijn
* Een Unit-object heeft alleen vacantSpaces, occupiedSpaces en occupation als er geen SubUnits zijn

De vacantSpaces van een Area-object kan bepaald worden door alle vacantSpaces van onderliggende Section, Units en SubUnits op te tellen
---

#### Example:

```json
{
	"id": "990214A1-69FC-4B3F-8E5C7733F97CB8CF",
	"timestamp": "2020-06-01T13:45:00",
	"parkingCapacity": 140,
	"vacantSpaces": 50,
	"sections": [
		{
			"id": "begane_grond",
			"parkingCapacity": 140,
			"vacantSpaces": 110,
			"occupation": [
				{
					"vehicle": {
						"type": 1,
					},
					"n": 70
				},
				{
					"vehicle": {
						"type": 1
					},
					"n": 18
				},
				{
					"vehicle": {
						"type": 2, 
						"propulsion": 2
					},
					"n": 2
				}
			],
			"sections": [
				{
					"parkingCapacity": 80,
					"vacantSpaces": 10,
					"space": {
						"type": 2
					},
					"occupation": [
						{
							"vehicle": {
								"type": 1,
							},
							"n": 70
						}
					]
				},
				{
					"parkingCapacity": 60,
					"vacantSpaces": 40,
					"space": {
						"type": 1,
						"level": 0
					},
					"occupation": [
						{
							"vehicle": {
								"type": 1
							},
							"n": 18
						},
						{
							"vehicle": {
								"type": 2, 
								"propulsion": 2
							},
							"n": 2
						}
					]
				},
			]
		},
		{
			"id": "verdieping_1",
			"parkingCapacity": 86,
			"vacantSpaces": 14,
			"occupation": [
				{
					"vehicle": {
						"type": 1
					},
					"n": 70
				},
				{
					"vehicle": {
						"type": 1
					},
					"n": 1
				},
				{
					"vehicle": {
						"type": 2,
						"propulsion": 2
					},
					"n": 1
				}
			],
			"sections": [
				{
					"space": {
						"type": 2,
					},
					"parkingCapacity": 80,
					"vacantSpaces": 10,
					"occupation": [
						{
							"vehicle": {
								"type": 1
							},
							"n": 70
						}
					]
				},
				{
					"spaceType": {
						"type": 1,
						"level": 0
					},
					"parkingCapacity": 6,
					"vacantSpaces": 4,
					"occupation": [
						{
							"vehicle": {
								"type": 1
							},
							"n": 1
						},
						{
							"vehicle": {
								"type": 2,
								"propulsion": 2
							},
							"n": 1
						}
					]
				}
			]
		}
	]
}
```

