

# Start container

```bash
docker compose up -d
```

# Check logs
```bash
docker compose logs -f
```

# Stop container
```bash
docker compose down
```


# Check for Splunk version

```bash
docker manifest inspect splunk/splunk:10.6 > /dev/null && echo exists
```
If you already have data in the volumes from 10.2, check these first:

- KV Store: In Splunk Enterprise 10.6, the KV store is now backed by PostgreSQL. Since your setup uses KV Store for CMDB data, test your apps against 10.6 before pointing it at production data. 
- Upgrade path: I didn't find a statement on whether 10.2 can upgrade directly to 10.6. The 10.6 notes say Not all Splunk apps and add-ons are compatible with Splunk Enterprise version 10.6, so read the "About upgrading to 10.6 READ THIS FIRST" page on help.splunk.com. 
- Backup: use the tar commands from my previous message before changing the version.
Version format: from 10.6 on, Splunk uses a four-segment version format (Major.Minor.Maintenance.Patch, e.g., 10.6.0.5). Tag naming on Docker Hub may differ from what you're used to. 

If you only need a fresh test instance, the volumes are empty and none of this matters.


# Upgrade flow

Change SPLUNK_VERSION in .env, then:

```bash
docker compose pull
docker compose up -d
```

