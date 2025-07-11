# task-3 : Cloud Computing

**Name** : CODTECH IT SOLUTIONS

**Name** : Khushboo Vijay Patil

**Intern ID** : CT06DF672

**Domain** : Cloud Computing

**Duration** : 6 Weeks 

**Mentor** : Neela Santosh

**Description of Task**

### 🔁 AWS (for frontend + DB)

### 🔁 GCP (for backend API)

We’ll go through everything in a **live-deploy-ready format**, with commands, screenshots suggestions, and sample code.

---

## ✅ What We’ll Build Practically

| Layer       | Cloud | Service         | Purpose                     |
| ----------- | ----- | --------------- | --------------------------- |
| Frontend    | AWS   | S3 + CloudFront | Host static React/HTML app  |
| Backend API | GCP   | Cloud Run       | Node.js/Express backend API |
| Database    | AWS   | RDS (MySQL)     | Store app data              |

---

## 🧩 Part 1: Setup AWS for Frontend & Database

---

### ✅ 1.1 Create & Upload Frontend (React/HTML) to S3

#### 🔧 Terminal (on your laptop):

```bash
npx create-react-app myapp
cd myapp
npm run build
```

#### 🔧 AWS Console:

1. Go to **S3 → Create bucket**

   * Name: `kp-frontend-bucket`
   * Region: `us-east-1`
2. **Uncheck** “Block all public access”
3. Go to **Properties → Enable Static Website Hosting**

   * Index: `index.html`
4. Upload the entire `build/` folder

---

### ✅ 1.2 Set Up CloudFront to Serve the Frontend

#### 🔧 AWS Console:

1. Go to **CloudFront → Create Distribution**
2. **Origin domain:** Select the S3 bucket
3. **Viewer Protocol Policy:** Redirect HTTP to HTTPS
4. **Default Root Object:** `index.html`
5. **Cache Policy:** Use `CachingOptimized`
6. Click **Create**

→ After a few minutes, you'll get a **CloudFront URL**.

---

### ✅ 1.3 Create AWS RDS (MySQL) for the Backend

#### 🔧 AWS Console:

1. Go to **RDS → Create Database**
2. Select **MySQL**
3. Set:

   * DB Name: `myapp_db`
   * Username: `admin`
   * Password: `MySecurePwd123`
4. **Public access:** Yes (temporarily)
5. Click **Create**
6. Once live → copy the **Endpoint**

---

## ☁️ Part 2: Setup GCP for Backend API

---

### ✅ 2.1 Create Node.js Backend

#### 🔧 `index.js`

```js
const express = require('express');
const mysql = require('mysql2');
const app = express();
const port = process.env.PORT || 8080;

const db = mysql.createConnection({
  host: 'your-rds-endpoint.amazonaws.com',
  user: 'admin',
  password: 'MySecurePwd123',
  database: 'myapp_db'
});

app.get('/', (req, res) => {
  db.query('SELECT NOW()', (err, result) => {
    if (err) return res.status(500).send(err);
    res.send(`Backend working! DB time: ${result[0]['NOW()']}`);
  });
});

app.listen(port, () => {
  console.log(`App running on port ${port}`);
});
```

#### 🔧 `Dockerfile`

```Dockerfile
FROM node:18
WORKDIR /app
COPY . .
RUN npm install
CMD ["node", "index.js"]
```

---

### ✅ 2.2 Deploy to Cloud Run

#### 🔧 Cloud Shell or Terminal:

```bash
gcloud auth login
gcloud config set project your-gcp-project-id

gcloud builds submit --tag gcr.io/your-gcp-project-id/myapp-api

gcloud run deploy myapp-api \
  --image gcr.io/your-gcp-project-id/myapp-api \
  --platform managed \
  --allow-unauthenticated \
  --region us-central1
```

✅ After deployment → You’ll get a **Cloud Run URL**

---

## 🔗 Part 3: Connect Frontend (AWS) → Backend (GCP)

---

### ✅ 3.1 Modify React Code

#### 🔧 `src/App.js`

```js
import React, { useEffect, useState } from 'react';

function App() {
  const [data, setData] = useState('');

  useEffect(() => {
    fetch('https://your-cloud-run-url.a.run.app')
      .then(res => res.text())
      .then(data => setData(data));
  }, []);

  return <div>{data || "Loading..."}</div>;
}

export default App;
```

### ✅ 3.2 Rebuild and Re-upload to S3

```bash
npm run build
```

→ Upload new `build/` to S3 bucket.

---

## 🔐 Part 4: Secure & Monitor

---

### ✅ 4.1 Security

* Use **HTTPS (CloudFront + Cloud Run)**
* Use **CORS** if needed on backend
* Use **IAM roles** and **API key** if private

---

### ✅ 4.2 Monitoring

* **AWS CloudWatch** → For RDS + S3 logs
* **GCP Monitoring** → Enable for Cloud Run

---

## 🧪 Part 5: Final Test & Output

1. Open **CloudFront URL**
2. React app loads → calls backend API on GCP
3. GCP backend connects to AWS RDS and returns data

---

## ✅ Result: Working Multi-Cloud App

* AWS → Frontend + DB
* GCP → Backend API
* Interoperable and secure
* Cloudfront- https://d1z5sfs5o5ey06.cloudfront.net
* Bucket website endpoint - http://kp-frontend.s3-website.ap-south-1.amazonaws.com

## OUTPUT

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/468e2060-ff18-4f3a-8818-03efc8952e87" />
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/3a75a550-c632-427b-9219-f4637deb6e4b" />
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/2f801a7e-a32d-4a9d-81de-bb3e703bfd91" />
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/be1d3aed-a003-47e7-ac9a-8eaf635c2a45" />
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/905c55ed-5b08-4e56-820f-e56bab5e3863" />
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/4b2d5ce9-c1d6-495a-95ae-765ada4ed3d3" />
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/cbb29d2a-d692-453b-8141-bd768b7c4951" />
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/b5c355de-4819-4b56-b88f-2158a8d80b4d" />
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/a5f41737-28cf-4fca-9eea-378f7fd00b60" />
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/01815684-f02a-4852-8d97-7c61a22458a8" />
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/fc9e4eb7-0e63-47c8-af0c-955ca631bbd5" />
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/b824f509-4aca-48e8-92c1-4e824d4d8d9f" />
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/1219f637-fc15-4474-842b-14af45ba106b" />
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/48b7538f-b002-464f-aa09-6e7aa510e858" />
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/2de64c20-9b22-4042-83bb-6d5d72d7bae7" />

## DOCUMENTATION
[Multi Cloud Project Report.docx](https://github.com/user-attachments/files/21181819/Multi.Cloud.Project.Report.docx)
