# **Signify – Signature Verification System**

Secure AI-powered signature verification using Flask, TensorFlow (ResNet50), and MongoDB Atlas.

This project provides user and admin authentication, signature comparison using deep learning, verification history logs, and a structured API for integration.

---

##  **Features**

###  **User Authentication**

* User registration & login
* Admin login with auto-creation of default admin from environment variables
  (from **app.py** )

###  **Signature Verification (AI Model)**

* Uses **ResNet50 pretrained on ImageNet** for feature extraction
  (from **model_loader.py** )
* Computes cosine similarity between two signatures
  (from **helpers.py** )
* Threshold-based match detection

###  **Verification Logs**

* User and Admin can view logs
* Each log stores:

  * Uploaded file names
  * Confidence score
  * Match/No-match result
  * Execution time
  * Timestamp
    (from **routes.py** )

###  **MongoDB Atlas Integration**

* Environment-based configuration
  (from **.env** example )
* Auto-admin creation
* Stores users and verification logs

###  **REST API Endpoints**

Full API support for login, register, verify, and logs.

---

##  **Project Structure**

```
project/
│── app.py               # Main Flask app & DB initialization
│── routes.py            # All routes & API endpoints
│── helpers.py           # Signature verification logic
│── model_loader.py      # Loads global ResNet50 model
│── requirements.txt     # Dependencies
│── runtime.txt          # Python runtime version (Heroku)
│── .env                 # Environment variables (not committed)
│── uploads/             # Temporary uploaded images
│── templates/           # HTML pages (login, dashboard etc.)
```

---

## ⚙️ **Installation & Setup**

###  **Clone the repository**

```sh
git clone https://github.com/your-username/signify.git
cd signify
```

---

###  **Create Virtual Environment**

```sh
python -m venv venv
source venv/bin/activate   # Linux/Mac
venv\Scripts\activate      # Windows
```

---

###  **Install Dependencies**

```sh
pip install -r requirements.txt
```

(From **requirements.txt** )

---

###  **Configure Environment Variables**

Create a `.env` file:

```env
SECRET_KEY=your_secret_key
ATLASDB_URL=your_mongodb_connection_string
ADMIN_USERNAME=admin
ADMIN_PASSWORD=admin@123
```

Reference from your provided `.env` 

---

###  **Run the App**

```sh
python app.py
```

App will start at:

```
http://127.0.0.1:5000
```

---

##  **API Endpoints**

### **Auth**

| Method | Endpoint    | Description      |
| ------ | ----------- | ---------------- |
| POST   | `/register` | Create new user  |
| POST   | `/login`    | Login user/admin |
| GET    | `/logout`   | Logout           |

---

### **Verification**

| Method | Endpoint  | Description                      |
| ------ | --------- | -------------------------------- |
| POST   | `/verify` | Upload two signatures and verify |

Body (form-data):

```
original: file
test: file
```

Response:

```json
{
  "confidence": 97.52,
  "match": true,
  "time": 0.214
}
```

---

### **User Logs**

| Method | Endpoint               |
| ------ | ---------------------- |
| GET    | `/user/logs`           |
| DELETE | `/user/logs?id=LOG_ID` |

---

### **Admin Logs**

| Method | Endpoint                |
| ------ | ----------------------- |
| GET    | `/admin/logs`           |
| DELETE | `/admin/logs?id=LOG_ID` |

---

##  **How Signature Verification Works**

1. Images resized to **224×224**
2. Features extracted using **ResNet50 (pooling=avg)**
   Source: **model_loader.py** 
3. Features normalized
4. **Cosine similarity** calculated
5. Threshold = **0.75** → above means *Genuine*, below means *Forged*
   Source: **helpers.py** 

---

##  Deployment Ready

Included:

* `runtime.txt` for Heroku (Python 3.10.13) 
* Environment-based config
* Gunicorn support (in requirements)

---
