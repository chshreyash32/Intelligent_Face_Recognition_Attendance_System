# 🎓 Intelligent Face Recognition Attendance System

**An AI-powered, real-time attendance management system using face recognition.**  
Built as a B.Tech CSE (AIML) academic project — Gokaraju Rangaraju Institute of Engineering and Technology.

---

## 📌 Problem Statement

Traditional attendance systems are:
- ❌ Time-consuming and manual
- ❌ Prone to proxy attendance
- ❌ Hard to track subject-wise records

This system solves all three by automatically recognizing student faces and marking attendance in real time — with zero manual effort from students.

---

## 🚀 System Flow

```
Student Registers → Captures 30 Face Images → Model Auto-Trains
                                  ↓
Faculty Starts Session → Webcam Detects Faces → Attendance Auto-Marked
                                  ↓
              CSV Database Updated in Real Time → Student Views Records
```

---

## ✨ Features

### 👨‍🎓 Student Portal
| Feature | Description |
|---|---|
| Face Registration | Captures 30 images via webcam and auto-trains the model |
| Attendance View | Enter roll number to view subject-wise records |
| Percentage Tracking | Calculates attendance % per subject automatically |

### 👨‍🏫 Faculty Portal
| Feature | Description |
|---|---|
| Secure Login | 4-digit Faculty ID authentication |
| Session Setup | Select subject, section, and number of periods |
| Live Attendance | Real-time face recognition with on-screen logs |
| Multi-Period Support | Marks multiple periods in a single session |

### 🤖 AI Engine
| Feature | Description |
|---|---|
| Face Detection | Haar Cascade Classifier (OpenCV built-in) |
| Face Recognition | LBPH (Local Binary Pattern Histogram) Algorithm |
| Confidence Threshold | Accepts match only if confidence score < 65 |
| Anti-Duplicate | 60-second cooldown per student per session |
| Unknown Rejection | Unrecognized faces are ignored automatically |

---

## 🛠️ Tech Stack

| Layer | Technology | Purpose |
|---|---|---|
| Language | Python 3.8+ | Core application |
| Web UI | Streamlit | Interactive dashboard & routing |
| Computer Vision | OpenCV (`cv2`) | Face detection & recognition |
| ML Model | LBPH Face Recognizer | Face recognition algorithm |
| Data Handling | Pandas, NumPy | CSV storage & data processing |
| Image Processing | PIL (Pillow) | Grayscale image conversion |
| Storage | CSV + JSON | Attendance DB & ID mapping |

---

## 📁 Project Structure

```
project/
│
├── model/                      # Trained model storage
├── trainer/                    # LBPH trainer output files
│
├── app.py                      # Main Streamlit application
├── collect_data.py             # Face image data collection
├── train_model.py              # Model training script
├── attendance_system.py        # Core attendance logic
├── dashboard.py                # UI dashboard components
├── attendance_summary.py       # Attendance reports & analytics
│
├── attendance.csv              # Attendance database (auto-created)
├── attendance_db.csv           # Attendance backup/extended DB
├── student_attendance.csv      # Student-specific attendance records
├── student_map.json            # Internal ID to Roll number mapping
├── student_embeddings.npz      # Stored face embeddings
├── labels.npy                  # Label data for recognition model
│
├── requirements.txt            # Python dependencies
└── README.md
```

> **Note:** `model/`, `trainer/`, CSV files, and `student_map.json` are generated automatically on first run. You do not need to create them manually.

---

## ⚙️ Installation & Setup

### Prerequisites
- Python 3.8 or above
- A working webcam

### Step 1 — Clone the Repository
```bash
git clone https://github.com/yourusername/face-attendance-system.git
cd face-attendance-system
```

### Step 2 — Install Dependencies
```bash
pip install -r requirements.txt
```

**requirements.txt**
```
streamlit
opencv-contrib-python-headless
numpy
pandas
pillow
streamlit-webrtc
av
```

> ⚠️ `opencv-contrib-python-headless` is used here (no GUI window dependency). `streamlit-webrtc` and `av` enable browser-based webcam streaming.

### Step 3 — Run the App
```bash
streamlit run app.py
```

The app opens automatically at `http://localhost:8501`

---

## 🔄 How to Use

### For Students

**1. Register (First Time Only)**
- Go to **Student Portal → Register Face**
- Enter Full Name, Roll Number, and Section
- Allow webcam access — 30 face images are captured automatically
- The recognition model trains itself — registration complete ✅

**2. View Attendance**
- Go to **Student Portal → View Attendance**
- Enter your Roll Number
- View subject-wise attendance, total classes held, and percentage

### For Faculty

**1. Login**
- Go to **Faculty Login**
- Enter a 4-digit numeric Faculty ID (password = same as ID in demo)

**2. Take Attendance**
- Select Subject, Section, and Number of Periods
- Click **Start Live Attendance**
- The webcam starts — recognized students are marked automatically
- Real-time logs appear on screen
- Click **Stop** when the session ends

---

## 🧠 How the AI Works

```
Webcam Frame
    ↓
Haar Cascade Classifier → Detects face region (bounding box)
    ↓
Crop + Convert to Grayscale → Preprocessed face patch
    ↓
LBPH Recognizer → Predicts student ID + confidence score
    ↓
Confidence < 65 → Valid match → Fetch roll number from student_map.json
    ↓
60-second cooldown check → Mark attendance in CSV
```

**LBPH (Local Binary Pattern Histogram)** works by analyzing local texture patterns in face images. It is lightweight, fast, and works well under varying lighting — making it ideal for real-time classroom use.

---

## 📊 Data Storage

### `attendance.csv`
| RollNo | Name | Subject | Section | Held | Attended | LastUpdated |
|---|---|---|---|---|---|---|
| 21A91A0501 | Shreyash | Machine Learning | Section A | 10 | 9 | 14:35:22 |

### `student_map.json`
```json
{
  "1": "21A91A0501",
  "2": "21A91A0502"
}
```
Maps internal OpenCV integer IDs to actual student roll numbers.

---

## ⚡ Configuration

Key parameters you can customize in `app.py`:

```python
# Add or remove subjects
SUBJECT_LIST = ["Machine Learning", "Big Data Analytics", "Software Engineering", ...]

# Add or remove sections
SECTION_LIST = ["Section A", "Section B", "Section C"]

# Recognition strictness — lower value = stricter matching
if conf < 65:   # Adjust confidence threshold here

# Anti-duplicate cooldown in seconds
if (now - last_marked[real_roll]).seconds > 60:
```

---

## 🔐 Authentication

Faculty login requires a 4-digit numeric ID. In this demo build, the password matches the ID.

> ⚠️ **Demo only.** For production use, replace with a proper authentication system using hashed passwords and a database-backed login.

---

## 🚀 Future Improvements

- [ ] Deep learning face recognition (FaceNet / MTCNN) for higher accuracy
- [ ] Cloud database (Firebase / PostgreSQL) instead of local CSV
- [ ] Anti-spoofing — detect photo-based attacks
- [ ] Low attendance email/SMS alerts
- [ ] Admin panel for managing students and faculty
- [ ] Export attendance reports as PDF or Excel
- [ ] Mobile-friendly deployment (Streamlit Cloud / Docker)

---

## 🎯 Skills Demonstrated

| Skill | How It Appears in This Project |
|---|---|
| Computer Vision | Real-time face detection and recognition pipeline using OpenCV |
| Machine Learning | LBPH model training, inference, and confidence-based filtering |
| Web Development | Full Streamlit app with multi-page session-state routing |
| Data Engineering | CSV/JSON-based persistent storage managed with Pandas |
| Software Design | Modular page architecture, clean separation of logic and UI |
| UI/UX Design | Custom dark theme via injected CSS inside Streamlit |

---

## 👨‍💻 Author

**Shreyash Chintalwar**  
B.Tech CSE(AIML)
Gokaraju Rangaraju Institute of Engineering and Technology


---

> *Built to explore real-world applications of AI, Computer Vision, and Web Development.*  
> *Open to feedback, contributions, and collaboration.*
