# YouTube CRUD Operations

## Table of Contents
- [Overview](#overview)
- [Features](#features)
- [Requirements](#requirements)
- [Setup Instructions](#setup-instructions)
- [How to Use](#how-to-use)
  - [1. Authentication](#1-authentication)
  - [2. Create (Upload a Video)](#2-create-upload-a-video)
  - [3. Read (Fetch Video Details)](#3-read-fetch-video-details)
  - [4. Update (Modify Video Details)](#4-update-modify-video-details)
  - [5. Delete (Remove a Video)](#5-delete-remove-a-video)
- [Project Structure](#project-structure)
- [Contributing](#contributing)
- [License](#license)

## Overview
This project demonstrates how to perform **CRUD (Create, Read, Update, Delete)** operations on YouTube using the **YouTube Data API v3**. It is part of a final year project focused on applying CRUD functionalities on social media platforms. The project is implemented in Python using Google Colab.

## Features
- **Create**: Upload videos to a YouTube channel.
- **Read**: Retrieve details of uploaded videos.
- **Update**: Modify video metadata (title, description, category, tags, etc.).
- **Delete**: Remove videos from a YouTube channel.

## Requirements
- Python 3.x
- Google Colab (or a local Python environment)
- YouTube Data API v3 enabled
- OAuth 2.0 credentials

## Setup Instructions

### Step 1: Clone the Repository
```bash
git clone https://github.com/your-username/YouTube-CRUD-Operations.git
cd YouTube-CRUD-Operations
```

### Step 2: Install Required Packages
Make sure to install the required Python packages:
```python
!pip install google-auth google-auth-oauthlib google-auth-httplib2 google-api-python-client
```

### Step 3: Create and Configure Google API Credentials
1. Go to the [Google Developers Console](https://console.developers.google.com/).
2. Create a new project or select an existing one.
3. Navigate to **APIs & Services** > **Library**.
4. Enable the **YouTube Data API v3**.
5. Go to **Credentials** and click on **Create Credentials** > **OAuth 2.0 Client IDs**.
6. Download the `client_secret.json` file and upload it to your Colab environment.

### Step 4: Authenticate Using OAuth 2.0
- Follow the prompts in Google Colab to authenticate using your Google account.
- Ensure the scope is set to: `https://www.googleapis.com/auth/youtube.force-ssl`

## How to Use

### 1. Authentication
```python
from google_auth_oauthlib.flow import InstalledAppFlow
from googleapiclient.discovery import build

scopes = ["https://www.googleapis.com/auth/youtube.force-ssl"]
flow = InstalledAppFlow.from_client_secrets_file('client_secret.json', scopes)
creds = flow.run_console()
youtube = build('youtube', 'v3', credentials=creds)
```

### 2. Create (Upload a Video)
```python
def upload_video(youtube, video_file, title, description, category_id, tags):
    request_body = {
        "snippet": {
            "title": title,
            "description": description,
            "tags": tags,
            "categoryId": category_id
        },
        "status": {
            "privacyStatus": "public"
        }
    }
    media_file = MediaFileUpload(video_file)
    response = youtube.videos().insert(
        part="snippet,status",
        body=request_body,
        media_body=media_file
    ).execute()
    print("Video uploaded successfully. Video ID:", response['id'])
```

### 3. Read (Fetch Video Details)
```python
def get_video_details(youtube, video_id):
    response = youtube.videos().list(
        part="snippet,contentDetails,statistics",
        id=video_id
    ).execute()
    print(response)
```

### 4. Update (Modify Video Details)
```python
def update_video(youtube, video_id, title, description, category_id):
    request_body = {
        "id": video_id,
        "snippet": {
            "title": title,
            "description": description,
            "categoryId": category_id
        }
    }
    response = youtube.videos().update(
        part="snippet",
        body=request_body
    ).execute()
    print("Video updated successfully.")
```

### 5. Delete (Remove a Video)
```python
def delete_video(youtube, video_id):
    youtube.videos().delete(id=video_id).execute()
    print("Video deleted successfully.")
```

## Project Structure
```
YouTube-CRUD-Operations/
├── client_secret.json      # OAuth 2.0 credentials
├── YouTube_CRUD.ipynb      # Jupyter Notebook with full implementation
└── README.md               # Project documentation
```

## Contributing
If you wish to contribute to this project, please fork the repository, make your changes, and create a pull request.

## License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
