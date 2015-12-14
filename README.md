# README

Importable python library for geocoding (address string -> geographical location) addresses in the City of Helsinki. Not a generic geocoder: can use only the [address registry of the greater Helsinki region](http://ptp.hel.fi/paikkatietohakemisto/?id=181) as source data for the lookups.

### Preparations
Clone this repository
```
git clone git:City-of-Helsinki/geocoder
cd geocoder
```

Get a hold of the address data for lookups (available from a WFS endpoint hosted at the City of Helsinki):
```
ogr2ogr -f GeoJSON osoitteet.json "WFS:http://kartta.hel.fi/ws/geoserver/avoindata/wfs" avoindata:PKS_osoiteluettelo
```

### Usage
```
>>> from HelsinkiGeocoder import HelsinkiGeocoder

#init with the address data we downloaded earlier
>>> coder = HelsinkiGeocoder('osoitteet.json')

#returns a geometry object if found, else returns None
>>> coder.geocode('kauppakartanonkatu 5')
> {'geometry': {'coordinates': [25504628.0, 6677331.0], 'type': 'Point'}, 'properties': {'postitoimipaikka': 'Helsinki', 'osoitenumero2': None, 'gatan': 'Handelshusgatan', 'staden': 'Helsingfors', 'tyyppi': 1, 'e': 25504628, 'n': 6677331, 'osoitenumero': 5, 'postinumero': '00930', 'osoitekirjain': None, 'id': 13992.0, 'gml_id': 'PKS_osoiteluettelo.fid--498474c_15191789947_-5a49', 'osoitenumero_teksti': '5', 'katunimi': 'Kauppakartanonkatu', 'kaupunki': 'Helsinki'}, 'type': 'Feature'}

>>> coder.geocode('ei-löydettävä osoite 5')
> None
```
