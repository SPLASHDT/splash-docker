# 1. Download the Software

~~~
git clone --recurse-submodules git@github.com:SPLASHDT/splash-docker.git
cd splash-docker
~~~

# 2. Setup the data storage path

The default location to store data is `/data`, about 4GB of data will be downloaded/stored.

Create this directory by running:

~~~
mkdir /data
~~~

edit docker-compose.yaml and change "source: /data" to the location of the data directory.

# 3. Downloading the static data

In addition to the daily data the digital twin relies on some models, tidal predictions (valid until December 2026) and two example datasets from notable storms.
These are supplied in a zip file which should be extracted into /data (or it's equivalent). This will create the following subdirectories:

~~~
data_inputs/dawlish_models
data_inputs/penzance_models
data_inputs/water
data_inputs/wave/no_overtopping
data_inputs/wave/storm_bert
data_inputs/wind/no_overtopping
data_inputs/wind/storm_bert
~~~

# 4. Setup the credentails file

There are no credentials stored in the Github repository. Create a file called credentials.sh in the downloader directory Add the following lines and set it to your real username, password, API key and order ID.

~~~
user="your username"
password="your password"
apikey="your API key"
order="your order ID"
~~~

# 5. Build the containers

The containers are all built using Docker Compose. Newer versions of Docker Compose use the command `docker compose`, older versions use `docker-compose`, the files available here should work on either version.

Build the frontend, backend and downloader containers:

~~~
docker-compose build
~~~

# 6. Run the Containers

Run all three containers:
~~~
docker-compose up -d
~~~

This should have the SPLASH server running on port 8050. Change the first 8050 under the frontend service if you want it to use a different port.
This can then be proxied via Apache/Nginx for web access.

# 7. Checking everything has worked

## Checking the Cron logs

The downloader will save it's logs to `/data/downloader/cron.log`. This is only accessible inside the container, to view it from outside run:

`docker exec -it splash-docker_downloader_1 cat /data/downloader/cron.log`

## Checking the data files exist and were downloaded

The wave data directory (`/data/data_inputs/wave/`) should contain 6 NetCDF files with name the naming convention:

`metoffice_wave_amm15_NWS_WAV_bYYYYMMDD_hiYYYMMDD.nc`

Where the first YYYYMMDD is always todays date (e.g. 20250312) and the second is either today or one of the next 5 day's.

The wind data directory (`/data/data_inputs/wind`) should contain 4 Grib2 files with the naming convention:

~~~
agl_wind-direction-YYYYMMDD.grib2
agl_wind-speed-YYYYMMDD.grib2
~~~

There should be one with today's date and one with yesterday's date.

# 8. Stopping the docker containers

In the splash-docker directory run:

~~~
docker-compose stop
~~~
