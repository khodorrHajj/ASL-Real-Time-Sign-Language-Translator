# Use an official Python runtime as a parent image
FROM python:3.10-slim

# Set environment variables
ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1

# Install system dependencies for OpenCV and MediaPipe
RUN apt-get update && apt-get install -y \
    libgl1-mesa-glx \
    libglib2.0-0 \
    && rm -rf /var/lib/apt/lists/*

# Set the working directory in the container
WORKDIR /app

# Copy the requirements file into the container at /app
COPY requirements.txt .

# Install any needed packages specified in requirements.txt
RUN pip install --no-cache-dir -r requirements.txt

# Copy the rest of the backend code into the container at /app
COPY . .

# Expose the port the app runs on
EXPOSE 8000

# Run startup script that downloads GPT-2 if missing, then starts the server
CMD ["sh", "-c", "if [ ! -d \"/app/models/word_predictor/gpt2\" ] || [ -z \"$(ls -A /app/models/word_predictor/gpt2 2>/dev/null)\" ]; then echo 'Downloading GPT-2 model...'; python /app/scripts/download_hf_model.py; fi; exec python main.py"]
