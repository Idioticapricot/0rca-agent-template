# Agent Server

A Flask-based agent server that handles job requests and stores execution state in SQLite.

## Features

- **Job Management**: Create and track job execution
- **SQLite Storage**: Local database for job state and logs
- **Docker Support**: Containerized deployment
- **REST API**: Simple HTTP endpoints for job operations

## API Endpoints

### POST /start_job
Creates a new job and returns job details.

**Request:**
```json
{
  "job_input": "Translate hello → Spanish"
}
```

**Response:**
```json
{
  "job_id": "J1762668254",
  "unsigned_group_txn": "bW9ja190eG5fSjE3NjI2NjgyNTQ=",
  "price": 0.1
}
```

### GET /job/{job_id}
Returns job status and results.

**Response:**
```json
{
  "job_id": "J1762668254",
  "status": "queued",
  "created_at": 1762668254,
  "output": null
}
```

## Database Schema

### jobs_local
- `job_id` - Unique job identifier
- `job_input` - Original user input
- `job_input_hash` - SHA256 hash for verification
- `status` - queued | running | succeeded | failed
- `output` - Job result

### receipts_local
- Chain verification records

### logs_local
- Execution logs for debugging

## Quick Start

### Docker
```bash
docker build -t agent-server .
docker run -p 8000:8000 agent-server
```

### Local Development
```bash
pip install -r requirements.txt
python app.py
```

Server runs on `http://localhost:8000`

## Testing

```bash
# Create job
curl -X POST http://localhost:8000/start_job \
  -H "Content-Type: application/json" \
  -d '{"job_input": "Translate hello → Spanish"}'

# Check status
curl http://localhost:8000/job/J1762668254
```