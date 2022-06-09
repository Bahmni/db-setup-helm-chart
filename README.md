# Helm Chart for DB Setup
This is a utility chart that can be used to create the necessary databases for different applications of Bahmni.

## Usage:
In order to use this chart, you would need a MySQL instance that is running within the cluster or can be connected from the cluster.

### Running MySQL Service (Optional Step):
You can run mysql within your cluster by the following command

```shell
helm install mysql mysql --repo https://charts.bitnami.com/bitnami --set auth.rootPassword=nimda --set image.tag=5.7
```
### Running DB Setup Chart:
Now when you want to create databases, you can install this chart by the following command. Make sure to adjust the values as per your need.
```shell
helm install db-setup db-setup --repo https://bahmni.github.io/helm-charts --devel --wait --wait-for-jobs --atomic --timeout 1m \
  --set DB_HOST=mysql \
  --set DB_ROOT_USERNAME="root" \
  --set DB_ROOT_PASSWORD="nimda" \
  --set databases.openmrs.DB_NAME="openmrsdb" \
  --set databases.openmrs.USERNAME="openmrsdbuser" \
  --set databases.openmrs.PASSWORD="openmrspassword"
```
The above command will create a database named `openmrsdb`, a user named `openmrsdbuser` and grant all privileges on `openmrsdb` to `openmrsdbuser`.

If more databases are needed, the same can be added with multiple set options.

### Deleting the secrets:
It is better to remove the secrets from the cluster from a security standpoint. Once the job is completed, run the following command to delete the helm release.

```shell
helm uninstall db-setup
```
