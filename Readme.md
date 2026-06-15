# WSO2 API Manager Demo

This project demonstrates a microservices architecture integrated with **WSO2 API Manager 4.4.0**. It includes three Node.js backend services (Patient, Doctor, and Appointment) managed through a central API Gateway.

## Architecture Overview

- **WSO2 APIM**: Acts as the Control Plane and Data Plane (Gateway).
- **Patient API**: Express service running on port `3001` (Host) / `3000` (Container).
- **Doctor API**: Express service running on port `3002` (Host) / `3000` (Container).
- **Appointment API**: Express service running on port `3003` (Host) / `3000` (Container).

## Prerequisites

- Docker and Docker Compose installed.
- Minimum 4GB of RAM allocated to Docker (WSO2 APIM is resource-intensive).

## Getting Started

1. **Start the environment**:
   ```bash
   docker compose up -d
   ```

2. **Access the Consoles**:
   Wait about 2-3 minutes for WSO2 APIM to fully initialize.
   - **Publisher Portal**: https://localhost:9443/publisher (To design and publish APIs)
   - **Developer Portal**: https://localhost:9443/devportal (To discover and subscribe)
   - **Admin Console**: https://localhost:9443/admin (Used for platform administration)
   
   **Default Credentials**: 
   - Username: `admin`
   - Password: `admin`

---

## Guide: How to use WSO2 APIM

### 1. Create and Publish an API
1. Log in to the **Publisher Portal**.

![Publisher Login Screen](./img/1.jpeg)

2. Click **REST API** > **Start From Scratch**.
3. Provide the details:
   - **Name**: `PatientAPI`
   - **Context**: `/patient/v1`
   - **Version**: `1.0.0`
   - **Endpoint**: `http://patient-api:3000` (Note: We use the Docker container name here).

![API Details](./img/2.jpeg)

4. Click **Create**.

![API Overview](./img/3.jpeg)

5. Navigate to **Resources** to define your paths (e.g., `/hello` ).

![API Resources](./img/4.jpeg)
![API Resources](./img/5.jpeg)


6. Navigate to **Endpoints** and ensure the Production/Sandbox URLs are set to `http://patient-api:3000`.

7. Go to **Deployments** and click **Deploy New Revision**.
![API Endpoints](./img/7.jpeg)

7. Go to **Lifecycle** and check the status of the deployment.
![API Endpoints](./img/6.jpeg)

8. In this way add all other apis as well.


### 2. Subscribe to the API
1. Log in to the **Developer Portal**. You could see the available apis to be subscribed here.
![Developer Portal](./img/8.jpeg)

2. Click on the `PatientAPI` you just published.
3. Go to **Subscriptions**.
![Subscriptions](./img/9.jpeg)
4. Click **Subscriptions** > **Add New Application**.
![Add Application](./img/10.jpeg)
![Add Application](./img/11.jpeg)
5. Name it `DemoApp` and click **Save**.
6. Select `DemoApp` and click **Subscribe**.
7. Like that subscribe to all three services in the DemoApp
![DemoApp Subscribtions](./img/12.jpeg)

### 3. Generate Keys and Invoke
1. While in the Developer Portal, go to **Applications** > **DemoApp**.
2. Click **Sandbox Keys** > **Generate Keys**.
![Sandbox Keys](./img/13.jpeg)

2. And you can generate Production keys as well.
Click **Production Keys** > **Generate Keys**.
![Production Keys](./img/14.jpeg)

3. Click **Generate Access Token**. Copy the token provided.

![Access Token](./img/15.jpeg)



4. Use the **Try Out** console within the portal or use `curl`:
   ```bash
   curl -X GET "http://localhost:8280/patient/v1/hello" \
        -H "Authorization: Bearer <YOUR_ACCESS_TOKEN>"
   ```

---

## Backend Service Endpoints (Internal)

If you want to test the backend services directly bypassing the Gateway:
| Service | Host URL | Internal Docker URL |
| :--- | :--- | :--- |
| Patient API | `http://localhost:3001` | `http://patient-api:3000` |
| Doctor API | `http://localhost:3002` | `http://doctor-api:3000` |
| Appointment API | `http://localhost:3003` | `http://appointment-api:3000` |

## Troubleshooting

### Port Conflicts
If you see an error regarding `0.0.0.0:3000`, ensure no local Node.js processes are running on your host machine.

### WSO2 Startup Issues
WSO2 requires significant resources. If the container exits, check the logs:
```bash
docker logs -f wso2apim
```
Ensure your Docker Desktop settings have at least 8GB of Memory and 4 CPUs allocated for a smooth experience.

### Endpoint Connectivity
Inside WSO2 APIM, always use the **container_name** (e.g., `http://doctor-api:3000`) as the endpoint address. `localhost` inside WSO2 refers to the WSO2 container itself, not your computer.

```