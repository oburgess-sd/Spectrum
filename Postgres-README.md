# Deploying postgres on kubernetes
Referenced the following link primarily: https://adamtheautomator.com/postgres-to-kubernetes/

This link mentions how a config map could possibly be used: https://www.containiq.com/post/deploy-postgres-on-kubernetes

The following commands are used to install postgres

    kubectl create namespace postgres
    kubectl apply -f local-pv.yaml -n postgres
    kubectl apply -f pv-claim.yaml -n postgres
    helm install postgresql -f values.yaml bitnami/postgresql -n postgres --set volumePermissions.enabled=true

upon creation of the postgres instance you will receive a response similar to the below:

    NAME: postgresql
    LAST DEPLOYED: Mon Feb 20 20:46:40 2023
    NAMESPACE: postgres
    STATUS: deployed
    REVISION: 1
    TEST SUITE: None
    NOTES:
    CHART NAME: postgresql
    CHART VERSION: 12.1.15
    APP VERSION: 15.2.0

    ** Please be patient while the chart is being deployed **

    PostgreSQL can be accessed via port 5432 on the following DNS names from within your cluster:

        postgresql.postgres.svc.cluster.local - Read/Write connection

    To get the password for "postgres" run:

        export POSTGRES_ADMIN_PASSWORD=$(kubectl get secret --namespace postgres postgresql -o jsonpath="{.data.postgres-password}" | base64 -d)

    To get the password for "oburgess" run:

        export POSTGRES_PASSWORD=$(kubectl get secret --namespace postgres postgresql -o jsonpath="{.data.password}" | base64 -d)

    To connect to your database run the following command:

        kubectl run postgresql-client --rm --tty -i --restart='Never' --namespace postgres --image docker.io/bitnami/postgresql:15.2.0-debian-11-r0 --env="PGPASSWORD=$POSTGRES_PASSWORD" \
        --command -- psql --host postgresql -U oburgess -d spectrum_db -p 5432

        > NOTE: If you access the container using bash, make sure that you execute "/opt/bitnami/scripts/postgresql/entrypoint.sh /bin/bash" in order to avoid the error "psql: local user with ID 1001} does not exist"

    To connect to your database from outside the cluster execute the following commands:

        kubectl port-forward --namespace postgres svc/postgresql 5432:5432 &
        PGPASSWORD="$POSTGRES_PASSWORD" psql --host 127.0.0.1 -U oburgess -d spectrum_db -p 5432

    WARNING: The configured password will be ignored on new installation in case when previous Posgresql release was deleted through the helm command. In that case, old PVC will have an old password, and setting it through helm won't take effect. Deleting persistent volumes (PVs) will solve the issue.

# Enabling the connector to obtain the CDC logs
The wal level on the server needs to be changed by running the following SQL command

    ALTER SYSTEM SET wal_level = logical;

then use kill on the pod in k9s to relaunch the pod and apply the setting

Alternative to using SQL commands
https://stackoverflow.com/questions/71214664/how-to-change-bitnami-postgresql-helm-chart-configs-ph-hba-conf-postgresql-co

# Fixing errors

    io.debezium.DebeziumException: Creation of replication slot failed  
    ERROR: could not access file "decoderbufs": No such file or directory

Solution: https://stackoverflow.com/questions/59978213/debezium-could-not-access-file-decoderbufs-using-postgres-11-with-default-plug