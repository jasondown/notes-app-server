# Notes App Tutorial

This project is the backend node.js component of a notes application based on a tutorial by Chris Blakely, found [here](https://www.freecodecamp.org/news/full-stack-project-tutorial-create-a-notes-app-using-react-and-node-js/).

The front-end for the completed tutorial can be found in my other repository [here](https://github.com/jasondown/notes-app-ui).

## Notes

### Docker compose

The original tutorial relies on hosting a Postgres database on a free 3rd party site and using Prisma to push the schema. Instead, I have created a docker compose file that includes the following:

- Postgres hosted on port 5432. _User_: _admin_, _password_: _root_
- pgAdmin4 to allow using a GUI to perform tasks on the database*
- Liquibase to generate the database and notes table. This will allow change tracking in the repository and has potential for setting up easy integration tests.
    - The liquibase command will automatically run when the container starts up and only apply changesets not yet applied.

To run the docker compose file, you can go into the _etc_ folder and run `docker compose -f .\pg-docker-compose.yml up -d`. To bring it down you can run `docker compose -f .\pg-docker-compose.yml down`.

You will of course need docker installed to do this.

_* You may need to connect to the database using the IP address instead of localhost, though this may have only been an issue for me hosting both projects inside of WSL. To get the IP, you can run a `docker ps` command to get the id of the postgres container, then run `docker inspect {container-id}` to find the IP Address._

### Prisma

If following the tutorial, you will not be able to run `npx prisma db push` if using the database via docker. Instead, the database will already exist, so you must run `npx prism generate` and `npx prism db pull`. You will  get a warning (also shown as a comment in the _schema.prisma_ file) stating _The underlying table does not contain a valid unique identifier and can therefore currently not be handled by Prisma Client._, but this is ok; you won't be making calls to that table from the source code. That table is used by Liquibase to track which changesets have been applied.
