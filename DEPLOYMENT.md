# Deployment Guide

## Quick Start with Vercel

### 1. Deploy to Vercel

```bash
npm i -g vercel
vercel
```

### 2. Set Environment Variables

In Vercel Dashboard:
```
ELEVENLABS_API_KEY=your_key
PEXELS_API_KEY=your_key
ANTHROPIC_API_KEY=your_key
AWS_ACCESS_KEY_ID=your_key
AWS_SECRET_ACCESS_KEY=your_key
AWS_S3_BUCKET=your_bucket
```

### 3. Your Live Links

**Main App:** `https://your-project.vercel.app`
**Embed Link:** `https://your-project.vercel.app/embed`

---

## Docker Deployment

### Build Docker Image

```dockerfile
FROM node:18-alpine

WORKDIR /app

COPY package*.json ./
RUN npm install

# Install FFmpeg
RUN apk add --no-cache ffmpeg

COPY . .

RUN npm run build

EXPOSE 5000

CMD ["npm", "start"]
```

### Deploy

```bash
docker build -t video-making-machine .
docker run -p 5000:5000 -e ELEVENLABS_API_KEY=your_key video-making-machine
```

---

## AWS Deployment

### Using AWS App Runner

1. Push to GitHub
2. Connect to App Runner
3. Set environment variables
4. Deploy!

### Using EC2

```bash
ssh ec2-user@your-instance

# Install Node.js
curl -fsSL https://rpm.nodesource.com/setup_18.x | sudo bash -
sudo yum install -y nodejs

# Install FFmpeg
sudo yum install -y ffmpeg

# Clone and setup
git clone https://github.com/m35014762-ctrl/video-making-machine
cd video-making-machine

npm install
npm run build

# Use PM2 for process management
npm i -g pm2
pm2 start dist/server.js --name "video-maker"
```

---

## Heroku Deployment

```bash
heroku create your-app-name
git push heroku main
heroku config:set ELEVENLABS_API_KEY=your_key
```

---

## Railway Deployment

1. Connect GitHub repo to Railway
2. Set environment variables
3. Deploy!

**Live URL:** `https://your-project.railway.app`

---

## Getting API Keys

### ElevenLabs (Text-to-Speech)
1. Visit: https://elevenlabs.io
2. Sign up and create API key
3. Cost: ~$5-50/month depending on usage

### Pexels (Background Videos)
1. Visit: https://pexels.com/api
2. Get free API key
3. Unlimited free usage

### AWS S3 (Optional Video Storage)
1. Create AWS account
2. Create S3 bucket
3. Generate access keys
4. Cost: ~$0.023/GB stored

---

## Production Checklist

- [ ] All API keys configured
- [ ] FFmpeg installed on server
- [ ] S3 bucket created (if using)
- [ ] Security headers configured
- [ ] Rate limiting enabled
- [ ] Error monitoring (Sentry)
- [ ] Analytics enabled
- [ ] Domain configured
- [ ] SSL certificate installed
- [ ] Backup strategy in place

---

## Troubleshooting

### FFmpeg not found
```bash
# Install FFmpeg
# Ubuntu/Debian
sudo apt-get install ffmpeg

# macOS
brew install ffmpeg

# Windows
choco install ffmpeg
```

### Video generation fails
1. Check API keys are valid
2. Verify FFmpeg is installed
3. Check disk space
4. Review server logs

### Videos not uploading to S3
1. Verify AWS credentials
2. Check S3 bucket exists
3. Verify bucket permissions
4. Check AWS region is correct
