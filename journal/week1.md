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