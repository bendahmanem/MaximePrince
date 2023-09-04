FROM node:14
WORKDIR /app
#COPY package*.json ./
RUN npm install -g serve
COPY . .
CMD serve -s dist
