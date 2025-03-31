# Uploading data for use in OpenReproLab

Depending on the origin of the data, several scenarios are possible:

## From online

Your data is on the Internet.

### From data catalogs

There are online data catalogs that allow you to load data lazily (i.e. transparently when the process is running):
- [pangeo](https://catalog.pangeo.io/browse/master/)
- [leap](https://catalog.leap.columbia.edu/)

In this case, point to the dataset in the notebook as specified on the catalog. For example:

```python
import xarray as xr

store = 'gs://leap-persistent/data-library/feedstocks/eNATL_feedstock/eNATL60-BLBT02.zarr'
ds = xr.open_dataset(store, engine='zarr', chunks={})
```

This way, all you have to do is manipulate the `ds` variable afterwards.

### From a server

If the data is on a remote server, then after configuring your SSH keys on your OpenReproLab home, run a Terminal and use the SCP or RSYNC commands:

```bash
cd
rsync <need example here>
# or
scp <need example here>
```

Launch these download commands in the right folder on the platform, remembering that your home is only 50GB in size.

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

IGE-calcul1 is a cluster accessible to anyone in the IGE

If you are outside the lab or via WIFI, you need to connect first to ige-ssh: `ssh agalan@ige-ssh.u-ga.fr`

Connect and configure your access to ige-calcul1 following the first steps of [this tutorial](https://ige-calcul.github.io/public-docs/docs/clusters/Ige/ige-calcul1.html)

A mirror of your private-storage that you have in the OpenReproLab cloud is located at `/mnt/summer/openreprolab/USER_GITHUB` on calcul1

Then you can upload data from your personnal computer with : `rsync -rav MONDOSSIER  calcul1:/mnt/summer/openreprolab/USER_GITHUB/` or from another place on calcul1 : `cp data /mnt/summer/openreprolab/USER_GITHUB/.`

Be aware that you have different identities on the cloud and on calcul1 :
  - on the cloud you are logued via your github account
  - on ige-calcul1 you are logued via your agalan account

That means that when you create a sub-directory in your private-storage on the cloud, you cannot write to it on calcul1 and conversly

We suggest that you create two sub-directories :
  - one from calcul1 : INPUTS-calcul1
  - one from the cloud : OUTPUTS-cloud

This way, in a notebook you will be able to read the files in INPUTS and write to OUTPUTS (choose the right names for your directories !)



