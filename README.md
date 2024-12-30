# CloudEndure API Documentation

![diagram.png]()

## Authentication
```
POST /api/latest/login
```
**Request:**
```json
{
    "username": "string",
    "password": "string"
}
```
**Response:** 200 OK/307 Redirect + XSRF-TOKEN cookie

## Project Operations
### List Projects
```
GET /api/latest/projects
```
**Response:**
```json
{
    "items": [
        {
            "id": "string",
            "name": "string",
            "agentInstallationToken": "string"
        }
    ]
}
```

### Get Project Details
```
GET /api/latest/projects/{project_id}
```

## Machine Management
### List Machines
```
GET /api/latest/projects/{project_id}/machines
```
**Response:**
```json
{
    "items": [
        {
            "id": "string",
            "sourceProperties": {
                "name": "string"
            }
        }
    ]
}
```

### Get Machine Status
```
GET /api/latest/projects/{project_id}/machines/{machine_id}
```
**Response:**
```json
{
    "replicationStatus": "string",
    "replicationInfo": {
        "replicatedStorageBytes": "number",
        "totalStorageBytes": "number",
        "lastConsistencyDateTime": "string",
        "backloggedStorageBytes": "number"
    }
}
```

## Replication Status Codes
- NOT_STARTED: Initial state
- STARTED: Replication in progress
- PAUSED: Replication paused
- ERROR: Replication error
- CONSISTENT: Data consistent and ready for launch

## Blueprint Management
### Get Machine Blueprint
```
GET /api/latest/projects/{project_id}/blueprints
```

### Update Blueprint
```
PATCH /api/latest/projects/{project_id}/blueprints/{blueprint_id}
```
**Request:**
```json
{
    "instanceType": "string",
    "subnetIDs": ["string"],
    "securityGroupIDs": ["string"],
    "machineId": "string"
}
```

## Agent Installation
### Download Agent
```
GET /{installer_filename}
```
Available installers:
- installer_win.exe
- installer_linux.py

### Installation Command
Windows:
```
installer_win.exe -t {installation_token} --no-prompt
```
Linux:
```
python installer_linux.py -t {installation_token} --no-prompt
```

## Launch Operations
### Launch Target Machine
```
POST /api/latest/projects/{project_id}/launchMachines
```
**Request:**
```json
{
    "items": [{"machineId": "string"}],
    "launchType": "TEST"
}
```

### Monitor Job Status
```
GET /api/latest/projects/{project_id}/jobs/{job_id}
```
**Response:**
```json
{
    "status": "string",
    "log": [{"message": "string"}]
}
```

## Error Codes
- 200: Success
- 202: Accepted (async operations)
- 307: Redirect
- 401: Unauthorized
- 404: Not Found
- 500: Server Error
