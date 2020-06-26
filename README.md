# Experiments with K30 Sensor data from drone platform

This GitHub repo uses [tarql](https://tarql.github.io/) to produce rdf triples from a csv produced from a drone flight. The csv has a id timestamp that is in unix epoch that needs to be converted to xsd:dateTime format. The easiest method is to use [miller](https://github.com/johnkerl/miller) to replace the $id column in the csv file with the xsd:dateTime.

To convert unix epoc time to iso 8601 time for xsd:dateTime:

```
mlr --csv put '$id = strftime($id, "%Y-%m-%dT%H:%M:%SZ") ' CO2Meter_GPS.csv
```

To produce basic set of triples using the [SOSA](https://www.w3.org/TR/vocab-ssn/) ontology. The sensor triple will be duplicated for each observation, to remove duplicate sensor use the --dedup flag with a window size larger than the number of triples.


```
tarql --dedup 10000 co2_construct.rq  CO2Meter_GPS_iso.csv
```
