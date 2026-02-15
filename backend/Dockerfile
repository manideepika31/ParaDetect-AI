FROM python:3.11-slim

WORKDIR /app

# Copy requirements and install dependencies
COPY requirements-pytorch.txt requirements.txt
RUN pip install --no-cache-dir -r requirements.txt

# Copy application files
COPY app_pytorch.py app.py
COPY model_fast.py .
COPY models/ models/

EXPOSE 8000

# Run the application
CMD ["uvicorn", "app:app", "--host", "0.0.0.0", "--port", "8000"]
