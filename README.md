#### README #####
![GPSD-OBJECTS.png](http://i.imgur.com/jm1rYT8.png)
```
GPS3 (gps3.py) is a Python 2.7-3.5 GPSD interface (http://www.catb.org/gpsd)
Defaults host='127.0.0.1', port=2947, gpsd_protocol='json'
GPS3 has two classes.
1) 'GPSDSocket' to create a socket connection and retreive the output from GPSD.
2) 'Fix' unpacks the streamed gpsd data into python dictionaries.
These dictionaries are populated from the JSON data packet sent from the GPSD.
Import           import gps3
Instantiate      gps_connection = gps3.GPSDSocket(host='192.168.0.4')
                 gps_fix = gps3.Fix()
Iterate          for new_data in gps_connection:
                     if new_data:
                        gps_fix.refresh(new_data)
Use                     print('Altitude = ',gps_fix.TPV['alt'])
                        print('Latitude = ',gps_fix.TPV['lat'])
Consult Lines 153-ff for Attribute/Key possibilities.
or http://www.catb.org/gpsd/gpsd_json.html
Run human.py; python[X] human.py [arguments] for a human experience.

```

##### human.py access demo for gps3.py, for humans at a terminal #####
```bash
me@work:~/projects/gps3$ python3 human.py --help
usage: human.py [-h] [-host HOST] [-port PORT] [-device DEVICEPATH] [-json] [-nmea]
               [-rare] [-raw] [-scaled] [-timimg] [-split24] [-pps]

optional arguments:
  -h, --help          show this help message and exit
  -host HOST          DEFAULT "127.0.0.1"
  -port PORT          DEFAULT 2947
  -json               DEFAULT JSON objects */
  -device DEVICEPATH  alternate devicepath e.g.,"-device /dev/ttyUSB4"
  -nmea               */ output in NMEA */
  -rare               */ output of packets in hex */
  -raw                */ output of raw packets */
  -scaled             */ scale output to floats */
  -timimg             */ timing information */
  -split24            */ split AIS Type 24s */
  -pps                */ enable PPS JSON */
me@work:~/projects/gps3$
```
Commandline execution without options is the same as using the DEFAULT option flags.
*0*,*1*,*2*,*3* toggle RAW, DDD, DMM, DMS, *m*,*i*,*n*,*0*, for metric, imperial, nautical, raw units

![Python3.5.png](http://i.imgur.com/hG1cFq3.png)   ![Python2.7.png](http://i.imgur.com/gUoZfHd.png)

 But wait, there's more, ...an experiment.

If we make one large unordered associative array and dump all the data pairs into one bucket, 
as long as 'time' or 'device' in their respective JSON class do not clash there is no problem.
With my $40 gps, it's not an issue.

![agps3-attributes.png](http://i.imgur.com/hXCh3aW.png)

````python
"""
agps3.py is a Python 2.7-3.5 GPSD interface (http://www.catb.org/gpsd)
Defaults host='127.0.0.1', port=2947, gpsd_protocol='json' in two classes.

1) 'GPSDSocket' creates a GPSD socket connection & request/retreive GPSD output.
2) 'Dot' unpacks the streamed gpsd data into object attribute values.

    import agps3
    gps_connection = agps3.GPSDSocket()dot = agps3.Dot()
    for new_data in gps_connection:
        if new_data:
            dot.unpack(new_data)
            print('Lat/Lon = ',dot.lat,' ', dot.lon)
            print('Altitude = ',dot.alt)

Consult Lines 146-ff for Attribute/Key possibilities.
As long as TPV'time', GST'time', ATT'time', and POLL'time' are the same,
or 'device' is the same as path throughout "she'll be right"
"""
````

#### ahuman.py a showcase demo for agps3.py ####

Simularly, toggle Lat/Lon form with '**0**', '**1**', '**2**', '**3**' for RAW, DDD, DMM, DMS

Toggle units with  '**0**', '**m**', '**i**', '**n**', for 'raw', Metric, Imperial, Nautical

Quit with '**q**' or '**^c**'

``python[X] ahuman.py --help``   for list of commandline options.

**gegps3.py and agegps3.py**

Are trivial applications that creates a 'live' kml files for Google Earth from their respective clients.  Scant documentation is in the files.
