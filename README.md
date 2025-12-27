# South Africa Crime Stats

This repository contains South Africa's monthly crime statistics from January 2020 to June 2025, extracted from the quarterly reports published by SAPS. It also includes spatial data of police district boundaries, and population counts for the police districts from Census 2022.

## Sources

* [Quarterly crime statistics spreadsheets from the SAPS website](https://www.saps.gov.za/services/crimestats.php), including older files from the [Wayback Machine](https://web.archive.org/web/20250000000000*/https://www.saps.gov.za/services/crimestats.php). I used [this script](https://github.com/afrith/unhide-xlsx) to unhide the hidden sheets which contain the raw data.
* [Police district boundaries from Stats SA](https://www.statssa.gov.za/wp-content/uploads/2025/08/PoliceDistrict.zip).
* Police district population counts extracted from [StatsSA SuperWeb](https://superweb.statssa.gov.za/webapi).

## Cleaning

During the time period covered by this data a number of police stations were renamed, mostly in the Eastern Cape. I have cleaned the data to refer consistently to the new names. The changes were as follows.

| Old name | New name |
|----------|----------|
| Cradock | Nxuba |
| Dingleton | Siyathemba |
| Fort Beaufort | KwaMaqoma |
| Grahamstown | Makhanda |
| Jamestown | James Calata |
| Kirkwood | Nqweba |
| Mooiplaas | Khwenxurha |
| Mount Fletcher | Tlokoeng |
| Somerset East | KwaNojoli |

I have also cleaned some of the names for consistency, as follows.

| Original name | Cleaned name |
|----------|----------|
| Balfour TVL | Balfour MP |
| Balfour | Balfour EC |
| Heidelberg (Gp) | Heidelberg GP |
| Heidelberg(C) | Heidelberg WC |
| Hilton-Kzn | Hilton |
| Int Airp King Shaka | King Shaka Intl Airport |
| JHB Central | Johannesburg Central |
| Mayville-Kzn | Mayville |
| Middelburg Mpumalang | Middelburg MP |
| Middelburg(EC) | Middelburg EC |
| Morgenzon Transvaal | Morgenzon |
| OR Tambo Intern Airp | OR Tambo Intl Airport |
| Richmond-Kzn | Richmond KZN |
| Richmond(C) | Richmond NC |

In two cases new police stations have been established recently, but do not appear in Stats SA's police district boundaries or population counts. I have reallocated their crime incidents to the "parent" districts that the new police districts were created from.

* Phaudi (Limpopo) started reporting statistics from January 2025. I have attributed them to Matlala.
* Majola (Eastern Cape) started reporting statistics from February 2025. I have attributed them to Port St Johns.

I have cleaned the spatial data using PostGIS's [ST_CoverageClean](https://postgis.net/docs/manual-3.6/ST_CoverageClean.html) so that it forms a polygonal coverage with no gaps or overlaps.

## Files

* `police_stations.csv`: list of police districts, including name, population according to Census 2022, area in km², and codes indicating which local municipality, district municipality, and province the police station is in.
* `police_stations.gpkg`: spatial data for the boundaries of the police districts, including all the fields in the CSV.
* `crime-stats.csv`: count of crime incidents reported per police station, crime category (see next section), year and month.

## Crime categories

SAPS reports its crime statistics under the following hierarchy of crime categories.

* 17 Community reported serious Crime
  * Contact crime (Crimes against the person)
    * Murder
    * Sexual offences
      * Rape
      * Sexual assault
      * Attempted sexual offences
      * Contact sexual offences
    * Attempted murder
    * Assault with the intent to inflict grievous bodily harm
    * Common assault
    * Common robbery
    * Robbery with aggravating circumstances
      * TRIO Crime
        * Carjacking
        * Robbery at residential premises
        * Robbery at non-residential premises
      * Robbery of cash in transit
      * Bank robbery
      * Truck hijacking
  * Contact-related Crime
    * Arson
    * Malicious damage to property
  * Property-related Crime
    * Burglary at non-residential premises
    * Burglary at residential premises
    * Theft of motor vehicle and motorcycle
    * Theft out of or from motor vehicle
    * Stock-theft
  * Other serious Crime
    * All theft not mentioned elsewhere
    * Commercial crime
    * Shoplifting
* Crime detected as a result of police action
  * Illegal possession of firearms and ammunition
  * Drug-related crime
  * Driving under the influence of alcohol or drugs
  * Sexual offences detected as a result of police action
* Culpable homicide
* Public violence
* Crimen injuria
* Neglect and ill-treatment of children
* Kidnapping
* Abduction

## License

Neither SAPS nor Stats SA explicitly state terms of use for their data. In my opinion, this is public information that should be freely redistributable under the Promotion of Access to Information Act and section 32(1)(a) of the Constitution.

To the extent that I have any claim to copyright or database right in respect of the cleaning I have done, I relinquish any such rights and dedicate them to the public domain, in the terms set out in the [Open Data Commons Public Domain Dedication and License (PDDL) v1.0](https://opendatacommons.org/licenses/pddl/1-0/).