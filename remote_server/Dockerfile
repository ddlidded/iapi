FROM node:16-alpine as build
WORKDIR /app/frontend
COPY remote_server/frontend/package*.json ./
RUN npm install
COPY remote_server/frontend/ ./
RUN npm run build

FROM python:3.9-slim
WORKDIR /app

# Install dependencies
COPY remote_server/requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy application files
COPY remote_server/app.py .

# Create a default .env file (will be overridden by environment variables)
RUN echo "PORT=5163\nLOG_LEVEL=INFO" > .env

# Create static directory
RUN mkdir -p static

# Copy built frontend from the build stage
COPY --from=build /app/frontend/dist/* /app/static/

# Expose port (use the one from your .env file)
EXPOSE 5163

# Run the application
CMD ["python", "app.py"]