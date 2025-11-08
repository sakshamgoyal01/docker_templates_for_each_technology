# ==========================================
# GPU Runtime (TensorFlow or PyTorch)
# ==========================================
FROM pytorch/pytorch:2.4.0-cuda12.1-cudnn8-runtime

WORKDIR /app
COPY app/requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY app/ .

# Optional: preload model weights
RUN python -c "import torch; torch.jit.load('app/model/model.pt')"

EXPOSE 8080
HEALTHCHECK CMD curl -f http://localhost:8080/docs || exit 1

CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8080"]
