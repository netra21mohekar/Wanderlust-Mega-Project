# ------------------- Stage 1: Build Stage ------------------------------
FROM node:21 AS frontend-builder

# Set working directory
WORKDIR /app

# Copy package files and install dependencies
COPY package*.json ./
RUN npm install

# Copy the rest of the app and build it
COPY . .
RUN npm run build

# ------------------- Stage 2: Serve Stage -----------------------------
FROM node:21-slim

# Set working directory
WORKDIR /app

# Install a lightweight static server
RUN npm install -g serve

# Copy built frontend from builder stage
COPY --from=frontend-builder /app/dist ./dist

# Expose port
EXPOSE 5173

# Start the static server
CMD ["serve", "-s", "dist", "-l", "5173"]
