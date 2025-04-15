# dp-203-lab-17
# Real-Time Order Processing with Azure Stream Analytics

This project is based on Lab 17 from the DP-203: Data Engineering on Microsoft Azure course. It demonstrates how to capture, process, and store streaming sales order data using various Azure services.

## 🚀 Overview

The solution uses the following Azure components:

- **Azure Event Hubs** – for ingesting streaming data
- **Azure Stream Analytics** – to process and aggregate streaming data
- **Azure Blob Storage** – to store the processed output in JSON format

## 🛠️ Setup & Execution

### 1. Resource Provisioning
Resources were provisioned using:
- Azure Cloud Shell (PowerShell)
- An ARM template and a PowerShell script

### 2. Data Simulation
A Node.js client app sent 1000 simulated sales orders (ProductID, Quantity) to Event Hubs.

### 3. Stream Processing
An Azure Stream Analytics job was created to:
- Read from Event Hubs
- Aggregate order quantities in 10-second tumbling windows
- Output results to Azure Blob Storage in JSON format

##SS

<img width="1431" alt="Screenshot 2025-04-15 at 18 08 28" src="https://github.com/user-attachments/assets/ab571abf-56e1-4a27-af34-9365784f5d7d" />
<img width="1440" alt="Screenshot 2025-04-15 at 18 25 29" src="https://github.com/user-attachments/assets/6ec2adfe-2643-4572-8fdc-d636d902aa1c" />
<img width="1440" alt="Screenshot 2025-04-15 at 18 24 54" src="https://github.com/user-attachments/assets/151c1943-0b8f-4ea4-a624-c21477f0db01" />
<img width="1440" alt="Screenshot 2025-04-15 at 18 24 09" src="https://github.com/user-attachments/assets/2f08285d-a405-4ed9-a8a6-e17680a33cb6" />
<img width="1440" alt="Screenshot 2025-04-15 at 18 23 25" src="https://github.com/user-attachments/assets/23506b3f-a876-44de-9e79-3c7f025958c5" />
<img width="1440" alt="Screenshot 2025-04-15 at 18 21 47" src="https://github.com/user-attachments/assets/a5d41c39-cdca-4015-9a3c-49aa048a0f85" />
<img width="1440" alt="Screenshot 2025-04-15 at 18 21 30" src="https://github.com/user-attachments/assets/fa032ce9-dab1-4628-9fca-93000977045b" />
<img width="1440" alt="Screenshot 2025-04-15 at 18 19 53" src="https://github.com/user-attachments/assets/1eaa93a9-dd86-452e-9c88-90c40fe40c33" />
<img width="1440" alt="Screenshot 2025-04-15 at 18 18 29" src="https://github.com/user-attachments/assets/52b03659-f029-433e-b78b-ddcb7daee9a3" />
<img width="1440" alt="Screenshot 2025-04-15 at 18 17 33" src="https://github.com/user-attachments/assets/54d82e37-d20f-421e-8180-b289aa8b7b4b" />
<img width="1440" alt="Screenshot 2025-04-15 at 18 16 28" src="https://github.com/user-attachments/assets/cf284c1c-727d-428e-9b43-52b51cbd8cf3" />
<img width="1440" alt="Screenshot 2025-04-15 at 18 10 56" src="https://github.com/user-attachments/assets/8cd4be39-937e-4676-a8a9-eaba81305456" />
<img width="1440" alt="Screenshot 2025-04-15 at 18 09 15" src="https://github.com/user-attachments/assets/4156cbd0-4277-4bdb-9663-88388485813f" />
<img width="1431" alt="Screenshot 2025-04-15 at 18 08 28" src="https://github.com/user-attachments/assets/0b7331a3-69c9-4e2a-a71c-a083111b8412" />


### 4. Query Used

```sql
SELECT
    DateAdd(second,-10,System.TimeStamp) AS StartTime,
    System.TimeStamp AS EndTime,
    ProductID,
    SUM(Quantity) AS Orders
INTO
    [blobstore]
FROM
    [orders] TIMESTAMP BY EventProcessedUtcTime
GROUP BY ProductID, TumblingWindow(second, 10)
HAVING COUNT(*) > 1
