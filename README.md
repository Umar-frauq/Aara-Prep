# AARA Prep - AI Therapist Platform

## Project Status

AARA Prep is an open-source project under active development. The repository is organized to make the project easier to understand, run locally, and contribute to.

- [Contributing guide](CONTRIBUTING.md)
- [Roadmap](ROADMAP.md)
- [Security policy](SECURITY.md)
- [Code of conduct](CODE_OF_CONDUCT.md)
- [Changelog](CHANGELOG.md)

> AARA Prep is not a therapist, diagnosis tool, medical-advice service, or crisis-care service. It is intended to support—not replace—qualified professional care.

AARA is a pre-therapy and therapy companion that turns daily experiences into clear, shareable insights — so therapists understand users faster, and users feel understood. That’s it. No extra claims. No therapy replacement.

## 🚫 What AARA is NOT

Let’s be strict here :

- ❌ **Not a therapist**
- ❌ **Not a diagnosis tool**
- ❌ **Not medical advice**
- ❌ **Not crisis care**

AARA never replaces human therapy. It supports it.

## 🧠 Core Philosophy

**Nothing is connected by default. Everything is connected by consent.**

- Journals are private unless user opts in
- Chat is summarized, not exposed
- Reports are snapshots, not surveillance

## 🚀 Features

### User Features
- 🤖 **AI Therapy Chat** - Powered by OpenAI GPT-4 with voice input/output (Whisper + ElevenLabs)
- 📝 **Daily Check-Ins** - Progressive emotional tracking with 24h frequency validation
- 📖 **Private Journaling** - Voice and text entries with granular consent controls
- 📊 **Insights Dashboard** - Real-time mood trends, themes, and patterns (7/30/90 day views)
- 📄 **Shareable Reports** - Generate pre-therapy, therapy, or self-insight reports
- 🎮 **Mental Wellness Games** - 5 interactive games for focus, calm, and mindfulness
- 👨‍⚕️ **Therapist Booking** - Secure session booking with Stripe integration

### Therapist Features
- 📨 **Secure Report Sharing** - PDF download or private link with revocation
- 👁️ **Read-Only Access** - View patient insights without data collection
- 🔐 **Privacy-First** - All sharing requires explicit patient consent

### Privacy & Security
- ✅ **Consent-Based Processing** - No data used without explicit opt-in
- 🔒 **Immutable Reports** - Reports locked after creation, journals processed once
- 📜 **Full Audit Trail** - All consent actions logged with timestamps
- 🗑️ **GDPR Compliant** - Right to deletion and data export
- 🔐 **Firebase Auth** - Google and Email authentication

## 🏗️ Backend Architecture (v1.0)

### Data Flow
```
User → Check-In/Journal → Consent → Insight Processing → Report Generation → Sharing
```

### Core Services

| Service | Purpose | Key Features |
|---------|---------|--------------|
| **User State** | Manages therapy journey stages | 4 states (exploration→preparing→in_therapy→maintenance) |
| **Check-In** | Daily emotional tracking | 24h frequency rule, progressive depth |
| **Consent** | Privacy-first data control | Granular opt-in, full audit trail |
| **Insights** | Pattern detection | Theme extraction, emotional patterns, recurrence signals |
| **Reports** | Clinical summaries | 3 types (pre-therapy, therapy, self-insight), immutable |
| **Sharing** | Therapist access | Secure tokens, PDF generation (Puppeteer), revocation |

### API Endpoints

**User State:**
- `GET /api/user/state` - Get current state and suggestions
- `PATCH /api/user/state` - Update state (requires confirmation)

**Check-Ins:**
- `POST /api/check-in` - Submit daily check-in
- `GET /api/check-in/latest` - Get latest check-in + `canCheckIn` status
- `GET /api/check-in/level` - Get progressive question level

**Insights:**
- `GET /api/insights/current?days={7|30|90}` - Real-time insights

**Reports:**
- `POST /api/reports` - Generate new report
- `GET /api/reports` - List user reports
- `GET /api/reports/:id` - Get specific report

**Sharing:**
- `POST /api/reports/:id/share` - Create share (PDF or secure link)
- `DELETE /api/reports/:id/share/:shareId` - Revoke share
- `GET /api/share/:token` - Public therapist access (validates token)

### Data Models

See [`/lib/models/backend.ts`](file:///d:/aara%20website/Aara%20app/lib/models/backend.ts) for complete TypeScript interfaces.

### Privacy Guarantees

1. **No Processing Without Consent** - Journals are private by default
2. **One-Time Processing** - Journals marked `processed` after first report inclusion
3. **Report Immutability** - Reports locked (`locked: true`) immediately after creation
4. **State Transitions** - All state changes require user confirmation
5. **Share Revocation** - Users can revoke therapist access anytime

---

## 🛠️ Tech Stack

- **Frontend**: Next.js 14, React 18, TypeScript
- **Styling**: Tailwind CSS, Framer Motion
- **Backend**: Firebase (Auth, Firestore, Realtime DB)
- **AI**: OpenAI GPT-4, Whisper (speech-to-text), ElevenLabs (text-to-speech)
- **Payments**: Stripe
- **Analytics**: Mixpanel (optional)
- **Deployment**: Vercel

## 📦 Installation

### Prerequisites

- Node.js 18+ and npm
- Firebase project
- OpenAI API key
- Stripe account (optional, for payments)
- ElevenLabs API key (optional, for voice features)

### Setup

1. **Clone the repository:**
```bash
git clone <repository-url>
cd aara-therapist
```

2. **Install dependencies:**
```bash
npm install
```

3. **Create `.env.local` file:**
```bash
cp .env.example .env.local
```

4. **Fill in environment variables:**
```env
# Firebase Configuration
NEXT_PUBLIC_FIREBASE_API_KEY=your_firebase_api_key
NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN=your_project_id.firebaseapp.com
NEXT_PUBLIC_FIREBASE_PROJECT_ID=your_project_id
NEXT_PUBLIC_FIREBASE_STORAGE_BUCKET=your_project_id.appspot.com
NEXT_PUBLIC_FIREBASE_MESSAGING_SENDER_ID=your_messaging_sender_id
NEXT_PUBLIC_FIREBASE_APP_ID=your_app_id
NEXT_PUBLIC_FIREBASE_MEASUREMENT_ID=your_measurement_id
NEXT_PUBLIC_FIREBASE_DATABASE_URL=https://your_project_id-default-rtdb.firebaseio.com

# OpenAI Configuration
OPENAI_API_KEY=your_openai_api_key

# Stripe Configuration (Optional)
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY=your_stripe_publishable_key
STRIPE_SECRET_KEY=your_stripe_secret_key
STRIPE_WEBHOOK_SECRET=your_stripe_webhook_secret

# ElevenLabs API (Optional)
ELEVENLABS_API_KEY=your_elevenlabs_api_key

# Mixpanel Analytics (Optional)
NEXT_PUBLIC_MIXPANEL_TOKEN=your_mixpanel_token

# Site URL (for sitemap)
SITE_URL=https://your-domain.com
```

5. **Run development server:**
```bash
npm run dev
```

6. **Open browser:**
Navigate to `http://localhost:3000`

## 🔥 Firebase Setup

1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Create a new project
3. Enable Authentication:
   - Go to **Authentication > Sign-in method**
   - Enable **Email/Password** and **Google**
4. Create Firestore Database:
   - Go to **Firestore Database**
   - Create database in production mode
   - Start in test mode (for development)
5. Enable Realtime Database (optional, for real-time chat):
   - Go to **Realtime Database**
   - Create database
6. Copy your Firebase config to `.env.local`

## 📱 PWA Icons

Add PWA icons to `public/`:
- `icon-192.png` (192x192px)
- `icon-512.png` (512x512px)

## 🏗️ Build for Production

```bash
npm run build
npm start
```

## 🚢 Deploy to Vercel

1. Push your code to GitHub
2. Import project in [Vercel](https://vercel.com)
3. Add all environment variables in Vercel dashboard
4. Deploy!

The project includes:
- `vercel.json` configuration
- Automatic sitemap generation
- SEO optimization
- PWA support

## 📁 Project Structure

```
├── app/                    # Next.js app directory
│   ├── api/               # API routes (auth-protected)
│   ├── auth/              # Authentication pages
│   ├── chat/              # Chat page with AI
│   ├── games/             # Games page
│   ├── therapists/        # Therapists page
│   ├── journal/           # Journal page
│   ├── mode/              # Analytics/Mode page
│   ├── profile/           # Profile page
│   ├── privacy/           # Privacy policy
│   └── terms/             # Terms of service
├── components/            # React components
│   ├── ui/               # Base UI components
│   ├── layout/           # Layout components
│   ├── home/             # Home page components
│   ├── games/            # Game components (lazy-loaded)
│   └── therapists/       # Therapist components
├── lib/                  # Utility libraries
│   ├── firebase/         # Firebase config and helpers
│   ├── ai/              # AI integration (OpenAI, ElevenLabs)
│   ├── stripe/          # Stripe integration
│   ├── auth/            # Auth verification
│   └── analytics.ts     # Analytics helpers
└── hooks/               # Custom React hooks
```

## 🔐 Security Features

- Server-side auth verification (`verifyAuth()`)
- Protected API routes
- Data deletion functionality
- Consent toggles for therapist sharing
- Crisis disclaimers

## 🎨 Design System

- **Theme**: Dark glassmorphic with neon accents
- **Colors**: 
  - Primary: Neon Blue (#00AEEF)
  - Secondary: Neon Purple (#7A5FFF)
  - Background: Dark gradient (#0B0C10 → #1C1E24)
- **Components**: Glass cards with backdrop blur
- **Animations**: Framer Motion for smooth transitions

## 🚀 Performance

- Lazy-loaded game components
- Dynamic imports for heavy assets
- Optimized images with Next.js Image
- API response caching
- Lighthouse scores: Perf 85+, A11y 90+, PWA 90+

## 📝 Environment Variables

See `.env.example` for all required environment variables.

## 🐛 Troubleshooting

- **Build errors**: Ensure all environment variables are set
- **Firebase errors**: Verify Firebase config in `.env.local`
- **OpenAI errors**: Check API key validity and credits
- **Stripe errors**: Verify Stripe keys are correct
- **Missing dev script**: Run `npm install` to ensure all dependencies are installed

## 📄 License

MIT

## 💬 Support

For issues and questions, please open an issue on GitHub.

---

**Built with ❤️ for mental wellness**
