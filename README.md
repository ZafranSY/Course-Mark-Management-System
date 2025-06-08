
### **Complete Guide:**

---

### **Step 1: Creating the Dockerfile**

1. Open your project directory where your PHP Slim app is located. This will typically be something like:

   ```bash
   cd /path/to/your/project
   ```

2. Create a `Dockerfile` inside your project directory. You can do this with any text editor or directly in the terminal. For example:

   ```bash
   touch Dockerfile
   ```

3. Add the following content to the `Dockerfile` (use the editor of your choice):

   ```Dockerfile
   # Use the official PHP 7 Alpine image as a base image
   FROM php:7-alpine

   # Install system dependencies and PHP extensions
   RUN apk --no-cache add \
       bash \
       git \
       libpng-dev \
       libjpeg-dev \
       libfreetype6-dev \
       && docker-php-ext-configure gd --with-freetype --with-jpeg \
       && docker-php-ext-install gd

   # Install Composer (PHP Dependency Manager)
   RUN curl -sS https://getcomposer.org/installer | php \
       && mv composer.phar /usr/local/bin/composer

   # Set the working directory to /var/www
   WORKDIR /var/www

   # Copy the composer.json and composer.lock (if available) to the container
   COPY composer.json /var/www/

   # Install PHP dependencies using Composer
   RUN composer install

   # Copy the rest of your application code to the container
   COPY . /var/www

   # Expose the port 8080
   EXPOSE 8080

   # Set the command to run the PHP built-in server
   CMD ["php", "-S", "0.0.0.0:8080", "-t", "public"]
   ```

4. **Explanation**:

   * This `Dockerfile` uses a lightweight PHP 7 Alpine image, installs the necessary dependencies, Composer, and copies your app files into the container.
   * It then runs the built-in PHP server on port 8080 and serves your Slim app.

---

### **Step 2: Building the Docker Image**

1. Open the terminal (in the same directory as the `Dockerfile`).

2. Build the Docker image by running:

   ```bash
   docker build -t slim-app:v1 .
   ```

   * `slim-app:v1` is the tag for your image.
   * The `.` at the end indicates that Docker should look for the `Dockerfile` in the current directory.

3. Docker will begin downloading the base image, installing dependencies, and setting up the environment according to the `Dockerfile`. This might take a few minutes.

4. Once it's finished, you can confirm the image was created by running:

   ```bash
   docker image ls
   ```

   You should see `slim-app:v1` listed.

---

### **Step 3: Running the Docker Container**

1. Now that the image is built, run the container with the following command:

   ```bash
   docker run --name slim-app-container -p 8080:8080 -d slim-app:v1
   ```

   * `--name slim-app-container`: Names the container `slim-app-container`.
   * `-p 8080:8080`: Maps port `8080` on your machine to port `8080` in the container.
   * `-d`: Runs the container in detached mode (in the background).
   * `slim-app:v1`: The image to run.

2. You can now access your Slim application in the browser by going to `http://localhost:8080`.

3. To confirm the container is running, you can use:

   ```bash
   docker ps
   ```

   This will show all running containers.

---

### **Step 4: Stopping the Docker Container**

1. To stop the running container, use:

   ```bash
   docker stop slim-app-container
   ```

   This stops the container.

2. To remove the container after stopping it (optional):

   ```bash
   docker container rm slim-app-container
   ```

---

### **Step 5: Pulling the Docker Image for Your Groupmates**

To make it easier for your groupmates to use the image without building it from scratch, you can upload your Docker image to Docker Hub or another container registry. Here’s how:

1. **Login to Docker Hub (if you don't have an account, create one first)**:

   ```bash
   docker login
   ```

2. **Tag your image for Docker Hub**:

   Tag your image with your Docker Hub username. Replace `yourusername` with your actual Docker Hub username:

   ```bash
   docker tag slim-app:v1 yourusername/slim-app:v1
   ```

3. **Push the image to Docker Hub**:

   After tagging the image, you can push it to Docker Hub:

   ```bash
   docker push yourusername/slim-app:v1
   ```

4. **Now, groupmates can pull the image**:

   To pull the image, your groupmates can run the following command:

   ```bash
   docker pull yourusername/slim-app:v1
   ```

5. Once they have pulled the image, they can run it with the same command:

   ```bash
   docker run --name slim-app-container -p 8080:8080 -d yourusername/slim-app:v1
   ```

---

### **Step 6: Complete Commands Recap**

Here is a complete summary of the commands you or your groupmates will need:

#### **Create and Build Docker Image**

```bash
cd /path/to/your/project          # Navigate to your project directory
touch Dockerfile                  # Create a Dockerfile (if not already created)
# Add content to Dockerfile (as shown earlier)

docker build -t slim-app:v1 .      # Build the Docker image
docker image ls                   # Confirm the image exists
```

#### **Run the Docker Container**

```bash
docker run --name slim-app-container -p 8080:8080 -d slim-app:v1
docker ps                         # Confirm the container is running
```

#### **Stop and Remove the Docker Container**

```bash
docker stop slim-app-container    # Stop the container
docker container rm slim-app-container  # Optionally, remove the container
```

#### **Push the Docker Image to Docker Hub**

```bash
docker login                      # Log in to Docker Hub (if you haven't already)
docker tag slim-app:v1 yourusername/slim-app:v1    # Tag the image for Docker Hub
docker push yourusername/slim-app:v1  # Push the image to Docker Hub
```

#### **Pull and Run the Docker Image**

```bash
docker pull yourusername/slim-app:v1  # Pull the image from Docker Hub
docker run --name slim-app-container -p 8080:8080 -d yourusername/slim-app:v1  # Run the image
```

---

### **Conclusion**

This guide covers the full process of creating a Docker image for your PHP Slim application, building it, running the container, stopping it, and how to push and pull Docker images for your groupmates to use.

Feel free to ask if you need further clarification or any help!
