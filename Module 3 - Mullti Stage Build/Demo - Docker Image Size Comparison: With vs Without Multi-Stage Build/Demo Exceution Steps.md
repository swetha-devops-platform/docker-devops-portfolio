# Docker Image Size Comparison: With vs Without Multi-Stage Build

A hands-on guide to understanding how multi-stage builds reduce Docker image size using a Python Calculator app.

---

## Prerequisites

- AWS EC2 instance (Ubuntu 22.04) or local machine
  
- Docker installed
  
- Basic knowledge of Dockerfile

---

## Project Structure

```
docker-multistage-demo/
├── calculator.py
├── Dockerfile.single        # Without multi-stage build
├── Dockerfile.multistage    # With multi-stage build
└── README.md
```

---

## 🐍 Application File

**`calculator.py`**
```python
def add(a, b): return a + b
def subtract(a, b): return a - b
def multiply(a, b): return a * b
def divide(a, b):
    if b == 0:
        raise ValueError("Cannot divide by zero!")
    return a / b

def main():
    print("=== Simple Calculator ===")
    operations = {
        "1": ("Addition",       add),
        "2": ("Subtraction",    subtract),
        "3": ("Multiplication", multiply),
        "4": ("Division",       divide),
    }
    while True:
        print("\n1.Add  2.Subtract  3.Multiply  4.Divide  5.Quit")
        choice = input("Choose: ").strip()
        if choice == "5":
            print("Goodbye!")
            break
        elif choice in operations:
            try:
                a = float(input("First number: "))
                b = float(input("Second number: "))
                name, func = operations[choice]
                print(f"Result: {func(a, b)}")
            except ValueError as e:
                print(f"Error: {e}")

if __name__ == "__main__":
    main()
```

---

## 🔴 Stage 1 — Without Multi-Stage Build

**`Dockerfile.single`**
```dockerfile
# Single stage — full Ubuntu base image
FROM ubuntu:22.04

ENV DEBIAN_FRONTEND=noninteractive

# Install Python and build tools
RUN apt-get update && \
    apt-get install -y python3 python3-pip build-essential && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

WORKDIR /app

COPY calculator.py .

CMD ["python3", "calculator.py"]
```

### Build the Single-Stage Image:
```bash
docker build -f Dockerfile.single -t calculator-single .
```

---

## 🟢 Stage 2 — With Multi-Stage Build

**`Dockerfile.multistage`**
```dockerfile
# ─── Stage 1: Builder ───────────────────────────────
FROM ubuntu:22.04 AS builder

ENV DEBIAN_FRONTEND=noninteractive

# Install Python and build dependencies
RUN apt-get update && \
    apt-get install -y python3 python3-pip build-essential && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

WORKDIR /app

COPY calculator.py .

# ─── Stage 2: Final (lean runtime image) ────────────
FROM python:3.11-slim AS final

WORKDIR /app

# Copy only the app file from builder stage
COPY --from=builder /app/calculator.py .

CMD ["python3", "calculator.py"]
```

### Build the Multi-Stage Image:
```bash
docker build -f Dockerfile.multistage -t calculator-multistage .
```

---

## 📊 Step 3: Compare Image Sizes

```bash
docker images | grep calculator
```

### Expected Output:
```
REPOSITORY                TAG       IMAGE ID       SIZE
calculator-single         latest    abc123456      174MB
calculator-multistage     latest    def789012       58MB
```

### Detailed size inspection:
```bash
# Inspect single-stage image
docker inspect calculator-single --format='{{.Size}}' | \
  awk '{printf "Single-Stage Size: %.2f MB\n", $1/1024/1024}'

# Inspect multi-stage image
docker inspect calculator-multistage --format='{{.Size}}' | \
  awk '{printf "Multi-Stage Size: %.2f MB\n", $1/1024/1024}'
```

---

## ▶️ Step 4: Run Both Containers

```bash
# Run single-stage
docker run -it calculator-single

# Run multi-stage
docker run -it calculator-multistage
```

> Both containers behave identically — only the image size differs.

---

## 📈 Size Comparison Summary

| Type                  | Base Image         | Includes          | Approx Size |
|-----------------------|--------------------|-------------------|-------------|
| ❌ Without Multi-Stage | `ubuntu:22.04`     | OS + build tools  | ~174 MB     |
| ✅ With Multi-Stage    | `python:3.11-slim` | Runtime only      | ~58 MB      |
| 💡 Size Reduction      | —                  | —                 | **~67% smaller** |

---

## 🔍 Step 5: Layer-by-Layer Analysis

```bash
# View layers of single-stage image
docker history calculator-single

# View layers of multi-stage image
docker history calculator-multistage
```

### Sample Output:
```
# calculator-single
IMAGE         CREATED BY                                SIZE
abc123        CMD ["python3" "calculator.py"]           0B
...           COPY calculator.py .                      1kB
...           RUN apt-get install python3 build-essential  160MB

# calculator-multistage
IMAGE         CREATED BY                                SIZE
def789        CMD ["python3" "calculator.py"]           0B
...           COPY --from=builder /app/calculator.py .  1kB
...           python:3.11-slim base                     45MB
```

---

## 🧹 Step 6: Cleanup

```bash
# Remove both containers
docker rm $(docker ps -aq)

# Remove both images
docker rmi calculator-single calculator-multistage

# Full system cleanup
docker system prune -a
```

---

## 💡 How Multi-Stage Build Works

```
┌─────────────────────────────────────────────────┐
│              WITHOUT Multi-Stage                 │
│                                                  │
│  ubuntu:22.04 + python3 + build-essential + app  │
│  Everything stays in the final image → LARGE     │
└─────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────┐
│               WITH Multi-Stage                   │
│                                                  │
│  Stage 1 (Builder)         Stage 2 (Final)       │
│  ubuntu + build tools  →   python:slim + app     │
│  (discarded)               only app copied →SMALL│
└─────────────────────────────────────────────────┘
```

---

## ✅ When to Use Multi-Stage Builds

| Scenario                        | Use Multi-Stage? |
|---------------------------------|------------------|
| Compiling code (Go, Java, C++)  | ✅ Yes           |
| Installing build dependencies   | ✅ Yes           |
| Simple Python script (no deps)  | ⚠️ Optional      |
| Production deployment           | ✅ Always        |
| Quick local testing             | ❌ Not needed    |

---

## 📌 Key Takeaways

- ✅ Multi-stage builds **separate build-time and run-time** environments
- ✅ Final image contains **only what's needed to run** the app
- ✅ Reduces attack surface for **security**
- ✅ Faster pulls and deployments due to **smaller size**
- ✅ No changes needed to application code

---

## 🚀 Push to GitHub

```bash
git init
git add .
git commit -m "Docker multi-stage build size comparison"
git remote add origin https://github.com/<your-username>/docker-multistage-demo.git
git branch -M main
git push -u origin main
```
