FROM python:3.12-slim

# Prevent python buffering
ENV PYTHONUNBUFFERED=1

WORKDIR /app

RUN apt update && apt install -y \
    python3 \
    python3-pip \
    python3-venv \
    && apt clean

RUN python3 -m venv /app/venv

ENV PATH="/app/venv/bin:$PATH"

COPY Containerization/requirements.txt .

RUN pip install --no-cache-dir --upgrade pip && \
    pip install --no-cache-dir -r requirements.txt

COPY fastapiapp/ .

EXPOSE 8000

CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]