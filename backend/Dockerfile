# Stage 1: Builder
FROM node:21 AS backend-builder

WORKDIR /app

# copy package.json and lock file first (better caching)
COPY package*.json ./

# install dependencies (including dev for tests)
RUN npm install

# copy the rest of the code
COPY . .

# run tests (CI mode)
RUN npm test -- --ci --runInBand --coverage

# Stage 2: Runtime
FROM node:21-slim

WORKDIR /app

# copy only necessary files
COPY package*.json ./

# install only production dependencies
RUN npm install --omit=dev

# copy built app (not node_modules from builder)
COPY --from=backend-builder /app/dist ./dist
COPY .env.docker .env

EXPOSE 8080

CMD ["npm", "start"]
