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
                      /service.json
                      /<lang>/layersConfig.json
                      /<lang>/catalog.<topic>.json
    

Currently, localisation files are stored elsewhere, so it not possible to add completely new and translated layers.


# Examples

## Two topics

Only using two topics by editing the `services.json` file.

![two topics](https://github.com/procrastinatio/geoadminch_configs/raw/master/images/two_topics.png "Only two topics")

https://map.geo.admin.ch/?config_url=%2F%2Fpublic.dev.bgdi.ch%2Fconfigs%2Ftwo_topics&lang=de

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
      },

Using a reddish and italic style for labels, and a triangle instead of circle to mark station locations.

![alternate style](https://github.com/procrastinatio/geoadminch_configs/raw/master/images/meteo_alternate.png "Alternate geoJSON style")


https://map.geo.admin.ch/?topic=meteoschweiz&config_url=%2F%2Fpublic.dev.bgdi.ch%2Fconfigs%2Falternate_style&lang=de&layers=ch.meteoschweiz.messwerte-lufttemperatur-10min&bgLayer=ch.swisstopo.pixelkarte-farbe&E=2582578.60&N=1185861.80&zoom=3
