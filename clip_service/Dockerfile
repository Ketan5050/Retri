FROM python:3.9-slim

WORKDIR /app

# Install system dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    && rm -rf /var/lib/apt/lists/*

COPY requirements.txt .

# Install PyTorch CPU to drastically reduce container size and build time
RUN pip install torch torchvision --index-url https://download.pytorch.org/whl/cpu

# Install remaining requirements
RUN pip install --no-cache-dir -r requirements.txt

# Set up writable cache directory for Hugging Face Transformers
ENV HF_HOME=/app/cache
RUN mkdir -p /app/cache && chmod 777 /app/cache

COPY . .

# Explicitly set the port to 7860 for Hugging Face
ENV PORT=7860
EXPOSE 7860

# We use python -u to unbuffer standard output so logs show up immediately
CMD ["python", "-u", "app.py"]
