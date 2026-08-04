# Day 36 – Docker Project: Dockerize a Full Application

## Task 1: Pick Your App

### A Node.js Express app with MongoDB

# School Attendance API

A Node.js + Express + MongoDB backend for tracking school attendance, with JWT auth, role-based access (admin/teacher), daily/monthly reports, and CSV export.

## Setup

```bash
npm install
cp .env.example .env
# edit .env with your MongoDB URI and JWT secret

# create the first admin account
node seedAdmin.js

# start the server
npm run dev    # with nodemon
npm start      # plain node
```

Default seeded admin: `admin@school.com` / `ChangeMe123!` — change this immediately after first login.

## Roles

- **admin** — full access: manage classes, students, users, attendance, reports
- **teacher** — can view classes/students, mark and update attendance, view reports (cannot create classes/students/users or delete attendance)

## Auth

All routes except `/api/auth/login` require a `Authorization: Bearer <token>` header.

| Method | Route | Access | Description |
|---|---|---|---|
| POST | `/api/auth/register` | admin | Create a new user (admin or teacher) |
| POST | `/api/auth/login` | public | Log in, returns JWT |
| GET | `/api/auth/me` | any logged-in user | Get current user profile |

## Classes

| Method | Route | Access |
|---|---|---|
| GET | `/api/classes` | any |
| GET | `/api/classes/:id` | any |
| POST | `/api/classes` | admin |
| PUT | `/api/classes/:id` | admin |
| DELETE | `/api/classes/:id` | admin |

## Students

| Method | Route | Access |
|---|---|---|
| GET | `/api/students?classId=` | any |
| GET | `/api/students/:id` | any |
| POST | `/api/students` | admin |
| PUT | `/api/students/:id` | admin |
| DELETE | `/api/students/:id` | admin |

## Attendance

| Method | Route | Access |
|---|---|---|
| POST | `/api/attendance` | admin, teacher |
| GET | `/api/attendance?classId=&date=&studentId=` | any |
| PUT | `/api/attendance/:id` | admin, teacher |
| DELETE | `/api/attendance/:id` | admin |

**Mark attendance for a class** (bulk, one call per class per day):

```json
POST /api/attendance
{
  "classId": "664f...",
  "date": "2026-08-02",
  "records": [
    { "studentId": "664a...", "status": "present" },
    { "studentId": "664b...", "status": "absent" },
    { "studentId": "664c...", "status": "late" }
  ]
}
```

## Reports

| Method | Route | Access | Description |
|---|---|---|---|
| GET | `/api/reports/daily?classId=&date=` | any | Present/absent/late counts for one day |
| GET | `/api/reports/monthly?classId=&year=&month=` | any | Per-student totals for the month |
| GET | `/api/reports/export?classId=&startDate=&endDate=` | any | Downloads a CSV of attendance for the date range |

## Project structure

```
school-attendance-app/
├── config/db.js
├── models/          User, Class, Student, Attendance
├── middleware/       auth.js (JWT), role.js (RBAC)
├── controllers/       auth, class, student, attendance, report
├── routes/
├── server.js
├── seedAdmin.js       bootstrap first admin
└── .env.example
```

<img width="1363" height="728" alt="image" src="https://github.com/user-attachments/assets/6d7ea361-a6e7-4812-b1a5-6489cde1fd24" />

<img width="1365" height="682" alt="image" src="https://github.com/user-attachments/assets/cf4f125e-36bb-4242-8e19-1306a0d6bbb2" />

<img width="1362" height="728" alt="image" src="https://github.com/user-attachments/assets/611b8423-771b-43db-8456-884f6e996c9c" />

## Notes

- Attendance is unique per `(student, class, date)` — marking twice for the same day updates the existing record instead of duplicating it.
- Passwords are hashed with bcrypt; never stored in plain text.
- Swap the in-memory JWT secret in `.env` for a strong random value before deploying.

## Task 2: Write the Dockerfile

### Create a Dockerfile for your application
Docker file for backend

```bash

ubuntu@ip-172-31-7-36:~/school-attendance-app$ batcat Dockerfile
───────┬──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: Dockerfile
───────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ FROM node:22-alpine
   2   │
   3   │ WORKDIR /app
   4   │
   5   │ COPY package*.json ./
   6   │
   7   │ RUN npm install
   8   │
   9   │ COPY . .
  10   │
  11   │ EXPOSE 3000
  12   │
  13   │ CMD ["npm", "start"]
  14   │
───────┴──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
ubuntu@ip-172-31-7-36:~/school-attendance-app$


```
```
ubuntu@ip-172-31-7-36:~/school-attendance-app$ docker build -t school-attendance-app:v1 .
DEPRECATED: The legacy builder is deprecated and will be removed in a future release.
            Install the buildx component to build images with BuildKit:
            https://docs.docker.com/go/buildx/

Sending build context to Docker daemon  17.94MB
Step 1/7 : FROM node:22-alpine
22-alpine: Pulling from library/node
16da5a640377: Pulling fs layer
55afa1ecc21d: Pulling fs layer
efbef6f9e333: Pulling fs layer
a2980c1fee17: Pulling fs layer
a2980c1fee17: Download complete
16da5a640377: Download complete
55afa1ecc21d: Download complete
16a559d14b4b: Download complete
35d06909b2a1: Download complete
55afa1ecc21d: Pull complete
efbef6f9e333: Download complete
efbef6f9e333: Pull complete
a2980c1fee17: Pull complete
16da5a640377: Pull complete
Digest: sha256:c610fcdfb1d5b4740dd70c284ed3cb16bb857e0f7166196e36a5501df7a3aa32
Status: Downloaded newer image for node:22-alpine
 ---> c610fcdfb1d5
Step 2/7 : WORKDIR /app
 ---> Running in 8f2d2ae39601
 ---> Removed intermediate container 8f2d2ae39601
 ---> c4ae23f18368
Step 3/7 : COPY package*.json ./
 ---> 917cd1ef12e8
Step 4/7 : RUN npm install
 ---> Running in b927307e3963
npm warn deprecated lodash.get@4.4.2: This package is deprecated. Use the optional chaining (?.) operator instead.

added 137 packages, and audited 138 packages in 3s

23 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
npm notice
npm notice New major version of npm available! 10.9.8 -> 12.0.2
npm notice Changelog: https://github.com/npm/cli/releases/tag/v12.0.2
npm notice To update run: npm install -g npm@12.0.2
npm notice
 ---> Removed intermediate container b927307e3963
 ---> 0f8cc2678208
Step 5/7 : COPY . .
 ---> 0328447ff4c9
Step 6/7 : EXPOSE 3000
 ---> Running in f72e3abae852
 ---> Removed intermediate container f72e3abae852
 ---> 2183a254a665
Step 7/7 : CMD ["npm", "start"]
 ---> Running in 223807fdc9ed
 ---> Removed intermediate container 223807fdc9ed
 ---> 44d2dd9bf086
Successfully built 44d2dd9bf086
Successfully tagged school-attendance-app:v1
ubuntu@ip-172-31-7-36:~/school-attendance-app$ docker images
                                                                                                                              i Info →   U  In Use
IMAGE                      ID             DISK USAGE   CONTENT SIZE   EXTRA
node:22-alpine             c610fcdfb1d5        232MB         58.1MB
school-attendance-app:v1   44d2dd9bf086        300MB         69.4MB
ubuntu@ip-172-31-7-36:~/school-attendance-app$
```
### "Follow Docker best practices by using multi-stage builds where applicable, running containers with a non-root user, and minimizing image size through lightweight base images like Alpine or Slim

```bash
 
 # Stage 1 - Install production dependencies
   2   │ FROM node:22-alpine AS deps
   3   │
   4   │ WORKDIR /app
   5   │
   6   │ COPY package*.json ./
   7   │
   8   │ RUN npm ci --omit=dev && npm cache clean --force
   9   │
  10   │
  11   │ # Stage 2 - Production image
  12   │ FROM node:22-alpine AS runner
  13   │
  14   │ WORKDIR /app
  15   │
  16   │ COPY --from=deps /app/node_modules ./node_modules
  17   │
  18   │ COPY server.js seedAdmin.js ./
  19   │ COPY config ./config
  20   │ COPY controllers ./controllers
  21   │ COPY middleware ./middleware
  22   │ COPY models ./models
  23   │ COPY routes ./routes
  24   │ COPY frontend ./frontend
  25   │
  26   │ # Create non-root user
  27   │ RUN addgroup -S appgroup && adduser -S appuser -G appgroup \
  28   │     && chown -R appuser:appgroup /app
  29   │
  30   │ USER appuser
  31   │
  32   │ EXPOSE 3000
  33   │
  34   │ CMD ["node", "server.js"]


```

```bash

Multi Stage Dockerfile for backend

ubuntu@ip-172-31-7-36:~/school-attendance-app$ docker images
                                                                                                                              i Info →   U  In Use
IMAGE                      ID             DISK USAGE   CONTENT SIZE   EXTRA
node:22-alpine             c610fcdfb1d5        232MB         58.1MB
school-attendance-app:v1   44d2dd9bf086        300MB         69.4MB
ubuntu@ip-172-31-7-36:~/school-attendance-app$ docker build --no-cache -t school-attendance-app .
DEPRECATED: The legacy builder is deprecated and will be removed in a future release.
            Install the buildx component to build images with BuildKit:
            https://docs.docker.com/go/buildx/

Sending build context to Docker daemon  132.1kB
Step 1/18 : FROM node:22-alpine AS deps
 ---> c610fcdfb1d5
Step 2/18 : WORKDIR /app
 ---> Running in 77ad8e13a81d
 ---> Removed intermediate container 77ad8e13a81d
 ---> af6ad13f8ff7
Step 3/18 : COPY package*.json ./
 ---> 4cf45debc4e7
Step 4/18 : RUN npm ci --omit=dev && npm cache clean --force
 ---> Running in 926adf41e5cc
npm warn deprecated lodash.get@4.4.2: This package is deprecated. Use the optional chaining (?.) operator instead.

added 110 packages, and audited 111 packages in 3s

18 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
npm notice
npm notice New major version of npm available! 10.9.8 -> 12.0.2
npm notice Changelog: https://github.com/npm/cli/releases/tag/v12.0.2
npm notice To update run: npm install -g npm@12.0.2
npm notice
npm warn using --force Recommended protections disabled.
 ---> Removed intermediate container 926adf41e5cc
 ---> b3d78825a955
Step 5/18 : FROM node:22-alpine AS runner
 ---> c610fcdfb1d5
Step 6/18 : WORKDIR /app
 ---> Running in 52b5168054ea
 ---> Removed intermediate container 52b5168054ea
 ---> a2bfc32c6c62
Step 7/18 : COPY --from=deps /app/node_modules ./node_modules
 ---> 4f30678320ee
Step 8/18 : COPY server.js seedAdmin.js ./
 ---> 2b963d726fa7
Step 9/18 : COPY config ./config
 ---> 6fa6a1bb240c
Step 10/18 : COPY controllers ./controllers
 ---> f806aa922ec3
Step 11/18 : COPY middleware ./middleware
 ---> bd9bece1202f
Step 12/18 : COPY models ./models
 ---> 3b8528156645
Step 13/18 : COPY routes ./routes
 ---> 531cd2e17f19
Step 14/18 : COPY frontend ./frontend
 ---> d25ac80b5af2
Step 15/18 : RUN addgroup -S appgroup && adduser -S appuser -G appgroup     && chown -R appuser:appgroup /app
 ---> Running in c59b4bbfdaf5
 ---> Removed intermediate container c59b4bbfdaf5
 ---> b6f5ad51fe07
Step 16/18 : USER appuser
 ---> Running in 2883c63126a1
 ---> Removed intermediate container 2883c63126a1
 ---> 98f439aeafac
Step 17/18 : EXPOSE 3000
 ---> Running in 72cf5098131b
 ---> Removed intermediate container 72cf5098131b
 ---> a1589ac6f57d
Step 18/18 : CMD ["node", "server.js"]
 ---> Running in 685bed026660
 ---> Removed intermediate container 685bed026660
 ---> 8b59882a086b
Successfully built 8b59882a086b
Successfully tagged school-attendance-app:latest
ubuntu@ip-172-31-7-36:~/school-attendance-app$ docker images
                                                                                                                              i Info →   U  In Use
IMAGE                          ID             DISK USAGE   CONTENT SIZE   EXTRA
node:22-alpine                 c610fcdfb1d5        232MB         58.1MB
school-attendance-app:latest   8b59882a086b        280MB         64.4MB
school-attendance-app:v1       44d2dd9bf086        300MB         69.4MB
ubuntu@ip-172-31-7-36:~/school-attendance-app$


ubuntu@ip-172-31-7-36:~/school-attendance-app$
```

### Added a .dockerignore file

```bash

ubuntu@ip-172-31-7-36:~/school-attendance-app$ cat .dockerignore
node_modules
.git
.gitignore
Dockerfile
README.md
.env
npm-debug.log
*.log

ubuntu@ip-172-31-7-36:~/school-attendance-app$
```

Builded and tested it locally.

Docker image for Frontend
```bash
      │ File: Dockerfile
───────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ FROM nginx:1.25-alpine
   2   │
   3   │ COPY . /usr/share/nginx/html
   4   │
   5   │ EXPOSE 80
   6   │
   7   │ CMD ["nginx", "-g", "daemon off;"]
   8   │

```

```bash
ubuntu@ip-172-31-7-36:~/school-attendance-app/frontend$ vim Dockerfile
ubuntu@ip-172-31-7-36:~/school-attendance-app/frontend$ docker build -t school-attendance-app-fe .
DEPRECATED: The legacy builder is deprecated and will be removed in a future release.
            Install the buildx component to build images with BuildKit:
            https://docs.docker.com/go/buildx/

Sending build context to Docker daemon  31.74kB
Step 1/4 : FROM nginx:1.25-alpine
1.25-alpine: Pulling from library/nginx
0d0c16747d2c: Pulling fs layer
4abcf2066143: Pulling fs layer
fc21a1d387f5: Pulling fs layer
e6ef242c1570: Pulling fs layer
13fcfbc94648: Pulling fs layer
d4bca490e609: Pulling fs layer
5406ed7b06d9: Pulling fs layer
8a3742a9529d: Pulling fs layer
8a3742a9529d: Download complete
e6ef242c1570: Download complete
13fcfbc94648: Download complete
d4bca490e609: Download complete
5406ed7b06d9: Download complete
4abcf2066143: Download complete
fc21a1d387f5: Download complete
0d0c16747d2c: Download complete
fc44f5562637: Download complete
30d1be61be14: Download complete
4abcf2066143: Pull complete
fc21a1d387f5: Pull complete
e6ef242c1570: Pull complete
13fcfbc94648: Pull complete
d4bca490e609: Pull complete
8a3742a9529d: Pull complete
5406ed7b06d9: Pull complete
0d0c16747d2c: Pull complete
Digest: sha256:516475cc129da42866742567714ddc681e5eed7b9ee0b9e9c015e464b4221a00
Status: Downloaded newer image for nginx:1.25-alpine
 ---> 516475cc129d
Step 2/4 : COPY . /usr/share/nginx/html
 ---> f2db3134125b
Step 3/4 : EXPOSE 80
 ---> Running in 816c6b5aac3d
 ---> Removed intermediate container 816c6b5aac3d
 ---> 082f97641aca
Step 4/4 : CMD ["nginx", "-g", "daemon off;"]
 ---> Running in 5ce9dbf9605e
 ---> Removed intermediate container 5ce9dbf9605e
 ---> b7dcdb213c8a
Successfully built b7dcdb213c8a
Successfully tagged school-attendance-app-fe:latest
ubuntu@ip-172-31-7-36:~/school-attendance-app/frontend$ docker images
                                                                                                                              i Info →   U  In Use
IMAGE                             ID             DISK USAGE   CONTENT SIZE   EXTRA
nginx:1.25-alpine                 516475cc129d       75.4MB         21.7MB
node:20-alpine                    fb4cd12c85ee        194MB         48.8MB
node:22-alpine                    c610fcdfb1d5        232MB         58.1MB
school-attendance-app-fe:latest   b7dcdb213c8a       74.2MB         20.5MB
school-attendance-app:latest      8b59882a086b        280MB         64.4MB
school-attendance-app:v1          44d2dd9bf086        300MB         69.4MB
ubuntu@ip-172-31-7-36:~/school-attendance-app/frontend$
```
