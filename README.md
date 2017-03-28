# README

Importable python library for geocoding (address string -> geographical location) addresses in the City of Helsinki. I wrote it to facilitate batch geocoding of a big bunch of addresses. Works by breaking down the input string for things that look like a (1) street name, (2) a house number, and (3) a letter possibly following the number. Includes a very simple fuzzy search: if the street name is not found in the address registry, we try to look for street names that are highly similar. Then tries to match the house number with a few variations.

**NB!** This is not a generic geocoder: can use only the [address registry of the greater Helsinki region](http://ptp.hel.fi/paikkatietohakemisto/?id=181) as source data for the lookups.

### Preparations
Clone this repository

    git clone git@github.com:hkotkanen/helsinki-geocoder.git
    cd helsinki-geocoder

Get a hold of the address data for lookups (available from a WFS endpoint hosted at the City of Helsinki):

    ogr2ogr -f GeoJSON osoitteet.json "WFS:http://kartta.hel.fi/ws/geoserver/avoindata/wfs" avoindata:PKS_osoiteluettelo


### Usage

    >>> from HelsinkiGeocoder import HelsinkiGeocoder

Initialize a geocoder object with the address data we downloaded earlier. This loads the entire data into memory as a dictionary.

    >>> coder = HelsinkiGeocoder('osoitteet.json')

If need be, you can also filter places using filter function:

    >>> helfilter = lambda place: place['properties']['postitoimipaikka'] == 'Helsinki'
    >>> coder = HelsinkiGeocoder('osoitteet.json', filter_func)

Function should return True for places to include and False for places to skip.

Geocode addresses by passing them as a parameter to the function geocode.

    >>> coder.geocode('kauppakartanonkatu 5')
    {'geometry': {'coordinates': [25504628.0, 6677331.0], 'type': 'Point'}, 'properties': {'postitoimipaikka': 'Helsinki', 'osoitenumero2': None, 'gatan': 'Handelshusgatan', 'staden': 'Helsingfors', 'tyyppi': 1, 'e': 25504628, 'n': 6677331, 'osoitenumero': 5, 'postinumero': '00930', 'osoitekirjain': None, 'id': 13992.0, 'gml_id': 'PKS_osoiteluettelo.fid--498474c_15191789947_-5a49', 'osoitenumero_teksti': '5', 'katunimi': 'Kauppakartanonkatu', 'kaupunki': 'Helsinki'}, 'type': 'Feature'}

    >>> coder.geocode('ei-löydettävä osoite 5')
    None
