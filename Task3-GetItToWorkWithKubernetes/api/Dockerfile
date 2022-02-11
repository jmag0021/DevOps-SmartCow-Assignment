# Dockerfile for Python API application
# docker build . -t api-image
# docker run --rm --name api -e MODULE_NAME=app -e PORT=8000 -p 8000:8000 api-image
# Check result on http://localhost:8000/stats

FROM tiangolo/meinheld-gunicorn-flask:python3.7

COPY . /app

RUN pip install \
  --quiet \
  -r requirements.txt

# Define ENV variables needed for image 
# MODULE_NAME - The Python "module" (file) to be imported by Gunicorn, this module would contain the actual Flask application in a variable.
# PORT - The port the container should listen on.

# ENV MODULE_NAME="app" \
#     PORT="8000"