# Talking Avatar AI 🎭

A full-stack AI-powered application that transforms photos into talking avatars with synchronized audio. Built with Next.js, Python microservices, and cutting-edge AI models.

## 🌟 Features

- **Photo to Video**: Convert static photos into animated talking avatars
- **Text-to-Speech**: Generate natural-sounding speech from text using Chatterbox TTS
- **Voice Cloning**: Clone voices from audio samples for personalized avatars
- **Video Translation**: Translate videos to different languages
- **Audio Replacement**: Change audio tracks in existing videos
- **Credit System**: Integrated payment system with Polar for managing user credits
- **Real-time Processing**: Background job processing with Inngest
- **Secure Storage**: AWS S3 integration for media file storage

## 🏗️ Architecture

![Architecture Diagram](./architecture-diagram.png)

### System Components

```
┌─────────────────────────────────────────────────────────────────┐
│                         Frontend (Next.js)                      │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐         │
│  │   UI/UX      │  │   Auth       │  │   Dashboard  │         │
│  │  Components  │  │ (Better Auth)│  │   & Modals   │         │
│  └──────────────┘  └──────────────┘  └──────────────┘         │
│                                                                  │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │            Inngest (Background Jobs)                     │  │
│  │  - Photo to Video Processing                             │  │
│  │  - Video Translation                                     │  │
│  │  - Audio Replacement                                     │  │
│  └──────────────────────────────────────────────────────────┘  │
└────────────┬──────────────────────┬──────────────────┬─────────┘
             │                      │                  │
             ▼                      ▼                  ▼
┌────────────────────┐  ┌──────────────────┐  ┌───────────────┐
│   PostgreSQL DB    │  │   AWS S3         │  │  FAL AI       │
│   (Prisma ORM)     │  │   (Media Files)  │  │  (AI Avatar)  │
└────────────────────┘  └──────────────────┘  └───────────────┘
             │
             │
             ▼
┌─────────────────────────────────────────────────────────────────┐
│                   Python Microservices (Modal)                  │
│  ┌──────────────────┐  ┌──────────────────┐  ┌──────────────┐ │
│  │ Text-to-Speech   │  │ Photo-to-Video   │  │ File-to-S3   │ │
│  │ (Chatterbox TTS) │  │ (Hallo3 Model)   │  │              │ │
│  │ - Voice Cloning  │  │ - Avatar Gen     │  │ - Upload Mgmt│ │
│  │ - TTS Generation │  │ - Lip Sync       │  │              │ │
│  └──────────────────┘  └──────────────────┘  └──────────────┘ │
└─────────────────────────────────────────────────────────────────┘
```

## 🛠️ Tech Stack

### Frontend
- **Next.js 15** - React framework with App Router
- **React 19** - UI library
- **TypeScript** - Type safety
- **Tailwind CSS** - Styling
- **Shadcn/UI** - UI components
- **Prisma** - Database ORM
- **Better Auth** - Authentication
- **Inngest** - Background job processing
- **FAL AI** - AI avatar generation

### Backend
- **Python 3.10+** - Core language
- **Modal** - Serverless deployment platform
- **FastAPI** - API framework
- **Chatterbox TTS** - Text-to-speech model
- **Hallo3** - Photo-to-video AI model
- **PyTorch** - Deep learning framework

### Infrastructure
- **PostgreSQL** - Primary database
- **AWS S3** - Media file storage
- **Polar** - Payment processing

## 📋 Prerequisites

Before you begin, ensure you have the following installed:

- **Node.js** (v18 or higher)
- **Python** (v3.10 or higher)
- **PostgreSQL** database
- **AWS Account** (for S3)
- **Modal Account** (for Python microservices)
- **FAL AI Account** (for AI avatar generation)
- **Polar Account** (for payment processing)

## 🚀 Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/Naman16rajani/calhack-12.git
cd calhack-12
```

### 2. Frontend Setup

#### Install Dependencies

```bash
cd frontend
npm install
```

#### Environment Variables

Create a `.env` file in the `frontend` directory:

```env
# Database
DATABASE_URL="postgresql://user:password@localhost:5432/talking_avatar_ai"

# Node Environment
NODE_ENV="development"

# Modal (Python Microservices)
MODAL_KEY="your_modal_key"
MODAL_SECRET="your_modal_secret"

# AWS S3
AWS_ACCESS_KEY_ID="your_aws_access_key"
AWS_SECRET_ACCESS_KEY_ID="your_aws_secret_key"
AWS_REGION="us-east-1"
S3_BUCKET_NAME="your_bucket_name"

# Microservice Endpoints
TEXT_TO_SPEECH_ENDPOINT="https://your-modal-tts-endpoint.modal.run"
PHOTO_TO_VIDEO_ENDPOINT="https://your-modal-ptv-endpoint.modal.run"
FILE_TO_S3_ENDPOINT="https://your-modal-s3-endpoint.modal.run"

# Polar (Payment)
POLAR_ACCESS_TOKEN="your_polar_access_token"
POLAR_WEBHOOK_SECRET="your_polar_webhook_secret"

# FAL AI
FAL_KEY="your_fal_api_key"

# Credit Pack IDs (from Polar)
NEXT_PUBLIC_SMALL_CREDIT_PACK_ID="your_small_pack_id"
NEXT_PUBLIC_MEDIUM_CREDIT_PACK_ID="your_medium_pack_id"
NEXT_PUBLIC_LARGE_CREDIT_PACK_ID="your_large_pack_id"
```

#### Database Setup

```bash
# Generate Prisma Client
npm run db:generate

# Push schema to database
npm run db:push

# (Optional) Open Prisma Studio to view database
npm run db:studio
```

#### Start Development Server

```bash
npm run dev
```

The frontend will be available at `http://localhost:3000`

#### Start Inngest Dev Server (Separate Terminal)

```bash
npm run inngest
```

### 3. Backend Setup

#### Install Modal CLI

```bash
pip install modal
```

#### Authenticate with Modal

```bash
modal token new
```

#### Configure Modal Secrets

Create secrets in Modal dashboard for:
- `talking-avatar-ai-secret` - AWS credentials and S3 bucket info

#### Deploy Microservices

**Text-to-Speech Service:**
```bash
cd backend/text-to-speech
modal deploy tts.py
```

**Photo-to-Video Service:**
```bash
cd backend/photo-to-video
modal deploy ptv.py
```

**File-to-S3 Service:**
```bash
cd backend/file-to-s3
modal deploy file_to_s3.py
```

After deployment, Modal will provide URLs for each service. Add these URLs to your frontend `.env` file.

### 4. AWS S3 Setup

1. Create an S3 bucket in your AWS account
2. Configure CORS for the bucket:

```json
[
    {
        "AllowedHeaders": ["*"],
        "AllowedMethods": ["GET", "PUT", "POST", "DELETE"],
        "AllowedOrigins": ["*"],
        "ExposeHeaders": []
    }
]
```

3. Update the bucket name and credentials in your `.env` file

### 5. Database Management

Start the local PostgreSQL database:

```bash
cd frontend
./start-database.sh
```

## 📱 Usage

1. **Sign Up/Login**: Create an account or login using Better Auth
2. **Purchase Credits**: Buy credit packs through Polar integration
3. **Create Avatar**:
   - Upload a photo
   - Provide script text or upload audio
   - Choose voice settings
   - Generate talking avatar video
4. **Manage Creations**: View and download your generated videos

## 🔧 Development Scripts

### Frontend

```bash
npm run dev          # Start development server with Turbo
npm run build        # Build for production
npm run start        # Start production server
npm run lint         # Run ESLint
npm run lint:fix     # Fix linting issues
npm run typecheck    # Run TypeScript type checking
npm run format:check # Check code formatting
npm run format:write # Format code with Prettier
npm run inngest      # Start Inngest dev server
```

### Backend

```bash
# Deploy individual services
modal deploy <service>.py

# Run services locally for testing
modal run <service>.py
```

## 🏛️ Project Structure

```
talking-avatar-ai/
├── frontend/                 # Next.js frontend application
│   ├── src/
│   │   ├── app/             # App router pages
│   │   ├── components/      # React components
│   │   ├── actions/         # Server actions
│   │   ├── inngest/         # Background job functions
│   │   ├── lib/             # Utility libraries
│   │   └── server/          # Server-side code
│   └── prisma/              # Database schema
└── backend/                  # Python microservices
    ├── text-to-speech/      # TTS service
    ├── photo-to-video/      # Avatar generation
    └── file-to-s3/          # S3 upload service
```

## 🔐 Security

- Authentication handled by Better Auth
- Secure file uploads to S3 with presigned URLs
- Environment variables for sensitive data
- CORS configured for API security


## 👥 Contributors

- [Naman Rajani](https://github.com/Naman16rajani)

