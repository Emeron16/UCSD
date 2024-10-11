# Hand Gesture Detection Flask App

This folder contains the containerizes files needed to containerize the application using Docker. Here are the instruction to containerize the application

## 🛠 Installation

1. **Install Docker on your local machine**
   - Download and install Docker from [here](https://www.docker.com/products/docker-desktop/).

2. **Create a Dockerfile**
   - Use VI (or any text editor) in Terminal to create a `Dockerfile`.
   - Copy and paste the following code into the `Dockerfile`.

   ```dockerfile
   # Use an official Python runtime as a base image
   FROM python:3.9-slim

   # Install required system packages for OpenCV and GLib
   RUN apt-get update && apt-get install -y \
       libgl1-mesa-glx \
       libglib2.0-0 \
       && apt-get clean \
       && rm -rf /var/lib/apt/lists/*

   # Install dependencies for downloading ngrok
   RUN apt-get update && apt-get install -y \
       wget \
       unzip \
       curl

   # Set the working directory inside the container
   WORKDIR /app

   # Copy the requirements.txt file to the container
   COPY requirements.txt /app/

   # Install any Python dependencies
   RUN pip install --no-cache-dir -r requirements.txt

   # Copy the entire project to the container
   COPY . /app/

   # Expose port 5001 (default for Flask) and 4040 (for Ngrok)
   EXPOSE 5001
   EXPOSE 4040

   # Set environment variables
   ENV FLASK_APP=FlaskDeploymentHandGesture.py
   ENV FLASK_RUN_HOST=0.0.0.0

   # Entrypoint command
   CMD ["flask", "run", "--no-reload", "--host=0.0.0.0", "--port=5001"]


3. **Create `requirements.txt`**
   - In the project directory, create a `requirements.txt` file with the following contents:

   ```text
   Flask==3.0.3
   Flask-Cors==5.0.0
   flasgger==0.9.7.1
   numpy==1.26.4
   opencv-python==4.10.0.84
   matplotlib==3.8.0
   tensorflow==2.16.2
   pyngrok==7.2.0
   ```

## 🚀 Build and Run the Docker Image

4. **Build the Docker Image**

   To build the image, navigate to the project directory and run the following command:

   ```bash
   docker build -t my-flask-app .
   ```

5. **Run the Docker Image**

   After the image is built, run the container with:

   ```bash
   docker run -d -p 5001:5001 my-flask-app
   ```

6. **Access the Ngrok URL**

   To retrieve the Ngrok URL, navigate to [http://localhost:4040](http://localhost:4040), then:

   - Go to the headers section.
   - Find the URL under `Access-Control-Allow-Origin`.

## 📂 Saving and Managing Images(Optional)

7. **Add a path for saved images**

   To specify where images should be saved, do the following:
   - In Docker Desktop, navigate to the container and click `Run`.
   - In the host path, add the path where you want to save the images.
   - In the container path, set `/app/GeneratedImages`.



By following these steps, you should have your Flask-based hand gesture detection application running in a Docker container.

