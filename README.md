# lethal_heat
Python object for calculating lethal heat stress from Temperature and RH

## Installation
Clone this repository to somewhere you like.

In your terminal, cd into the directory. Enter:

```
pip install .
```

## Example Useage

This package just contains one object: Vecellio22(). It can be used for
determining lethal heat according to the 2022 Vecellio study.
It works by interpolating the data in that paper into a smooth curve.
Then, it takes temperature, humidity pairs and returns a boolean, or 
array of booleans.

Import lethal_heat:

```
from lethal_heat import Vecellio22
```

Create Vecellio22 object. By default, it will interpolate using numpy.polyfit with degree 1. You can provide a different degree.
```
v22 = Vecellio22(degree=1)
```

Test if a set of temperatures and relative humidities are lethal. This will return an array of booleans. True = Lethal.
```
temperature = [35, 40, 41, 45]
rel_humidity = [60, 70, 80, 90]
v22.isLethal(temperature, rel_humidity)
```


Plot lethal region with temperature, humidity pairs on a plot with the lethal region shaded. You can also just plot the lethal region.
```
v22.plot( tdb = temperature, rh = rel_humidity)
```

If you have data in netcdf, geotiff or zarr, you can calculate lethal heat lazily, in parallel and in chunks using something like:
```
temperature = xr.open_dataset(filename_temp, chunks={'time':10})
rel_humidity = xr.open_dataset(filename_rh, chunks={'time',:10})

uncomputed = v22.map_to_data(temperature, rel_humidity)
uncomputed.to_netcdf(fp_out)
```

Or alternatively, you can do it straight from file (netcdf only)
```
Vecellio22.calculate_from_files( fp_temperature, 
                                 fp_humidity, fp_out,
                                 chunks = {'time':100})
```
