## Task 1 - Dockerize the Application

The first task is to dockerise this application - as part of this task you will have to get the application to work with Docker and Docker Compose. You are expected to get this app to work with UWSGI or Gunicorn and serve the react frontend through Nginx. 

The React container should also perform `npm build` every time it is built.

Hint/Optional - Create 3 separate containers. 1 for the backend, 2nd for the proxy and 3rd for the react frontend.

It is expected that you create another small document/walkthrough or readme which helps us understand your thinking process behind all of the decisions you made. 

The only strict requirement is that the application should spin up with `docker-compose up --build` command. 

You will be evaluated based on the
* best practices
* ease of use
* quality of the documentation provided with the code

---

## Task 1 - Documentation

1. Create Dockerfile for api
    - Quick search on best practice of packaging docker file for python applications (Had previous experience of such builds)
    - Docker image with Meinheld managed by Gunicorn for high-performance web applications in Flask using Python with performance auto-tuning.
        - GitHub repo: https://github.com/tiangolo/meinheld-gunicorn-flask-docker
        - Docker Hub image: https://hub.docker.com/r/tiangolo/meinheld-gunicorn-flask/
    - Followed documenatation for image on dockerhub to deploy the application
        - First copying the fils to the /app directory
        - Running of pip to install required modules for the applications ( optional --quite to silence output)
        - Setup environmental variables accordingly to run

2. Create Dockerfile for sys-stats
    - Quick search on best practice of packaging docker file for react applications
    - Using multi stage dockerfile 
        - first stage we are using node alpine image to build the react app
        - second stage the nginx proxy is refering to the build result of the node image to serve the webapp

3. Creating the dokercompose
        - docker compose setup is setup with build directories for each application and environmental variables are set up accordingly

To run the project:
    - Run `docker-compose up --build` in the Task1-DockerizeTheApplication
    - Open browser http://localhost:3000