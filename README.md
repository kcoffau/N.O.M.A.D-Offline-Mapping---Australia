# N.O.M.A.D-Offline-Mapping---Australia
Produces on 1st of every month a PMTILES extract of Australia only

TO DOWNLOAD: REFER TO LINKS IN THE LATEST RELEASE

Below Info is for if people want to copy it and make their own PMTILE generator.

# Australia PMTiles Generator

Automatically generates monthly Australian offline map tiles for use with [Project N.O.M.A.D.](https://github.com/Crosstalk-Solutions/project-nomad) and any other [PMTiles](https://docs.protomaps.com/pmtiles/)-compatible mapping tool.

Map data is sourced from [Protomaps](https://protomaps.com/) (OpenStreetMap) and uploaded to Cloudflare R2 for free, public hosting.

-----

## How it works

1. On the 1st of each month, GitHub Actions spins up a free Ubuntu runner
1. Downloads the `pmtiles` CLI tool
1. Streams only the Australian bbox from Protomaps’ daily planet build (no full 120GB download)
1. Uploads the result to Cloudflare R2
1. Creates a GitHub Release as a changelog entry

The extracted `.pmtiles` file covers mainland Australia and Tasmania at up to zoom level 14.

-----

## Setup

### 1. Fork or create this repo

Make it **public** to use GitHub Actions for free.

### 2. Set up Cloudflare R2

1. Log in to [Cloudflare Dashboard](https://dash.cloudflare.com)
1. Go to **R2 Object Storage** → **Create bucket** (e.g. `nomad-maps`)
1. Under bucket settings, enable **Public access** and note the public URL
1. Go to **R2 → Manage R2 API tokens** → **Create API token**
- Permissions: **Object Read & Write**
- Scope: your bucket
1. Note down:
- Access Key ID
- Secret Access Key
- Your Cloudflare Account ID (found in the right sidebar of any R2 page)

### 3. Add GitHub Secrets

In your repo: **Settings → Secrets and variables → Actions → New repository secret**

|Secret name           |Value                                             |
|----------------------|--------------------------------------------------|
|`R2_ACCESS_KEY_ID`    |Your R2 API token Access Key ID                   |
|`R2_SECRET_ACCESS_KEY`|Your R2 API token Secret Access Key               |
|`R2_ACCOUNT_ID`       |Your Cloudflare Account ID                        |
|`R2_BUCKET_NAME`      |Your R2 bucket name (e.g. `nomad-maps`)           |
|`R2_PUBLIC_URL`       |Your R2 public bucket URL (e.g. `pub-xxxx.r2.dev`)|

### 4. Run it

Either wait for the 1st of next month, or go to:
**Actions → Generate Australian PMTiles → Run workflow**

-----

## Using the map in Project NOMAD

Once generated, import the map via the NOMAD Command Center using the public R2 URL:

```
https://YOUR_PUBLIC_URL/australia/australia.pmtiles
```

The `latest.json` file always reflects the most recent build:

```
https://YOUR_PUBLIC_URL/australia/latest.json
```

-----

## Coverage

|Region                       |Bbox                        |
|-----------------------------|----------------------------|
|Mainland Australia + Tasmania|`113.0, -43.7, 154.0, -10.7`|

To add states or territories individually (e.g. just NSW), duplicate the extract step in the workflow with a tighter bounding box.

-----

## License

Map data © [OpenStreetMap contributors](https://www.openstreetmap.org/copyright), licensed under [ODbL](https://opendatacommons.org/licenses/odbl/).

Workflow code: MIT
