# lethal_heat
Small set of code files for calculate lethal heat stress from Temperature and RH

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

E.g:

```
# Create Vecellio22 object. By default, it will interpolate using
# numpy.polyfit with degree 1. You can provide a different degree.
v22 = Vecellio22(degree=1)

# Test if a set of temperatures and relative humidities are lethal.
# This will return an array of booleans. True = Lethal.
temperature = [35, 40, 41, 45]
rel_humidity = [60, 70, 80, 90]
v22.isLethal(temperature, rel_humidity)

# Plot lethal region with temperature, humidity pairs on a plot
# with the lethal region shaded.
v22.plot( tdb = temperature, rh = rel_humidity)

# Calculate booleans lazily, in chunks and in parallel
temperature = xr.open_dataset(filename_temp, chunks={'time':10})
rel_humidity = xr.open_dataset(filename_rh, chunks={'time',:10})

uncomputed = v22.map_to_data(temperature, rel_humidity)
uncomputed.to_netcdf(fp_out)

# Or straight from file...
Vecellio22.calculate_from_files( fp_temperature, 
                                 fp_humidity, fp_out )
```