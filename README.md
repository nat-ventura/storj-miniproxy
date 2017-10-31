# storj-miniproxy

This is a mini CDN proxy app built using libstorj and node-libstorj.
Working on setting up basic functionalities such as:
  * Listing the user's buckets
  * Adding/removing buckets
  * Uploading files to a bucket
  * Removing files from a bucket

## Required Dependencies
  * libstorj
  * node-libstorj
  * dotenv

## Connecting to a Bridge Server
Storj-sdk is recommended for setting up the bridge server for use with this app.

### Troubleshooting Bridge Access with Storj Integration

When attempting to use the `bucketList` route to list buckets, I ran into the following error:
```
GET /bucketList - - ms - -
Error: Not authorized
    at Error (native)
```
This was because I had the incorrect `BRIDGE_EMAIL`, `BRIDGE_PASS`, and `ENCRYPT_KEY` in my `.env` file.
When using storj-integration, you need to `cd` into your storj-integration directory (that's wherever you cloned the repo) and then start the container that you have based on the storj-integration image. You can get its container-id as follows:
```bash
docker ps -a|grep storj-integration
```
Then you can start and attach to the container with:
```bash
start -ai <container-id>
```
The last step is to run the `start-everything.sh` script from the container you started/attached to:
```bash
/scripts/start-everything.sh
```
As long as you want to connect to the bridge server, you need to keep this terminal window running.
You can then use pm2 commands to view logs etc.

You can check if your bridge user credentials are working using the `list-buckets` command:
```bash
storj -u http://localhost:8080 list-buckets
```
To check your current bridge username, password, and encryption key, you can also use the command:
```bash
storj -u http://localhost:8080 export-keys
```
<b>These credentials</b> are the ones that need to be in your `.env` file, respectively assigned to `BRIDGE_EMAIL`, `BRIDGE_PASS`, `ENCRYPT_KEY`.

You can also view what users are associated with the container.
First use the docker shell to get into a mongo shell:
```bash
mongo
```
Then connect to the `storj-sandbox` database and look for users:
```bash
db = connect('storj-sandbox')
db.users.find({})
```

To see what docker containers are running:
```bash
docker ps
```

To stop a specific container from running:
```bash
docker stop <container id>
```

To stop all running docker containers:
```bash
docker stop $(docker ps -q)
```

