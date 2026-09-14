# AK EDITZ STUDIO - API INTEGRATION GUIDE

## Quick Start

### 1. Deploy Backend Server

#### Option A: Vercel Deployment
```bash
# Install Vercel CLI
npm i -g vercel

# Deploy
vercel

# Set environment variables in Vercel Dashboard
```

#### Option B: Railway Deployment
```bash
# Connect GitHub repo to Railway
# Set environment variables in Railway Dashboard
```

#### Option C: Local Development
```bash
# Install dependencies
npm install

# Create .env file
cp .env.example .env

# Add your API keys to .env

# Start server
npm start
```

---

## 2. Add Widget to Your Website

### Simple Installation

Add this single line to your HTML:

```html
<!-- In your website HTML -->
<div id="ak-editz-widget"></div>
<script src="https://your-deployed-server/embed.js"></script>
```

### Configure API URL

```html
<script>
    window.AK_EDITZ_CONFIG = {
        apiUrl: 'https://your-deployed-server.com'
    };
</script>
<div id="ak-editz-widget"></div>
<script src="https://your-deployed-server/embed.js"></script>
```

---

## 3. API Endpoints

### Create Video
```
POST /api/render-video
Content-Type: application/json

{
    "prompt": "Create a motivational video about success",
    "duration": 5,
    "voiceText": "Success is a journey, not a destination",
    "keywords": ["motivation", "success"],
    "quality": "720"
}

Response:
{
    "jobId": "uuid",
    "message": "Video rendering started",
    "statusUrl": "/api/job-status/uuid",
    "downloadUrl": "/api/download/uuid"
}
```

### Check Status
```
GET /api/job-status/:jobId

Response:
{
    "id": "uuid",
    "status": "rendering-video",
    "progress": 75,
    "error": null,
    "downloadUrl": "/api/download/uuid"
}

Status Values:
- processing
- generating-script
- downloading-video
- generating-voice
- rendering-video
- finalizing
- completed
- error
```

### Download Video
```
GET /api/download/:jobId

Returns: MP4 video file
```

### Preview Script
```
POST /api/preview
Content-Type: application/json

{
    "prompt": "Make a video about..."
}

Response:
{
    "script": "Generated video script...",
    "preview": {
        "duration": 5,
        "hasAudio": true,
        "hasWatermark": true
    }
}
```

### List Voices
```
GET /api/voices

Response:
[
    {
        "id": "voice_id",
        "name": "Voice Name",
        "preview": "https://url-to-preview.mp3"
    }
]
```

---

## 4. Get API Keys

### ElevenLabs (Voice)
1. Visit https://elevenlabs.io
2. Sign up and create API key
3. Cost: ~$5-50/month

### Pexels (Background Videos)
1. Visit https://pexels.com/api
2. Get free API key
3. Unlimited free usage

### Anthropic Claude (Script Generation)
1. Visit https://console.anthropic.com
2. Create API key
3. Cost: ~$0.003 per 1K input tokens

---

## 5. Environment Variables

Create `.env` file:

```
ELEVENLABS_API_KEY=your_key_here
PEXELS_API_KEY=your_key_here
ANTHROPIC_API_KEY=your_key_here
PORT=5000
NODE_ENV=production
```

---

## 6. Example: Complete Website Integration

```html
<!DOCTYPE html>
<html>
<head>
    <title>My Video Studio</title>
</head>
<body>
    <h1>Welcome to AK EDITZ</h1>
    
    <!-- Video Widget -->
    <div id="ak-editz-widget"></div>

    <!-- Configure and Load -->
    <script>
        window.AK_EDITZ_CONFIG = {
            apiUrl: 'https://ak-editz-api.vercel.app'
        };
    </script>
    <script src="https://ak-editz-api.vercel.app/embed.js"></script>
</body>
</html>
```

---

## 7. JavaScript API (Advanced)

```javascript
// Custom video creation
fetch('https://your-server/api/render-video', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
        prompt: 'Create a marketing video',
        duration: 10,
        voiceText: 'Check out our amazing product',
        quality: '1080'
    })
})
.then(r => r.json())
.then(data => {
    console.log('Job ID:', data.jobId);
    
    // Poll for status
    const checkStatus = setInterval(async () => {
        const status = await fetch(`/api/job-status/${data.jobId}`).then(r => r.json());
        console.log('Progress:', status.progress + '%');
        
        if (status.status === 'completed') {
            clearInterval(checkStatus);
            window.location.href = status.downloadUrl;
        }
    }, 1000);
})
```

---

## 8. Deployment Checklist

- [ ] All API keys configured
- [ ] Environment variables set
- [ ] FFmpeg installed on server
- [ ] CORS enabled
- [ ] Rate limiting configured
- [ ] Error monitoring (Sentry)
- [ ] Analytics enabled
- [ ] SSL certificate installed
- [ ] Backup strategy in place

---

## 9. Troubleshooting

### "Video rendering failed"
- Check API keys are valid
- Verify FFmpeg is installed
- Check disk space
- Review server logs

### "CORS error"
- Ensure CORS is enabled in server
- Check origin is allowed
- Verify Content-Type headers

### "Voice not generating"
- Verify ElevenLabs API key
- Check text length
- Try different voice ID

### "Video too large"
- Use lower quality setting
- Reduce duration
- Check bitrate settings

---

## 10. Support

- **GitHub Issues**: https://github.com/m35014762-ctrl/video-making-machine
- **Email**: support@akeditz.com
- **Discord**: https://discord.gg/akeditz

---

**Happy Video Making! 🎬**
