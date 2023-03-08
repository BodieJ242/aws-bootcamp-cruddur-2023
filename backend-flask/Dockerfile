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