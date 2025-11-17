
FROM node:20-alpine
WORKDIR /app

# Install dependencies (utilizes caching)
COPY package*.json ./
RUN npm install

# Copy application files and build TS
COPY . .
RUN npm run build 

EXPOSE 3000
CMD ["npm", "run", "serve"]