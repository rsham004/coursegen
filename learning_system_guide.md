# AI Learning Course Generator - Implementation Guide

## Project Overview

Build a comprehensive system that transforms YouTube video transcripts into structured learning courses using Starlight (Astro), featuring AI-generated content, video snippets, voice narration, and interactive elements.

## Architecture Overview

```
┌─────────────────┐    ┌──────────────────┐    ┌─────────────────────┐
│ YouTube         │    │ Content          │    │ Starlight           │
│ Transcript      │───▶│ Processing       │───▶│ Learning Portal     │
│ Input           │    │ Pipeline         │    │ Output              │
└─────────────────┘    └──────────────────┘    └─────────────────────┘
                              │
                              ▼
                       ┌──────────────────┐
                       │ Media Generation │
                       │ - Videos         │
                       │ - Voice          │
                       │ - Quizzes        │
                       └──────────────────┘
```

## Tech Stack Requirements

### Core Framework
- **Frontend**: Starlight (Astro-based)
- **Backend**: Node.js with Express
- **Database**: SQLite (lightweight) or JSON files for MVP
- **Processing**: Batch-based with queue system

### AI Services Integration
- **Primary LLM**: Anthropic Claude 3.5 Sonnet
- **Alternative**: Google Gemini Pro (cost comparison)
- **Voice Synthesis**: ElevenLabs API (free tier: 10k chars/month)
- **Video Generation**: Stable Diffusion + FFmpeg or RunwayML API

### Required Dependencies
```json
{
  "dependencies": {
    "@astrojs/starlight": "^0.15.0",
    "astro": "^4.0.0",
    "express": "^4.18.0",
    "anthropic": "^0.20.0",
    "elevenlabs": "^0.5.0",
    "ffmpeg": "^0.0.4",
    "sharp": "^0.32.0",
    "jsdom": "^22.0.0",
    "markdown-it": "^13.0.0",
    "sqlite3": "^5.1.0",
    "bull": "^4.10.0"
  }
}
```

## Project Structure

```
learning-course-generator/
├── src/
│   ├── content/
│   │   └── courses/           # Generated course content
│   ├── components/
│   │   ├── Quiz.astro
│   │   ├── VideoPlayer.astro
│   │   └── ProgressTracker.astro
│   ├── processors/
│   │   ├── transcript-analyzer.js
│   │   ├── content-generator.js
│   │   ├── video-creator.js
│   │   └── voice-synthesizer.js
│   ├── utils/
│   │   ├── ai-client.js
│   │   ├── media-utils.js
│   │   └── quality-assessor.js
│   └── server/
│       ├── api/
│       ├── queue/
│       └── database/
├── public/
│   ├── generated-media/
│   │   ├── videos/
│   │   ├── audio/
│   │   └── images/
├── templates/
│   ├── lesson-template.md
│   ├── quiz-template.json
│   └── course-structure.json
├── config/
│   ├── starlight.config.mjs
│   └── processing.config.js
└── scripts/
    ├── generate-course.js
    └── batch-process.js
```

## Core Implementation Components

### 1. Transcript Analysis Pipeline

**File**: `src/processors/transcript-analyzer.js`

**Requirements**:
- Parse YouTube transcript formats (VTT, SRT, JSON)
- Identify natural break points for lessons
- Extract key concepts and learning objectives
- Assess content quality and educational value
- Generate lesson metadata

**Key Functions**:
```javascript
async function analyzeTranscript(transcriptText) {
  // Implementation requirements:
  // 1. Segment transcript into logical chunks (2-4 minutes each)
  // 2. Use Claude API to identify:
  //    - Main topics and subtopics
  //    - Learning objectives
  //    - Difficulty level
  //    - Prerequisites
  //    - Key terms and definitions
  // 3. Return structured lesson data
}

async function extractLearningObjectives(segment) {
  // Use Bloom's taxonomy classification
  // Return objectives in measurable format
}

async function assessContentQuality(segment) {
  // Score: coherence, depth, educational value
  // Flag segments needing enhancement
}
```

### 2. Content Generation Engine

**File**: `src/processors/content-generator.js`

**Requirements**:
- Transform transcript segments into structured lessons
- Generate markdown content following Starlight conventions
- Create supplementary explanations and examples
- Generate quiz questions and interactive elements
- Maintain consistent educational tone and structure

**Claude API Integration**:
```javascript
const LESSON_GENERATION_PROMPT = `
You are an expert instructional designer creating structured learning content.

Transform the following transcript segment into a comprehensive lesson:

TRANSCRIPT SEGMENT:
{transcriptSegment}

LESSON CONTEXT:
- Course Title: {courseTitle}
- Lesson Number: {lessonNumber}
- Previous Topics: {previousTopics}
- Target Audience: {targetAudience}

Generate content in this exact structure:

## Lesson Title
Brief engaging description (2-3 sentences)

### Learning Objectives
- Objective 1 (action verb + measurable outcome)
- Objective 2
- Objective 3

### Prerequisites
- Required knowledge/skills

### Core Content
Structured explanation with:
- Clear subheadings
- Code examples (if applicable)
- Practical examples
- Visual descriptions for video generation

### Key Takeaways
- 3-5 bullet points summarizing main concepts

### Practice Questions
3 multiple choice questions with explanations

REQUIREMENTS:
- Use active voice and engaging language
- Include practical examples
- Optimize for 5-7 minute reading time
- Provide clear transitions between concepts
- Include calls-to-action for engagement
`;
```

### 3. Video Generation Pipeline

**File**: `src/processors/video-creator.js`

**Requirements**:
- Generate visual slides from lesson content
- Create smooth transitions and animations
- Combine with AI-generated voice narration
- Export in web-optimized formats
- Handle batch processing efficiently

**Implementation Strategy**:
```javascript
async function generateLessonVideo(lessonContent, audioFile) {
  // 1. Create visual slides using Canvas API or Puppeteer
  // 2. Generate slide transitions
  // 3. Combine with voice narration
  // 4. Add captions for accessibility
  // 5. Export as MP4 with optimal compression
  
  const videoSpecs = {
    resolution: "1920x1080",
    framerate: 30,
    duration: "2-4 minutes",
    format: "MP4",
    codec: "H.264"
  };
}

async function createSlideDesign(content) {
  // Generate visually appealing slides
  // Use consistent branding and typography
  // Include diagrams and code syntax highlighting
}
```

### 4. Voice Synthesis Integration

**File**: `src/processors/voice-synthesizer.js`

**Requirements**:
- Convert lesson text to natural speech
- Optimize for educational delivery (pacing, emphasis)
- Handle technical terminology correctly
- Manage API rate limits and costs
- Generate multiple voice options

**ElevenLabs Integration**:
```javascript
async function generateVoiceNarration(lessonText, voiceSettings) {
  // 1. Preprocess text for optimal speech synthesis
  // 2. Add SSML markup for natural delivery
  // 3. Handle technical terms and pronunciations
  // 4. Generate audio with appropriate pacing
  // 5. Return high-quality audio file
  
  const ssmlText = addEducationalPacing(lessonText);
  const audioBuffer = await elevenLabsClient.textToSpeech(ssmlText);
  return processAudioForVideo(audioBuffer);
}

function addEducationalPacing(text) {
  // Add pauses after key concepts
  // Emphasize important terms
  // Adjust speed for code examples
}
```

### 5. Interactive Quiz System

**File**: `src/components/Quiz.astro`

**Requirements**:
- Generate contextual quiz questions
- Provide immediate feedback
- Track progress and performance
- Support multiple question types
- Integrate with Starlight's component system

**Quiz Component Structure**:
```astro
---
export interface Props {
  questions: QuizQuestion[];
  lessonId: string;
  passingScore: number;
}

interface QuizQuestion {
  id: string;
  type: 'multiple-choice' | 'true-false' | 'fill-blank';
  question: string;
  options?: string[];
  correct: string | number;
  explanation: string;
  difficulty: 'easy' | 'medium' | 'hard';
}
---

<div class="quiz-container">
  <!-- Interactive quiz implementation -->
  <!-- Progress tracking -->
  <!-- Results and feedback -->
</div>

<script>
  // Quiz logic and state management
  // Progress saving to localStorage
  // Analytics event tracking
</script>
```

### 6. Batch Processing System

**File**: `src/server/queue/processor.js`

**Requirements**:
- Handle multiple course generation requests
- Manage API rate limits across services
- Provide progress tracking and status updates
- Handle failures gracefully with retry logic
- Optimize for cost-effective processing

**Queue Implementation**:
```javascript
const Queue = require('bull');
const courseGenerationQueue = new Queue('course generation');

courseGenerationQueue.process(async (job) => {
  const { transcriptData, courseConfig } = job.data;
  
  try {
    // 1. Analyze transcript
    const analysis = await analyzeTranscript(transcriptData);
    job.progress(20);
    
    // 2. Generate lesson content
    const lessons = await generateLessons(analysis);
    job.progress(50);
    
    // 3. Create media assets
    const mediaAssets = await generateMedia(lessons);
    job.progress(80);
    
    // 4. Build Starlight course structure
    const courseOutput = await buildStarlightCourse(lessons, mediaAssets);
    job.progress(100);
    
    return courseOutput;
  } catch (error) {
    throw new Error(`Course generation failed: ${error.message}`);
  }
});
```

## Configuration Files

### Starlight Configuration

**File**: `astro.config.mjs`

```javascript
import { defineConfig } from 'astro/config';
import starlight from '@astrojs/starlight';

export default defineConfig({
  integrations: [
    starlight({
      title: 'AI Learning Portal',
      logo: {
        src: './src/assets/logo.svg',
      },
      social: {
        github: 'https://github.com/your-repo',
      },
      sidebar: [
        // Dynamic sidebar generation from courses
      ],
      customCss: [
        './src/styles/learning-portal.css',
      ],
      components: {
        // Override default components
        Quiz: './src/components/Quiz.astro',
        VideoPlayer: './src/components/VideoPlayer.astro',
      },
    }),
  ],
});
```

### Processing Configuration

**File**: `config/processing.config.js`

```javascript
module.exports = {
  ai: {
    provider: 'anthropic', // or 'gemini'
    model: 'claude-3-5-sonnet-20241022',
    maxTokens: 4000,
    temperature: 0.3,
  },
  video: {
    maxDuration: 300, // 5 minutes
    resolution: '1920x1080',
    framerate: 30,
    format: 'mp4',
  },
  voice: {
    provider: 'elevenlabs',
    voice: 'professional-educator',
    speed: 0.9,
    stability: 0.5,
  },
  processing: {
    batchSize: 5,
    maxConcurrent: 3,
    retryAttempts: 3,
    timeout: 300000, // 5 minutes
  },
  quality: {
    minTranscriptLength: 1000,
    maxLessonDuration: 300,
    minLessonsPerCourse: 3,
    maxLessonsPerCourse: 20,
  }
};
```

## API Integration Patterns

### Claude API Client

**File**: `src/utils/ai-client.js`

```javascript
const Anthropic = require('@anthropic-ai/sdk');

class AIClient {
  constructor() {
    this.anthropic = new Anthropic({
      apiKey: process.env.ANTHROPIC_API_KEY,
    });
  }

  async generateContent(prompt, options = {}) {
    try {
      const response = await this.anthropic.messages.create({
        model: 'claude-3-5-sonnet-20241022',
        max_tokens: options.maxTokens || 4000,
        temperature: options.temperature || 0.3,
        messages: [
          { role: 'user', content: prompt }
        ],
      });

      return this.parseResponse(response);
    } catch (error) {
      throw new Error(`AI generation failed: ${error.message}`);
    }
  }

  parseResponse(response) {
    // Extract and validate generated content
    // Handle different response formats
    // Return structured data
  }
}

module.exports = AIClient;
```

## Implementation Steps

### Phase 1: Core Pipeline (Week 1-2)

1. **Set up project structure**
   - Initialize Starlight project
   - Configure build system
   - Set up development environment

2. **Implement transcript analysis**
   - Create transcript parser
   - Integrate Claude API for content analysis
   - Build lesson segmentation logic

3. **Build content generation**
   - Create lesson templates
   - Implement content generation pipeline
   - Generate first sample course

### Phase 2: Media Generation (Week 3-4)

1. **Voice synthesis integration**
   - Set up ElevenLabs API
   - Create voice generation pipeline
   - Optimize for educational delivery

2. **Video creation system**
   - Build slide generation
   - Implement animation system
   - Create video compilation pipeline

3. **Quiz system implementation**
   - Create quiz generation logic
   - Build interactive components
   - Integrate with Starlight

### Phase 3: Optimization & Scaling (Week 5-6)

1. **Batch processing system**
   - Implement job queue
   - Add progress tracking
   - Create monitoring dashboard

2. **Performance optimization**
   - Cache generated content
   - Optimize media processing
   - Implement error handling

3. **Quality assurance**
   - Add content validation
   - Implement quality scoring
   - Create review system

## Error Handling & Quality Assurance

### Content Validation
```javascript
function validateGeneratedContent(content) {
  const checks = [
    'hasLearningObjectives',
    'hasAdequateLength',
    'hasStructuredFormat',
    'hasEngagingLanguage',
    'hasActionableContent'
  ];
  
  return checks.every(check => contentChecks[check](content));
}
```

### Quality Metrics
- Content coherence score (0-100)
- Educational value assessment
- Engagement level measurement
- Technical accuracy validation
- Accessibility compliance check

### Performance Monitoring
- API response times
- Generation success rates
- Media processing efficiency
- User engagement metrics
- Cost per course generated

## Cost Optimization Strategies

### API Usage Management
- Implement request batching
- Cache frequently used results
- Use tiered processing (simple → complex)
- Monitor and alert on usage limits

### Resource Efficiency
- Optimize media compression
- Use CDN for asset delivery
- Implement lazy loading
- Cache generated courses

### Budget Tracking
```javascript
const costTracker = {
  anthropic: { requests: 0, tokens: 0, cost: 0 },
  elevenlabs: { characters: 0, cost: 0 },
  storage: { gb: 0, cost: 0 },
  total: 0
};
```

## Success Metrics

### Technical KPIs
- Course generation time: < 30 minutes per course
- Content quality score: > 85/100
- Video generation success rate: > 95%
- API cost per course: < $5

### Educational KPIs
- Lesson completion rate: > 80%
- Quiz pass rate: > 70%
- User engagement time: > 10 minutes per lesson
- Content retention rate: > 60%

## Deployment & Maintenance

### Deployment Strategy
- Use Vercel/Netlify for Starlight frontend
- Deploy Node.js backend on Railway/Render
- Use GitHub Actions for CI/CD
- Implement staging environment for testing

### Monitoring & Maintenance
- Set up error tracking (Sentry)
- Monitor API usage and costs
- Regular content quality audits
- User feedback collection and analysis

---

**Next Steps**: Start with Phase 1 implementation, focusing on the transcript analysis and content generation pipeline. Test with a single YouTube video before scaling to multiple courses.