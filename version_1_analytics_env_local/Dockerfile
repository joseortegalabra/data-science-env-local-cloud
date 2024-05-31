FROM python:3.10.13-slim-bullseye

# legacy connection with gcloud
#RUN apt-get update && apt-get install -y curl && curl -sSL https://sdk.cloud.google.com | bash
#FROM python-gcloud
#ENV PATH=$PATH:/root/google-cloud-sdk/bin


# Set the working directory in the container
RUN mkdir -p /app/
WORKDIR /app


# Copy the requirements file into the container at /app
COPY requirements.txt requirements.txt


# Install any dependencies
RUN pip3 install -r requirements.txt

# Expose the port the app runs on
EXPOSE 8888


# Start the Jupyter Lab server
CMD [ "jupyter", "lab", "--ip=0.0.0.0", "--port=8888", "--no-browser", "--allow-root","--NotebookApp.token=''","--NotebookApp.password=''"]