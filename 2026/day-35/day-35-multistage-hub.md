# Day 35 – Multi-Stage Builds & Docker Hub

## Task 1: The Problem with Large Images

### Write a simple Go, Java, or Node.js app (even a "Hello World" is fine)

# Node.js Conversion Steps

This project was converted from a static HTML application into a Node.js application using Express.js.

## 1. Initialize Node.js Project

Create a Node.js project:

```bash
npm init -y
```

This creates:

```
package.json
```

---

## 2. Install Express.js

Install Express framework:

```bash
npm install express
```

This creates:

```
node_modules/
package-lock.json
```

---

## 3. Project Structure

After conversion, the project structure:

```
weather/
│
├── server.js
├── package.json
├── package-lock.json
│
└── public/
    └── index.html
```

---

## 4. Create server.js

Create a file named:

```
server.js
```

Add the following code:

```javascript
const express = require("express");

const app = express();

const PORT = 3000;


// Serve frontend files
app.use(express.static("public"));


// Weather API endpoint
app.get("/api/weather", async (req, res) => {

    try {

        const { lat, lon } = req.query;


        if (!lat || !lon) {
            return res.status(400).json({
                error: "Latitude and longitude are required"
            });
        }


        const url =
        `https://api.open-meteo.com/v1/forecast?latitude=${lat}&longitude=${lon}&current=temperature_2m,relative_humidity_2m,wind_speed_10m`;


        const response = await fetch(url);

        const data = await response.json();


        res.json(data);


    } catch(error) {

        res.status(500).json({
            error: error.message
        });

    }

});


// Start server
app.listen(PORT, () => {

    console.log(`Weather app running on port ${PORT}`);

});
```

---

## 5. Update package.json

Add the start script:

```json
"scripts": {
  "start": "node server.js"
}
```

---

## 6. Move Frontend Files

Create a public folder:

```bash
mkdir public
```

Move the HTML file:

```bash
mv index.html public/
```

---

## 7. Run Node.js Application

Install dependencies:

```bash
npm install
```

Start the application:

```bash
npm start
```

Output:

```
Weather app running on port 3000
```

The application will run on:

```
http://localhost:3000
```

---

## API Endpoint

Node.js backend provides:

```
GET /api/weather
```

Example:

```
http://localhost:3000/api/weather?lat=28.61&lon=77.20
```

The server fetches weather data from Open-Meteo API and returns the response to the frontend.

<img width="1365" height="727" alt="image" src="https://github.com/user-attachments/assets/75f852c0-8547-4a65-a9af-87d5cc9f1d1f" />

### Create a Dockerfile that builds and runs it in a single stage

```bash

ubuntu@ip-172-31-8-6:~/weather$ cat Dockerfile
FROM node:22

WORKDIR /app

COPY package*.json ./

run npm install

COPY . .

EXPOSE 3000

CMD ["npm","start"]



ubuntu@ip-172-31-8-6:~/weather$
```


### Build the image and check its size

```bash
ubuntu@ip-172-31-8-6:~/weather$ docker build -t weather-node .
DEPRECATED: The legacy builder is deprecated and will be removed in a future release.
            Install the buildx component to build images with BuildKit:
            https://docs.docker.com/go/buildx/

Sending build context to Docker daemon  2.809MB
Step 1/7 : FROM node:22
22: Pulling from library/node
160cb63e6a29: Pulling fs layer
c4013e1e3834: Pulling fs layer
bd0ec93c9c52: Pulling fs layer
2dd2dd4f152b: Pulling fs layer
34e7f1cb7a29: Pulling fs layer
c487d6e7caa3: Pulling fs layer
ec53e715ea47: Pulling fs layer
b5082ce0961a: Pulling fs layer
b5082ce0961a: Download complete
160cb63e6a29: Download complete
c487d6e7caa3: Download complete
061c41f63f0a: Download complete
bd0ec93c9c52: Download complete
bde9519ccf9b: Download complete
c4013e1e3834: Download complete
ec53e715ea47: Download complete
2dd2dd4f152b: Download complete
34e7f1cb7a29: Download complete
c4013e1e3834: Pull complete
bd0ec93c9c52: Pull complete
2dd2dd4f152b: Pull complete
34e7f1cb7a29: Pull complete
c487d6e7caa3: Pull complete
b5082ce0961a: Pull complete
ec53e715ea47: Pull complete
160cb63e6a29: Pull complete
Digest: sha256:7725a5c2c83eed1d36258c66efae14b1ceccd021db9ed1d9559d3335ed3d68ed
Status: Downloaded newer image for node:22
 ---> 7725a5c2c83e
Step 2/7 : WORKDIR /app
 ---> Running in d3f837c999f9
 ---> Removed intermediate container d3f837c999f9
 ---> 80d2ea830ac0
Step 3/7 : COPY package*.json ./
 ---> 1bd58040238c
Step 4/7 : run npm install
 ---> Running in 5c978b82b27f

added 67 packages, and audited 68 packages in 2s

26 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
npm notice
npm notice New major version of npm available! 10.9.8 -> 12.0.2
npm notice Changelog: https://github.com/npm/cli/releases/tag/v12.0.2
npm notice To update run: npm install -g npm@12.0.2
npm notice
 ---> Removed intermediate container 5c978b82b27f
 ---> 07a5da13b864
Step 5/7 : COPY . .
 ---> 56b7335784c4
Step 6/7 : EXPOSE 3000
 ---> Running in 7e6dcfa31b19
 ---> Removed intermediate container 7e6dcfa31b19
 ---> 724371e0a62a
Step 7/7 : CMD ["npm","start"]
 ---> Running in 88211522b570
 ---> Removed intermediate container 88211522b570
 ---> d6033628964e
Successfully built d6033628964e
Successfully tagged weather-node:latest
ubuntu@ip-172-31-8-6:~/weather$ docker images
                                                                                                                              i Info →   U  In Use
IMAGE                 ID             DISK USAGE   CONTENT SIZE   EXTRA
node:22               7725a5c2c83e       1.64GB          425MB
weather-node:latest   d6033628964e       1.64GB          411MB
ubuntu@ip-172-31-8-6:~/weather$
```

## Task 2: Multi-Stage Build

### Multi-stage Docker builds separate the build environment from the production environment by compiling the application in Stage 1 and copying only the required final artifacts into a smaller, secure image in Stage 2.

```bash
───────┬──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: Dockerfile
───────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ # ==========================================
   2   │ # Stage 1: Install Dependencies
   3   │ # ==========================================
   4   │
   5   │ FROM node:22-alpine AS builder
   6   │
   7   │ WORKDIR /app
   8   │
   9   │ COPY package*.json ./
  10   │
  11   │ RUN npm install --production
  12   │
  13   │
  14   │ # ==========================================
  15   │ # Stage 2: Production Runtime
  16   │ # ==========================================
  17   │
  18   │ FROM node:22-alpine AS production
  19   │
  20   │ WORKDIR /app
  21   │
  22   │ COPY --from=builder /app/node_modules ./node_modules
  23   │
  24   │ COPY package*.json ./
  25   │
  26   │ COPY server.js ./
  27   │
  28   │ COPY public ./public
  29   │
  30   │
  31   │ EXPOSE 3000
  32   │
  33   │ CMD ["npm","start"]

```

### Build the image and check its size again

```bash

ubuntu@ip-172-31-8-6:~/weather$ docker build -t weather-mini .
DEPRECATED: The legacy builder is deprecated and will be removed in a future release.
            Install the buildx component to build images with BuildKit:
            https://docs.docker.com/go/buildx/

Sending build context to Docker daemon  2.809MB
Step 1/12 : FROM node:22-alpine AS builder
22-alpine: Pulling from library/node
16da5a640377: Pulling fs layer
efbef6f9e333: Pulling fs layer
a2980c1fee17: Pulling fs layer
16da5a640377: Download complete
a2980c1fee17: Download complete
35d06909b2a1: Download complete
16a559d14b4b: Download complete
efbef6f9e333: Download complete
efbef6f9e333: Pull complete
16da5a640377: Pull complete
a2980c1fee17: Pull complete
Digest: sha256:c610fcdfb1d5b4740dd70c284ed3cb16bb857e0f7166196e36a5501df7a3aa32
Status: Downloaded newer image for node:22-alpine
 ---> c610fcdfb1d5
Step 2/12 : WORKDIR /app
 ---> Running in 6191fa4a832d
 ---> Removed intermediate container 6191fa4a832d
 ---> d957b731f501
Step 3/12 : COPY package*.json ./
 ---> d54bc96dd947
Step 4/12 : RUN npm install --production
 ---> Running in 66bef61b3365
npm warn config production Use `--omit=dev` instead.

added 67 packages, and audited 68 packages in 2s

26 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
npm notice
npm notice New major version of npm available! 10.9.8 -> 12.0.2
npm notice Changelog: https://github.com/npm/cli/releases/tag/v12.0.2
npm notice To update run: npm install -g npm@12.0.2
npm notice
 ---> Removed intermediate container 66bef61b3365
 ---> e7cbc75322a9
Step 5/12 : FROM node:22-alpine AS production
 ---> c610fcdfb1d5
Step 6/12 : WORKDIR /app
 ---> Using cache
 ---> d957b731f501
Step 7/12 : COPY --from=builder /app/node_modules ./node_modules
 ---> de7d8c5a68a4
Step 8/12 : COPY package*.json ./
 ---> 138969b60442
Step 9/12 : COPY server.js ./
 ---> 9d3b96b1eeb1
Step 10/12 : COPY public ./public
 ---> c006ed609a1a
Step 11/12 : EXPOSE 3000
 ---> Running in 14426146b030
 ---> Removed intermediate container 14426146b030
 ---> 37020ecc80ce
Step 12/12 : CMD ["npm","start"]
 ---> Running in 649ef1512e62
 ---> Removed intermediate container 649ef1512e62
 ---> 961dbd996cbd
Successfully built 961dbd996cbd
Successfully tagged weather-mini:latest
ubuntu@ip-172-31-8-6:~/weather$ docker images
                                                                                                                              i Info →   U  In Use
IMAGE                 ID             DISK USAGE   CONTENT SIZE   EXTRA
node:22-alpine        c610fcdfb1d5        232MB         58.1MB
weather-mini:latest   961dbd996cbd        237MB         58.4MB
```
### Compare the two sizes

<img width="725" height="61" alt="image" src="https://github.com/user-attachments/assets/30bafd72-3933-469f-bff9-ebc15aed1998" />

<img width="489" height="51" alt="image" src="https://github.com/user-attachments/assets/0e78b4e5-e5e6-4b99-b5a3-79c7b348179d" />

### Write in your notes: Why is the multi-stage image so much smaller?

Multi-stage builds reduce image size by separating the build and production stages. The first stage creates the application artifact, and the second stage copies only the required artifacts and dependencies into a minimal image, removing unnecessary files.

Example:

Normal image: 1.61 GB
Multi-stage image: 232 MB
This makes the final image smaller, faster, and more efficient.

## Task 3: Push to Docker Hub

### Created a free Docker Hub account (if needed), logged in from the terminal, tagged the image as yourusername/image-name:tag, and pushed it to Docker Hub.

```bash

ubuntu@ip-172-31-8-6:~$
ubuntu@ip-172-31-8-6:~$ docker images
                                                                                                                              i Info →   U  In Use
IMAGE                 ID             DISK USAGE   CONTENT SIZE   EXTRA
node:22-alpine        c610fcdfb1d5        232MB         58.1MB
weather-mini:latest   961dbd996cbd        237MB         58.4MB
ubuntu@ip-172-31-8-6:~$
ubuntu@ip-172-31-8-6:~$ docker login
Authenticating with existing credentials... [Username: sufiyn]

i Info → To login with a different account, run 'docker logout' followed by 'docker login'


Login Succeeded
ubuntu@ip-172-31-8-6:~$ docker tag weather-mini:latest sufiyn/weather-mini:v1
ubuntu@ip-172-31-8-6:~$ docker images
                                                                                                                              i Info →   U  In Use
IMAGE                    ID             DISK USAGE   CONTENT SIZE   EXTRA
node:22-alpine           c610fcdfb1d5        232MB         58.1MB
sufiyn/weather-mini:v1   961dbd996cbd        237MB         58.4MB
weather-mini:latest      961dbd996cbd        237MB         58.4MB
ubuntu@ip-172-31-8-6:~$ docker push sufiyn/weather-mini:v1
The push refers to repository [docker.io/sufiyn/weather-mini]
efbef6f9e333: Mounted from library/node
4a6dec83989a: Pushed
e2a81fb3d6ae: Pushed
a402176eca24: Pushed
c3bc37e37e76: Pushed
55afa1ecc21d: Mounted from library/nginx
a2980c1fee17: Mounted from library/node
16da5a640377: Mounted from library/node
6fa1c242ab75: Pushed
v1: digest: sha256:961dbd996cbd211e61db1953f12b71f84ea8bab8f86ee8dc3a5fbcc8b81c5b20 size: 2280
ubuntu@ip-172-31-8-6:~$

```
### Pull it on a different machine (or after removing locally) to verify

```bash

ubuntu@ip-172-31-8-6:~$ docker images
                                                                                                                              i Info →   U  In Use
IMAGE            ID             DISK USAGE   CONTENT SIZE   EXTRA
node:22-alpine   c610fcdfb1d5        232MB         58.1MB
ubuntu@ip-172-31-8-6:~$ docker pull sufiyn/weather-mini:v1
v1: Pulling from sufiyn/weather-mini
Digest: sha256:961dbd996cbd211e61db1953f12b71f84ea8bab8f86ee8dc3a5fbcc8b81c5b20
Status: Downloaded newer image for sufiyn/weather-mini:v1
docker.io/sufiyn/weather-mini:v1
ubuntu@ip-172-31-8-6:~$ docker images
                                                                                                                              i Info →   U  In Use
IMAGE                    ID             DISK USAGE   CONTENT SIZE   EXTRA
node:22-alpine           c610fcdfb1d5        232MB         58.1MB
sufiyn/weather-mini:v1   961dbd996cbd        237MB         58.4MB
ubuntu@ip-172-31-8-6:~$
```

## Task 4: Docker Hub Repository

### Go to Docker Hub and check your pushed image

### <img width="1365" height="678" alt="image" src="https://github.com/user-attachments/assets/e833d595-0aa3-4bfb-b701-fd464c091159" />

### Add a description to the repository

<img width="1334" height="544" alt="image" src="https://github.com/user-attachments/assets/46b97ea1-b040-4adc-b0b2-c7eabf973333" />

### Explore the tags tab — understand how versioning works

<img width="1335" height="498" alt="image" src="https://github.com/user-attachments/assets/e791dd30-f4dc-4c57-a9e6-4ce5695336e9" />

### 
<img width="1365" height="682" alt="image" src="https://github.com/user-attachments/assets/ec699f9b-9067-4b7b-81ca-5984f0b595bb" />

