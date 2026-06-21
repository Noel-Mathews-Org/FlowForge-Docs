# Operations Runbooks

[← Back to Main Index](../README.md)

## Emergency Scenarios

### Restoring a Deleted Database
If a logical database is accidentally dropped:
1. Navigate to the Azure Portal -> Database for PostgreSQL Flexible Server.
2. Select **Restore** from the top menu.
3. Select the Point-In-Time (PITR) to restore from (up to the exact minute of deletion).
4. Restore to a new server instance.
5. Export the specific logical database from the restored server using `pg_dump`.
6. Re-import into the primary production server using `psql`.

### Rolling Back a Deployment
If a deployment introduces a critical bug:
1. Revert the commit in the `main` branch of the `FlowForge-Helm` repository.
2. ArgoCD will automatically detect the reversion and roll back the Kubernetes resources to the previous state.
3. *Note*: If the bad deployment included irreversible database migrations, a Point-In-Time database restore (above) must be performed simultaneously.
