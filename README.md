# LocalDocker

### ✅ **Step 1: Install Docker (with Buildx and Docker Compose)**
Now let's install Docker and the necessary components, including Buildx and Docker Compose.

#### Add Docker's official repository:
```bash
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo tee /etc/apt/keyrings/docker.asc > /dev/null
sudo chmod a+r /etc/apt/keyrings/docker.asc
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

#### Install Docker:
```bash
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

This will install Docker along with the necessary components.

### ✅ **Step 2: Verify Docker and Buildx Installation**
Now that Docker is installed, check if Buildx is available:
```bash
docker buildx version
```

If this works, then **Docker and Buildx** are installed successfully.

### ✅ **Step 3: Restart Docker**
In case Docker isn't running, restart it:
```bash
sudo systemctl restart docker
```




### Run react project by docker:

1. **Rebuild your React app**:

   In your React project directory, run the following command to build your app:
   ```bash
   npm run build
   ```

   This will create a `build/` directory containing the production-ready version of your app.

2. **Update your Dockerfile**:

   Ensure your `Dockerfile` copies the `build/` directory into the correct location inside the container. Here's an example of what your Dockerfile might look like:

   ```Dockerfile
  # Use the official Node.js image as the base image
  FROM node:22 AS build
  
  # Set the working directory
  WORKDIR /app
  
  # Copy package.json and package-lock.json to the working directory
  COPY package*.json ./
  
  # Install dependencies
  RUN npm install
  
  # Copy the rest of the application code to the working directory
  COPY . .
  
  # Build the React application
  RUN npm run build
  
  # Use the official Nginx image to serve the React application
  FROM nginx:alpine
  
  # Copy the build output to the Nginx html directory
  COPY --from=build /app/build /usr/share/nginx/html
  
  # Expose port 80 to make the app accessible
  EXPOSE 80
  
  # Start Nginx
  CMD ["nginx", "-g", "daemon off;"]

   ```

   This Dockerfile does the following:
   - It first builds the React app using a Node.js image (`node:16`).
   - Then, it uses an Nginx image (`nginx:alpine`) to serve the built React app.
   - It copies the contents of the `build/` directory to `/usr/share/nginx/html` in the Nginx container.

3.### ✅ **Move project directory in Ubuntu cmd:**
    ```bash
     cd /mnt/c/reactjs/myreactapp/ReactJsProject
    ```
3. **Rebuild the Docker image**:

   After updating the Dockerfile, rebuild the Docker image by running the following command:
   ```bash
   docker build -t my-react-app .
   ```

4. **Run the Docker container again**:

   Once the image is rebuilt, run the container again:
   ```bash
   docker run -p 3000:80 my-react-app
   ```

5. **Access your app**:

   Finally, open your browser and visit:
   ```
   http://localhost:3000
   ```
   Your React app should now be properly served by Nginx and accessible.
