# ConceptEase AI - Design Document

## 1. System Architecture
ConceptEase AI follows a standard **Client-Server Architecture**.

- **Client (Frontend)**: A modern, responsive single-page-like interface built with Vanilla HTML, CSS, and JS.
- **Server (Backend)**: A Flask-based REST API that communicates with external AI services.
- **External Services**: 
    - **Groq**: Provides low-latency inference for text (Llama-3.3-70b) and vision (Llama-4-Scout).
    - **Pollinations AI**: Generates images based on AI-refined prompts.

## 2. Frontend Design

### 2.1 UI/UX Philosophy
- **Glassmorphism**: Using semi-transparent backgrounds with backdrop filters for a premium 'frosted glass' look.
- **Dark Mode First**: A sleek dark theme with vibrant accent colors (Primary: Blue-Violet).
- **Tabbed Navigation**: Instant switching between features without page reloads.
- **Micro-animations**: Hover effects on cards, pulsing voice indicators, and smooth transitions for loaders.

### 2.2 Key Components
- **Navigation Bar**: Sidebar or top bar for switching between learning modules.
- **Input Area**: Unified input field with voice search integration and image upload capabilities.
- **Content Displays**: 
    - Markdown-rendered containers for explanations and roadmaps.
    - Interactive Quiz cards with state management for answers.
    - Visual gallery for generated infographics.

## 3. Backend Design

### 3.1 API Endpoints
| Endpoint | Method | Description |
| :--- | :--- | :--- |
| `/health` | GET | Check if the backend is running. |
| `/api/explain` | POST | Generates multi-level explanations. |
| `/api/quiz` | POST | Generates a JSON array of MCQs. |
| `/api/roadmap` | POST | Generates a structured learning path. |
| `/api/summarize` | POST | Generates a concise summary. |
| `/api/document` | POST | Processes code snippets for docs/explanation. |
| `/api/generate-image` | POST | Creates a prompt and returns an image URL. |
| `/api/analyze-image` | POST | Uses Vision-LLM to identify concepts from images. |

### 3.2 AI Implementation Logic
- **Prompt Engineering**: Each endpoint uses carefully crafted system prompts to ensure the AI responds in the correct format (Markdown or JSON).
- **Format Sanitization**: The backend includes logic to strip Markdown code blocks (e.g., ` ```json `) from responses intended to be raw JSON for the frontend.
- **Vision Workflow**: Base64 encoded images are sent to the Vision LLM to extract a "primary concept" string, which is then used to populate the search inputs.

## 4. Data Flow
1. **User Input**: User enters a topic via text, voice, or image upload.
2. **Frontend Request**: JS captures the input and sends a JSON payload to the corresponding Flask route.
3. **AI Processing**: Flask calls the Groq API with a specific prompt.
4. **Response Delivery**: AI returns content (Markdown/JSON) which is forwarded back to the frontend.
5. **Rendering**: `marked.js` converts Markdown to HTML, and custom JS logic renders interactive elements (Quizzes).

## 5. Security & Stability
- **API Key Management**: Keys are stored in a `.env` file and never exposed to the frontend.
- **CORS Configuration**: Flask-CORS is used to allow the frontend to communicate with the local server during development.
- **Error Handling**: Graceful degradation with toast notifications when the backend or AI service is unavailable.

## 6. External Integrations
- **Groq Cloud**: Selected for its exceptional speed, enabling real-time feeling interactions.
- **Pollinations AI**: Used for royalty-free, keyless image generation based on dynamic prompts.
- **Google Fonts**: "Outfit" used for a modern, geometric aesthetic.
