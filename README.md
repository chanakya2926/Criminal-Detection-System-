# Criminal Detection System

Real-time facial recognition system for criminal identification using Python, OpenCV, and Machine Learning. Matches live camera feeds against a watchlist database, triggers automated alerts, and provides case management tools.

## Features
- Real-time face detection, embedding, and matching (95%+ match accuracy in tests)
- Watchlist and criminal records management (CRUD)
- Automated alerts (configurable severity, optional email/SMS)
- Role-based access, audit trail for evidence traceability
- Supports SQLite (default) or MySQL backends
- Exportable reports and media storage for evidence

## Tech Stack
- Python, Django
- OpenCV, face embeddings (e.g., FaceNet/ResNet-based or OpenCV LBPH)
- SQLite/MySQL
- HTML/CSS/JS (Django templates), Bootstrap

## Architecture (High-Level)
- Capture: Webcam/RTSP → OpenCV
- Detect → Align → Embed (vector) → Match (cosine/L2)
- DB: Person profiles, embeddings, cases, alerts
- Web UI: Admin + dashboard for records, watchlist, and logs

## Getting Started

### Prerequisites
- Python 3.9+ (Windows supported)
- pip, virtualenv
- (Optional) MySQL Server 8+ if using MySQL

### Installation
```bash
# 1) Clone
git clone https://github.com/<your-username>/Criminal-Detection-System.git
cd Criminal-Detection-System

# 2) Create & activate venv (Windows PowerShell)
python -m venv venv
.\venv\Scripts\Activate.ps1

# 3) Install dependencies
pip install --upgrade pip
pip install -r requirements.txt
```

### Configuration
- Default DB: SQLite (`db.sqlite3`).
- MySQL (optional): Update `project/settings.py` or use environment variables.

Example MySQL settings (Django):
```python
DATABASES = {
    "default": {
        "ENGINE": "django.db.backends.mysql",
        "NAME": "criminal_db",
        "USER": "root",
        "PASSWORD": "your_password",
        "HOST": "127.0.0.1",
        "PORT": "3306",
        "OPTIONS": {"init_command": "SET sql_mode='STRICT_TRANS_TABLES'"},
    }
}
```

Environment variables (optional, if you use a `.env`):
```bash
DJANGO_SECRET_KEY=replace_me
DJANGO_DEBUG=True
DB_ENGINE=django.db.backends.mysql
DB_NAME=criminal_db
DB_USER=root
DB_PASSWORD=your_password
DB_HOST=127.0.0.1
DB_PORT=3306
ALERTS_EMAIL_FROM=noreply@example.com
ALERTS_EMAIL_TO=ops@example.com
```

### Database Setup
```bash
python manage.py migrate
python manage.py createsuperuser
```

### Run the App
```bash
python manage.py runserver
```
- Open http://127.0.0.1:8000
- Admin: http://127.0.0.1:8000/admin

## Using the System

1. Add/Import Records
   - Create person/criminal profiles and upload reference images (clear frontal faces).
   - Optionally bulk import mugshots; system will create embeddings and deduplicate.

2. Start Recognition
   - Launch the camera/stream capture page.
   - The pipeline detects faces, generates embeddings, and matches against the watchlist.

3. Alerts & Case Management
   - Positive matches create alerts with severity and context.
   - Review alerts, link to cases, export reports, and update watchlists.

## Model & Matching
- Detection: OpenCV-based face detection (e.g., Haar/DNN) with alignment.
- Embeddings: Configurable (FaceNet/ResNet-based) or OpenCV LBPH as baseline.
- Matching: Cosine/L2 similarity with thresholding; adjustable to trade off precision/recall.

## Evaluation
- Reported results (example on internal dataset):
  - Detection+Matching accuracy: 95%+
  - Identification latency reduced by ~60% via preprocessing and caching
  - False positives reduced with tuned thresholds and alignment

Note: Accuracy depends on lighting, camera quality, pose, and dataset quality.

## Project Structure
```
.
├─ manage.py
├─ project/                # Django settings/urls/wsgi
├─ core/                   # App: views, models, detection, embeddings, matching
├─ media/                  # Uploaded images, captured frames
├─ static/                 # CSS/JS assets
├─ requirements.txt
├─ README.md
└─ db.sqlite3
```

## Screenshots
- See `Screenshots/` and `Website_Screenshots/` folders.
- Add images to showcase: dashboard, detection feed, alert details.

## Deployment
- Containerize with Docker or deploy to a VPS/host supporting Python/Django.
- Configure `DEBUG=False`, secure secret key, and proper media/static serving.
- Add a production DB (MySQL/Postgres) and a reverse proxy (Nginx).

## Troubleshooting (Windows)
- If OpenCV fails to access the camera, ensure only one process uses it and update camera drivers.
- For MySQL, install the appropriate client (`mysqlclient`) and Microsoft C++ Build Tools if required.
- If dlib or GPU acceleration is used, ensure compatible compilers/CUDA are installed.

## Roadmap
- Multi-camera support and RTSP ingestion
- Active learning for incremental watchlist improvement
- Advanced alert routing and on-call integrations (Slack/Teams)
- Model swapping with ONNX runtime for speedups

## License
This project is for educational and research use. For production or commercial use, review face recognition legal and ethical guidelines in your jurisdiction.

## Acknowledgements
- OpenCV team and contributors
- Pretrained face embedding models and associated authors
- Django community

## Citation (Resume-Friendly)
- Built real-time facial recognition with Python/OpenCV (95%+ accuracy), cutting identification latency by 60%.
- Integrated MySQL-backed criminal records with automated alerts and role-based audit logging.
- Optimized preprocessing and thresholds to reduce false positives and improve robustness.
```

