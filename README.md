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


## Structure

Configurations files have the following structure

    <base url>/configs
                      /service.json
                      /<lang>/layersConfig.json
                      /<lang>/catalog.<topic>.json
    

Currently, localisation files are stored elsewhere, so it not possible to add completely new and translated layers.




https://map.geo.admin.ch/?topic=meteoschweiz&config_url=%2F%2Fpublic.dev.bgdi.ch%2Fconfigs%2Fmeteoschweiz&lang=de&layers=ch.meteoschweiz.messwerte-lufttemperatur-10min&bgLayer=ch.swisstopo.pixelkarte-farbe&E=2582578.60&N=1185861.80&zoom=3
