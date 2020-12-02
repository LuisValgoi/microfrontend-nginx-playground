###### BUILD MULTI-STAGE PROCESS

# BUILDING APP

# lets use the node image version naming as build
FROM node:12.18.3 as build

# our working directory its going to be /app
# it will create folder called /app inside the container having our source code
WORKDIR /app

# lets copy all files from the root of this Dockerfile inside the root of our WORKDIR
# it means copying the whole source code files of this project, and pasting inside the /app
COPY . .

# then, lets download the config of our project silently
RUN npm ci --silent

# then lets build the project to generate the /build folder
RUN npm run build

# CONFIGURING & STARTING NGINX 

# to tell where we are
FROM nginx

# changing the default config (/etc/nginx/nginx.conf ) to the file (./nginx.conf) we are going to create 
# this new file will have overriden configs
COPY ./nginx.conf /etc/nginx/nginx.conf 

# from the `build` container, copy the files (/app/build/react01) and paste them (/usr/share/nginx/html) into the ngix
COPY --from=build /app/build /usr/share/nginx/html