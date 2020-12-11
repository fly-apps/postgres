# Postgres on Fly

Deploy a Postgres database on Fly, ready for your Fly apps.

<!---- cut here --->

# Rationale

Postgres is a useful database to have as part of your Fly installation. To make it simple to install, this example takes you through all the steps necessary. 

The resulting Postgresql installation will available over the Fly 6PN internal network to other applications and to users who have configured a Fly Wireguard VPN on their local system.

## Initialize the Fly app

Create your `fly.toml` file by importing the template from the repository. Run:

`fly init --import fly.source.toml`

You will be prompted for an app name and organization. You can hit return to generate an app name when asked.

Do make a note of the name of the app and the region that it will initially deploy to. You'll need the app name to connect to it, and more immediately, the region for the next step.

## Create a volume

Postgres needs to be able to store and persist data on disk. For that Fly has volumes which can be created in all of the Fly regions. Each volume is region-specific and is identified by name.

`fly volumes create postgres --region lhr`

Would create a 10GB volume named `postgres` in the `lhr` region. Add `--size 20` to create a 20GB volume.

## Set an initial password

When Postgres initialised the database for the first time, it will use the `POSTGRES_PASSWORD` to set the password for the `postgres` super-user. To pass that to the database, we set a Fly secret with whatever password we have chosen.

 `fly secrets set POSTGRES_PASSWORD=<SUPER-SEKRIT>`

## Deploy Postgres

We are now ready to deploy Postgresql. Run:

`fly deploy`

## Connecting to Postgres

As configured, this Postgres has no direct external connectivity.

### Other Fly Apps

When 6PN private networking is enabled for an app, the Postgres instance will be accessible on `<appname>.internal`. It will not require TLS as connections between apps within Fly are already encrypted.

### Wireguard

If you have configured Fly's Wireguard, then you can securely connect to the Postgres server using that. The address of the server will be `<appname>.internal`. For example, your first connection to your new Postgresql with psql would be:

`psql postgres://postgres:@appname.internal/`







