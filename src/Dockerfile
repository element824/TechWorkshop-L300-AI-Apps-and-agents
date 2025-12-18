
FROM python:3.12-slim

# Install uv
COPY --from=ghcr.io/astral-sh/uv:latest /uv /uvx /bin/

# Upgrade system packages to reduce vulnerabilities
RUN apt-get update && apt-get upgrade -y && rm -rf /var/lib/apt/lists/*

# Set work directory
WORKDIR /app

# Install system dependencies
RUN apt-get update && apt-get install -y \
    build-essential unixodbc-dev gcc g++ \
    libgl1 libglib2.0-0 libsm6 libxext6 libxrender1 \
    && rm -rf /var/lib/apt/lists/*

# Copy requirements and install
COPY requirements.txt ./
RUN uv pip install --system --no-cache -r requirements.txt


# Copy the rest of the code
COPY . .

# Expose the port (default FastAPI/uvicorn port)
EXPOSE 8000

# Set environment variables (override in production)
ENV PORT=8000

# Start the FastAPI app
CMD ["uv", "run", "uvicorn", "chat_app:app", "--host", "0.0.0.0", "--port", "8000"]
