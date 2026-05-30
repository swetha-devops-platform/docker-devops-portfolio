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

# Step 1: Launch an EC2 Instance

  1. Go to **AWS Console → EC2 → Launch Instance**
  2. Choose **Ubuntu Server 22.04 LTS (Free Tier eligible)**
  3. Select instance type: `t2.micro` (Free Tier)
  4. Create or select an existing **Key Pair** (`.pem` file)
  5. Click **Launch Instance**

---

<img width="1920" height="349" alt="image" src="https://github.com/user-attachments/assets/4d6f75a2-9b42-4689-9462-767a13de08dd" />


---

# Step 2: Connect to EC2 via SSH

---

<img width="996" height="656" alt="image" src="https://github.com/user-attachments/assets/a08befd7-35e3-4f1e-82a1-dc033bb2fa64" />


---



## Step 3: Install Docker on EC2

---

<img width="578" height="65" alt="image" src="https://github.com/user-attachments/assets/d57b617a-ee6b-49bf-95fc-6c5d93c9615d" />


---

# Step 4: Clone Your GitHub Repository



### Expected Output:

---


---


## Size Comparison Summary

| Type                  | Base Image         | Includes          | Approx Size |
|-----------------------|--------------------|-------------------|-------------|
| Without Multi-Stage | `ubuntu:22.04`     | OS + build tools  |     |
| With Multi-Stage    | `python:3.11-slim` | Runtime only      |      |
| Size Reduction      | —                  | —                 | **~67% smaller** |

---

## How Multi-Stage Build Works

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



## 🚀 Push to GitHub

```bash
git init
git add .
git commit -m "Docker multi-stage build size comparison"
git remote add origin https://github.com/<your-username>/docker-multistage-demo.git
git branch -M main
git push -u origin main
```
