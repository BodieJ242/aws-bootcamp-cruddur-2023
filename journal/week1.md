# Week 1 — App Containerization

## Setup Synk 

Free Service

https://app.snyk.io/

## Info about AWS Secrets Manager

https://aws.amazon.com/secrets-manager/

## Built Dockerfile for Week 1 class

    FROM python:3.10-slim-buster

    #Inside the Container
    WORKDIR /backend-flask

    #Outside the Container > Inside the Container
    # This contains libraries we want to install to run the application
    COPY requirements.txt requirements.txt

    #Inside Container
    #Instal the python libraries used for the app
    RUN pip3 install -r requirements.txt

    #Outside Container > Onside Container
    #Means everything in the current directory
    #first period . /backend-flask (outside container)
    #second period  ./ backend -flask (inside container)
    COPY . .

    #Set Environment Variables inside the container
    ENV FLASK_ENV=development

    EXPOSE ${PORT}
    ## python3 -m flask run --host=0.0.0.0 --port=4567
    CMD [ "python3", "-m" , "flask", "run", "--host=0.0.0.0", "--port=4567"]


## Containerize Backend

    cd backend-flask
    export FRONTEND_URL="*"
    export BACKEND_URL="*"
    python3 -m flask run --host=0.0.0.0 --port=4567
    cd ..

make sure to unlock the port on the port tab
open the link for 4567 in your browser
append to the url to /api/activities/home
you should get back json

##Build Docker Container

    docker build -t  backend-flask ./backend-flask

## Run

    docker run --rm -p 4567:4567 -it backend-flask
    FRONTEND_URL="*" BACKEND_URL="*" docker run --rm -p 4567:4567 -it backend-flask
    export FRONTEND_URL="*"
    export BACKEND_URL="*"
    docker run --rm -p 4567:4567 -it -e FRONTEND_URL='*' -e BACKEND_URL='*' backend-flask
    docker run --rm -p 4567:4567 -it  -e FRONTEND_URL -e BACKEND_URL backend-flask
    unset FRONTEND_URL="*"
    unset BACKEND_URL="*"


## Build Container
        docker build -t frontend-react-js ./frontend-react-js
        
## Run Container
        docker run -p 3000:3000 -d frontend-react-js
 
## Get Container Images or Running Container Ids
    docker ps
    docker images
    
## Send Curl to Test Server
    curl -X GET http://localhost:4567/api/activities/home -H "Accept: application/json" -H "Content-Type: application/json"
    
## Check Container Logs
    docker logs CONTAINER_ID -f
    docker logs backend-flask -f
    docker logs $CONTAINER_ID -f
    
## Debugging adjacent containers with other containers
    docker run --rm -it curlimages/curl "-X GET http://localhost:4567/api/activities/home -H \"Accept: application/json\" -H \"Content-Type: application/json\""
    
busybosy is often used for debugging since it install a bunch of thing

    docker run --rm -it busybosy
    
## Gain Access to a Container

    docker exec CONTAINER_ID -it /bin/bash
You can just right click a container and see logs in VSCode with Docker extension

## Delete an Image

docker image rm backend-flask --force
docker rmi backend-flask is the legacy syntax, you might see this is old docker tutorials and articles.

There are some cases where you need to use the --force

## Overriding Ports
FLASK_ENV=production PORT=8080 docker run -p 4567:4567 -it backend-flask
Look at Dockerfile to see how ${PORT} is interpolated

## Containerize Frontend

##Run NPM Install
We have to run NPM Install before building the container since it needs to copy the contents of node_modules

    cd frontend-react-js
    npm i

## Create Docker File

    Create a file here: frontend-react-js/Dockerfile

    FROM node:16.18

    ENV PORT=3000

    COPY . /frontend-react-js
    WORKDIR /frontend-react-js
    RUN npm install
    EXPOSE ${PORT}
    CMD ["npm", "start"]
    
## Build Container

    docker build -t frontend-react-js ./frontend-react-js
    
## Run Container

    docker run -p 3000:3000 -d frontend-react-js
    
## Multiple Containers

## Create a docker-compose file

Create docker-compose.yml at the root of your project.

    version: "3.8"
    services:
      backend-flask:
        environment:
          FRONTEND_URL: "https://3000-${GITPOD_WORKSPACE_ID}.${GITPOD_WORKSPACE_CLUSTER_HOST}"
          BACKEND_URL: "https://4567-${GITPOD_WORKSPACE_ID}.${GITPOD_WORKSPACE_CLUSTER_HOST}"
        build: ./backend-flask
        ports:
          - "4567:4567"
        volumes:
          - ./backend-flask:/backend-flask
      frontend-react-js:
        environment:
          REACT_APP_BACKEND_URL: "https://4567-${GITPOD_WORKSPACE_ID}.${GITPOD_WORKSPACE_CLUSTER_HOST}"
        build: ./frontend-react-js
        ports:
          - "3000:3000"
        volumes:
          - ./frontend-react-js:/frontend-react-js

    # the name flag is a hack to change the default prepend folder
    # name when outputting the image names
    networks: 
      internal-network:
        driver: bridge
        name: cruddur
        
        
## Adding DynamoDB Local and Postgres

We are going to use Postgres and DynamoDB local in future labs We can bring them in as containers and reference them externally

Lets integrate the following into our existing docker compose file:

    Postgres
    services:
      db:
        image: postgres:13-alpine
        restart: always
        environment:
          - POSTGRES_USER=postgres
          - POSTGRES_PASSWORD=password
        ports:
          - '5432:5432'
        volumes: 
          - db:/var/lib/postgresql/data
    volumes:
      db:
        driver: local
        
To install the postgres client into Gitpod

      - name: postgres
        init: |
          curl -fsSL https://www.postgresql.org/media/keys/ACCC4CF8.asc|sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/postgresql.gpg
          echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" |sudo tee  /etc/apt/sources.list.d/pgdg.list
          sudo apt update
          sudo apt install -y postgresql-client-13 libpq-dev
          
## DynamoDB Local

    services:
      dynamodb-local:
        # https://stackoverflow.com/questions/67533058/persist-local-dynamodb-data-in-volumes-lack-permission-unable-to-open-databa
        # We needed to add user:root to get this working.
        user: root
        command: "-jar DynamoDBLocal.jar -sharedDb -dbPath ./data"
        image: "amazon/dynamodb-local:latest"
        container_name: dynamodb-local
        ports:
          - "8000:8000"
        volumes:
          - "./docker/dynamodb:/home/dynamodblocal/data"
        working_dir: /home/dynamodblocal
Example of using DynamoDB local https://github.com/100DaysOfCloud/challenge-dynamodb-local

## Volumes
directory volume mapping

    volumes: 
    - "./docker/dynamodb:/home/dynamodblocal/data"
named volume mapping

    volumes: 
    - db:/var/lib/postgresql/data

    volumes:
      db:
        driver: local
