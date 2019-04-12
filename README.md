# cypress-dockerfiles
Docker images for running Cypress tests on Continuous Integration environments.

## Example usage with docker-compose.yml
This will immediately run the tests on executing `docker-compose up -d`:

```yaml
version: "3"
services:
  frontend:
    image: my-frontend-image
    container_name: frontend
    ports:
      - "80:80"

  cypress:
    image: icolabora/cypress:3.2
    container_name: cypress
    working_dir: /app
    environment:
      - CYPRESS_baseUrl=http://frontend
    volumes:
      - cypress:/app/cypress:ro
      - cypress.json:/app/cypress.json:ro
    depends_on:
      - frontend
```

If you want to just start the containers and leave them running so later your CI pipeline can execute the tests via `docker exec`, change the entrypoint of the cypress container like so: `/bin/sh -c "tail -f /dev/null"`

## Building the image
`docker build -t [organization]/[image]:[tag] .`

Where [organization] is the name of your organization on dockerhub (can also be the URL of a private docker repository), [image] is the name of the image, and [tag] is the version tag. Example:

`docker build -t icolabora/cypress:latest .`
