Custom configurations for map.geo.admin.ch 
==========================================

The idea is from https://github.com/geoadmin/mf-geoadmin3/pull/4687

Two goals:

- Separate code and configuration
- Be able to test alternative layers and catalog configurations.


## Alternative configurations

With the help of the parameter `config_url`, alternative configurations may be loaded.

On https://map.geo.admin.ch the default configuration are stored in https://map.geo.admin.ch/configs

So, for test purpose, we may load the `test` configuration:

    https://map.geo.admin.ch?config_url=//mf-geoadmin3.dev.bgdi.ch/configs

Nota bene:
   For config to load, domain has to be `bgdi.ch` or `geo.admin.ch`. Resources 
   (GeoJSON, WMS, WMTS) referenced in these files are not restricted to these domains.


## Structure

Configurations files have the following structure

    <base url>/configs
                      /services.json
                      /<lang>/layersConfig.json
                      /<lang>/catalog.<topic>.json
    

Currently, localisation files are stored elsewhere, so it not possible to add completely new and translated layers.


### services.json

This file described which `topics` are available, which layers are selected and/or activated. This file is not translated.

    {
      "defaultBackground": "voidLayer",
      "plConfig": "https://map.geo.admin.ch/?topic=meteoschweiz&bgLayer=voidLayer&layers=ch.bafu.gefahren-basiskarte&layers_opacity=0.7&layers_visibility=true",
      "selectedLayers": [
        "ch.bafu.gefahren-basiskarte"
      ],
      "backgroundLayers": [
        "ch.swisstopo.swissimage",
        "ch.swisstopo.pixelkarte-farbe",
        "ch.swisstopo.pixelkarte-grau"
      ],
      "groupId": 1,
      "activatedLayers": [
        "ch.bafu.gefahren-basiskarte"
      ],
      "id": "meteoschweiz"
    }

### catalog.<topic>.json

This file describe a catalog for every topic listed in the `services.json` 

    "root": {
      "category": "root",
      "staging": "prod",
      "id": 15006,
      "children": [
        {
          "category": "topic",
          "staging": "prod",
          "selectedOpen": false,
          "children": [
            {
              "category": "topic",
              "staging": "prod",
              "selectedOpen": false,
              "children": [
                {
                  "category": "layer",
                  "staging": "prod",
                  "label": "Zeitreise - Kartenwerke",
                  "layerBodId": "ch.swisstopo.zeitreihen",
                  "id": 63843
                },
                {
                  "category": "layer",
                  "staging": "prod",
                  "label": "Einteilung Geologischer Atlas 25 Vector",
                  "layerBodId": "ch.swisstopo.geologie-geologischer_atlas_vector.metadata",
                  "id": 76357344
                },
                {
                  "category": "layer",
                  "staging": "prod",
                  "label": "Landeskarte 1:1 Million | LK1000",
                  "layerBodId": "ch.swisstopo.pixelkarte-farbe-pk1000.noscale",
                  "id": 63842
                },

### layersConfig.json

This files is the layer configuration. There are three layer type: `WMTS`, `WMS` and `GeoJSON`.

# Examples

## Two topics

Only using two topics by editing the `services.json` file.

![two topics](https://github.com/procrastinatio/geoadminch_configs/raw/master/images/two_topics.png "Only two topics")

Open link https://map.geo.admin.ch/?config_url=%2F%2Fpublic.dev.bgdi.ch%2Fconfigs%2Ftwo_topics&lang=de and click on `Thema wechseln`

## Alternate GeoJSON style

A GeoJSON file is fully configured in the `layersConfig.json` file. To change the style or use an alternate data source,
simply make the `styleUrl` or `geojsonUrl` point on a different location.


    "ch.meteoschweiz.messwerte-lufttemperatur-10min": {                                                                                                                                 
        "attribution": "MeteoSchweiz",
        "chargeable": false,
        "searchable": false,
        "serverLayerName": "ch.meteoschweiz.messwerte-lufttemperatur-10min",
        "attributionUrl": "ch.meteoschweiz.url",
        "tooltip": false,
        "label": "Messwerte Temperatur 2m",
        "styleUrl": "//public.dev.bgdi.ch/configs/styles/ch.meteoschweiz.messwerte-lufttemperatur-10min.json",
        "geojsonUrl": "https://data.geo.admin.ch/ch.meteoschweiz.messwerte-lufttemperatur-10min/ch.meteoschweiz.messwerte-lufttemperatur-10min_de.json",
        "highlightable": false,
        "background": false,
        "updateDelay": 300000,
        "topics": "api,ech,inspire,meteoschweiz",
        "hasLegend": true, 
        "type": "geojson",
        "timeEnabled": false 
      }

Using a reddish and italic style for labels, and a triangle instead of circle to mark station locations.

![alternate style](https://github.com/procrastinatio/geoadminch_configs/raw/master/images/meteo_alternate.png "Alternate geoJSON style")


https://map.geo.admin.ch/?topic=meteoschweiz&config_url=%2F%2Fpublic.dev.bgdi.ch%2Fconfigs%2Falternate_style&lang=de&layers=ch.meteoschweiz.messwerte-lufttemperatur-10min&bgLayer=ch.swisstopo.pixelkarte-farbe&E=2582578.60&N=1185861.80&zoom=3


Just compare with the [original style](https://map.geo.admin.ch/?topic=meteoschweiz&lang=de&layers=ch.meteoschweiz.messwerte-lufttemperatur-10min&bgLayer=ch.swisstopo.pixelkarte-farbe&E=2582506.29&N=1185883.68&zoom=3&catalogNodes=15046,15055)


See [Geoadmin Vector Styles Specification](https://github.com/geoadmin/mf-geoadmin3/blob/master/JSONSTYLES.md)


## External WMS layer



   "bln": {
       "wmsLayers": "ch.bafu.bundesinventare-bln",
       "attribution": "bafu",
       "chargeable": false,
       "searchable": false,
       "format": "png",
       "serverLayerName": "ch.bafu.bundesinventare-bln",
       "wmsUrl": "https://wms.geo.admin.ch",
       "attributionUrl": "https://www.bafu.admin.ch/bafu/de/home/themen/landschaft/fachinformationen/landschaftsqualitaet-erhalten-und-entwickeln/landschaften-von-nationaler-bedeutung/bundesinventar-der-landschaften-und-naturdenkmaeler-von-national.html",
       "tooltip": true,
       "label": "Bundesinventar der Landschaften und Naturdenkmäler",
       "singleTile": true,
       "highlightable": false,
       "background": false,
       "topics": "ech",
       "hasLegend": false,
       "type": "wms",
       "timeEnabled": false
    }


https://map.geo.admin.ch?lang=de&topic=ech&config_url=%2F%2Fpublic.dev.bgdi.ch%2Fconfigs%2Fexternal-wms&bgLayer=ch.swisstopo.pixelkarte-farbe&layers=bln
