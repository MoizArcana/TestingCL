# Corporate Login API - Response Mapping

This repository contains the **response mapping implementation** for the Corporate Login API.  
It is a Flask-based service that encrypts user credentials using RSA and interacts with the corporate login backend.

---

## Features

- Encrypts user credentials using RSA (`pycryptodome`)
- Supports both **Subgateway** and **Retailergateway** flows
- Provides a **web frontend** for manual testing
- Exposes a `/loginpayload` endpoint for API requests

---

## Requirements

- Python 3.9+
- Flask
- Requests
- pycryptodome

All dependencies are listed in `requirements.txt`.

---

## Docker Deployment

### Build Docker Image

\`\`\`bash
docker build -t <dockerhub-username>/cl-responsemapping:latest .
\`\`\`

### Run Container

**Example: using host port 5050 and naming the container `CorporateLogin`:**

\`\`\`bash
docker run -d -p 5050:5050 --name CorporateLogin <dockerhub-username>/cl-responsemapping:latest
\`\`\`

- Container listens on port **5050**  
- Flask binds to `0.0.0.0` to accept external requests  

---

## API Endpoint

### POST `/loginpayload`

- **Description:** Encrypts user credentials and returns a login payload along with x-hash.  
- **Content-Type:** `application/json`  
- **Body:**  

\`\`\`json
{
    "number": "<your-number-or-email>",
    "pin": "<your-pin>"
}
\`\`\`

- **Response:**

\`\`\`json
{
    "input": "<number:pin>",
    "LoginPayload": "<encrypted_payload>",
    "remote_status_code": 200,
    "remote_response": {...},
    "x-hash": "<hash>"
}
\`\`\`

---

### Example Curl

> Replace `<NUMBER>` and `<PIN>` with actual values.

\`\`\`bash
curl -X POST http://<EC2_PUBLIC_IP>:5050/loginpayload \
-H "Content-Type: application/json" \
-d '{"number": "<NUMBER>", "pin": "<PIN>"}'
\`\`\`

---

## Web Frontend

Open a browser at:

http://<EC2_PUBLIC_IP>:5050/


You can manually enter the number/email and PIN to generate login payloads.

---

## Notes

- Flask **development server** is used; consider using **Gunicorn** for production.  
- Ensure EC2 Security Group allows the exposed port (default `5050`).  
- Environment variables (like IBM client IDs/secrets) should be handled securely in production.  

---

## License

MIT License
EOF

git add README.md && git commit -m "Add README with Docker and API instructions" && git push origin CL-responsemapping

