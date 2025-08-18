# Stage 1: Builder
FROM node:21 AS backend-builder

WORKDIR /app

# copy dependency files first
COPY package*.json ./

# install all dependencies (including dev, for tests)
RUN npm install

# copy the rest of the code
COPY . .

# run tests in CI-friendly mode
RUN npm test -- --ci --runInBand --coverage

# Stage 2: Runtime
FROM node:21-slim

WORKDIR /app

# copy only package.json + lock file
COPY package*.json ./

# install only production dependencies
RUN npm install --omit=dev

# copy source code (not node_modules)
COPY --from=backend-builder /app . 

COPY .env.docker .env

EXPOSE 8080

CMD ["npm", "start"]
