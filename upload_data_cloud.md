# Data on the cloud

Multiple scenarios :

## DATA ALREADY ONLINE

For example, data from online catalogs like [pangeo](https://catalog.pangeo.io/browse/master/) or [leap](https://catalog.leap.columbia.edu/)

All you have to do is to load them directly in the notebook :

```python

import xarray as xr

store = 'gs://leap-persistent/data-library/feedstocks/eNATL_feedstock/eNATL60-BLBT02.zarr'
ds = xr.open_dataset(store, engine='zarr', chunks={})
```

## CLICK & DROP

## UPLOAD BUTTON

## WGET/CURL IN THE TERMINAL

## COPY TO IGE-CALCUL1
