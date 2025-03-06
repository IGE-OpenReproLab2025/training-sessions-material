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

Be sure to be in the right sub-directory in the explorer part of the cloud.


## UPLOAD BUTTON

Be sure to be in the right sub-directory in the explorer part of the cloud.


## WGET/CURL IN THE TERMINAL

Example :

```bash
wget https://ige-meom-opendap.univ-grenoble-alpes.fr/thredds/fileServer/meomopendap/extract/MEOM/eNATL60/eNATL60-BLBT02/1h/SICILe/eNATL60SICILe-BLBT02_y2009m08d29.1h_SSH.nc
```

## COPY TO IGE-CALCUL1

If you are outside the lab or via WIFI : ```ssh agalan@ige-ssh.u-ga.fr``

Connect and configure your access to ige-calcul1 following the first steps of [this tutorial](https://ige-calcul.github.io/public-docs/docs/clusters/Ige/ige-calcul1.html)

The mirror of your private-storage is located at ```/mnt/summer/openreprolab/USER_GITHUB```

Then you can upload data from your personnal computer with : ```rsync -rav MONDOSSIER  calcul1:/mnt/summer/openreprolab/USER_GITHUB/```


