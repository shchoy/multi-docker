# tag this as builder Phase
FROM node:alpine as builder

WORKDIR '/app'

COPY package*.json ./
RUN npm install
COPY . .
# will create a /app/build folder that stores the files 
# to be copy over to nginx for production.
RUN npm run build

# a new FROM statement meanse a new segment from he past FROM statement
# this creates a multi-step build, with the final image being FROM nginx
FROM nginx
# EXPOSE is used by AWS Elastic Beanstalk to automatically map Port
EXPOSE 3000 

# overwrite the Nginx routing conf rules to the nginx container default folder
COPY ./nginx/default.conf /etc/nginx/conf.d/default.conf 

# copy from the builder Phase, /app/build folder to the designated nginx folder requirements.
# nginx will automatically start-up once container starts. No need to specifically call a start-up.
COPY --from=builder /app/build /usr/share/nginx/html