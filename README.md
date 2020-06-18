# SmartNote

SmartNote is an app to track internal documentation, starting with FAQs around development.

## Setup

First install these:

- [asdf](https://asdf-vm.com/#/)
- [PostgreSQL 12](https://www.postgresql.org/)

Once those are installed you can setup the application.

```bash
# This installs elixir/erlang/nodejs at the versions required
asdf install

# Install elixir dependencies
mix deps.get
mix compile

# Install yarn and the rest of the javascript dependencies
npm install -g yarn
cd assets && yarn install && cd ..

# Create the database and migrate for dev & test
mix ecto.create
mix ecto.migrate
MIX_ENV=test mix ecto.migrate.reset

# Make sure tests pass
mix test

# See `Local Development` below to configure `.env.dev`
# Once complete, run the server
mix phx.server
```

## Local Development

In order to run locally, you'll need a `.env.dev` file with the following filled in:

```
GITHUB_ALLOWED_ORGANIZATIONS=comma,separated,list,of,github,orgs
GITHUB_CLIENT_ID=...
GITHUB_CLIENT_SECRET=...
```

Create a GitHub OAuth application and copy the client id and secret. Make sure the authorized redirect uri is `http://localhost:4000/auth/github/callback`.

## Kubernetes

To migrate on kubernetes, find a pod and exec into it and run this:

```bash
smart_note eval "SmartNote.ReleaseTasks.Migrate.run()"
```
