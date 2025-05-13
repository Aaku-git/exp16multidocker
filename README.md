# exp16multidocker
Creating a **multi-stage Dockerfile** helps you build smaller, optimized Docker images by separating the build and runtime environments. Here's a simple example using a **Python Flask application**.

---

## 🧩 Example Project Structure:

```
flask-app/
├── app.py
├── requirements.txt
└── Dockerfile
```

---

## 📝 app.py

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def home():
    return "Hello from Multi-Stage Docker Build!"

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
```

---

## 📄 requirements.txt

```
flask
```

---

## 🐳 Multi-Stage Dockerfile

```Dockerfile
# Stage 1: Build
FROM python:3.9-slim as builder

# Set working directory
WORKDIR /app

# Install dependencies
COPY requirements.txt .
RUN pip install --user -r requirements.txt

# Copy source code
COPY app.py .

# Stage 2: Runtime
FROM python:3.9-slim

# Set working directory
WORKDIR /app

# Copy only what is needed from builder
COPY --from=builder /root/.local /root/.local
COPY --from=builder /app /app

# Add pip binaries to PATH
ENV PATH=/root/.local/bin:$PATH

# Expose Flask port
EXPOSE 5000

# Run the app
CMD ["python", "app.py"]
```

---

## ✅ Build and Run

### 🔨 Build Docker image:

```bash
docker build -t flask-multi-stage .
```

### ▶️ Run the container:

```bash
docker run -p 5000:5000 flask-multi-stage
```

Visit [http://localhost:5000](http://localhost:5000) in your browser.

---

## ✅ Benefits of Multi-Stage:

* Final image is **cleaner and smaller**
* **No build tools** (like pip) remain in the runtime image
* Easier to manage production-ready containers

---

Would you like an example using **Node.js**, **Java**, or **compiled languages** like **Go** or **C++**? Let me know!
