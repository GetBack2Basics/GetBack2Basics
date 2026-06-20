# JobCrafter AI — Workspace & STAR Interview Copilot

[Copy of Readme from private repo for information purposes]

JobCrafter AI is a premium, full-suite professional campaign workspace designed to help candidates prepare for senior roles and technical interviews. It allows you to analyze target job descriptions, generate highly tailored cover letters, draft capability task presentation slides or operational spreadsheets, and practice answering behavioral and technical questions in an interactive STAR interview simulator.

The application leverages the high-performance **Gemini 3.5 Flash** model with the modern `@google/genai` SDK to produce highly accurate, domain-specific insights while upholding strict administrative standards.

---

## Key Capabilities & Features

### 1. Document & Campaign Generator
*   **Targeted Job Extraction**: Uses Gemini to analyze complex job postings (or URLs) and extract key requirements, company context, and employer details.
*   **Tailored Cover Letter Suite**: Pairs the candidate's professional profile against job post criteria to generate clean, formatted, and optimized cover letters with centered header structures.
*   **Iterative Paragraph Refinement**: Highlight any text snippet or paragraph in the letter to prompt custom, targeted AI rewrites or case-study swaps.
*   **"Remove AI Voice" Humanizer**: Applies career coaching strategies to strip away generic corporate fillers or "smooth" transitions, making the letter sound like an authentic experienced professional.
*   **Associated Submission Email**: Automatically drafts short, highly direct submission emails corresponding to the tailored letters.

### 2. Tailored CV / Resume Builder & ATS Audit
*   **ATS Compatibility Auditing**: Features a real-time, interactive compatibility score gauge that maps resume alignment to job description requirements (Excellent, Good, or Needs Optimization).
*   **Keyword Match Scanner**: Automatically scans for crucial job description keywords against the candidate's CV/Resume text, displaying side-by-side matches (green checks / red alerts) to highlight gaps.
*   **Strategic Recommendations Checklist**: Provides structured suggestions categorizing alignment gaps, along with detailed feedback and actionable recommendations.
*   **Interactive Resume Refinement**: Supports targeted refining using "Smart Match Check", "Refine & Incorporate Specifics", and "Remove AI Voice" humanizer to strip out AI clichés and polish the resume tone.

### 3. Capability Task workspace
*   **Slide Deck Outline Drafter**: Converts complex task instructions into an executive briefing deck. Follows MIT Sloan presenting standards (distinct non-repetitive slides, crisp under-ten-word outlines, and thorough point-form presenter notes). Supports high-quality visual backdrop prompts optimized for Slides AI tools.
*   **Work Sheet Matrix Generator**: Generates complete spreadsheet grids and tables (e.g., budget shift calculations, NTv2 coordinate transformation sheets, operational compliance) populated with realistic technical data points and professional summary metrics.
*   **Responsible AI Use Disclosures**: Programmatically appends compliant AI Use statements to every generated slide-deck, report, or spreadsheet to maintain alignment with transparency directives.

### 4. STAR Interview prep & Mock Live Arena
*   **Pre-generated Scenario Checklists**: Automatically builds targeted mock interview questions (Behavioral and Technical) based on the candidate's profile, custom panel questions, and the generated portfolio assets.
*   **Voice-Enabled Practice Arena**: Record your answer in real time using your microphone, or type your response directly into the workspace.
*   **Dynamic Response Evaluation**: Analyzes your transcribed practice response and returns a structured scorecard, custom numerical rating (1-10), constructive critique, and a polished revised answer combining your notes with strategic STAR frameworks.
*   **Simulated Hiring Panel**: Outlines three realistic professional panelists tailored to the target organization. Outlines their background, strategic communication tactics, and candidate-led questions to ask.
*   **Elevator Pitch Blueprint**: Outlines the five essential landmarks for a compelling introductory speech (Passion, Community, Experience, Current, Value).

---

## Getting Started

You can run the application either using **Docker Compose (Recommended for isolated environments)** or **natively with Node.js**.

### Prerequisites
* [Docker & Docker Compose](https://docs.docker.com/get-docker/) (if running containerized)
* [Node.js](https://nodejs.org/) v18+ and npm (if running natively)
* An active Google Gemini API Key

---

### Option A: Running with Docker Compose (Recommended)

This project is fully containerized and orchestrates multiple environments. The environment variables are loaded from the `.env` file.

1. **Configure Environment Variables**
   ```bash
   cp .env.example .env
   ```
   Open the `.env` file and insert your `GEMINI_API_KEY`. If setting up UAT, configure the `TUNNEL_TOKEN` as well.

2. **Start the Development Container (`app-dev`)**
   Spins up the dev server on port `3000` with volume hot-reloading (HMR):
   ```bash
   sudo docker compose up -d app-dev
   ```
   Open **`http://localhost:3000`** in your browser.

3. **Start the UAT / Review Environment**
   Spins up the production-built bundle served by Nginx on port `8080` along with the Cloudflare Tunnel client:
   ```bash
   sudo docker compose up -d app-uat cloudflared-uat
   ```
   Open **`http://localhost:8080`** for local UAT testing, or access via your configured Cloudflare Tunnel domain (e.g., `https://jobhuntcrafter.getback2basics.net`).

4. **Shutdown Containers**
   ```bash
   sudo docker compose down
   ```

---

### Option B: Running Natively

1. **Install Dependencies**
   ```bash
   npm install
   ```

2. **Configure Environment Variables**
   ```bash
   cp .env.example .env
   ```
   Edit `.env` and fill in `GEMINI_API_KEY`.

3. **Launch the Dev Server**
   ```bash
   npm run dev
   ```
   Open **`http://localhost:3000`** in your browser.

---

## Environment Pipeline & Deployment Guides

Detailed documentation regarding routing, branching pipelines, and staging can be found in the `docs/` folder:

*   **[docs/cloudflare_tunnels.md](file:///home/ubuntu/JobHunt_Crafter_AI/docs/cloudflare_tunnels.md)**: Steps to create and update Cloudflare Tunnels and how to configure internal Docker routing rules (`http://app-uat`).
*   **[docs/pipeline_guide.md](file:///home/ubuntu/JobHunt_Crafter_AI/docs/pipeline_guide.md)**: Guide on the Git branching model (`dev` ➔ `uat` ➔ `main`) and how we ensure development commits do not break the public-facing UAT staging container.

---

## Importing Back Into Google AI Studio

If you want to continue editing, deploying, or sharing this application within the **Google AI Studio** environment:

1.  **Create a ZIP Archive or Connect to GitHub**
    *   Compress the project directory into a standard `.zip` archive (be sure to exclude `node_modules/` and `dist/`), or push the updated codebase to a public/private GitHub repository.
2.  **Upload to AI Studio Build**
    *   Go to [AI Studio Build](https://ai.studio/build).
    *   Use the **Import Code** or **Sync GitHub Repo** action to load your file directory.
3.  **Platform Environment Variables**
    *   AI Studio automatically provisions the secure hosting container.
    *   You do not need to hardcode your `GEMINI_API_KEY` into any files. Simply navigate to the **Secrets** or **Settings** panel within the AI Studio interface and input your `GEMINI_API_KEY`. The platform will automatically inject this key to `process.env.GEMINI_API_KEY` when building your preview frame.

---

## Technical Architecture Notes

*   **UI Core**: Built with **React 19** and **TypeScript** configured over **Vite**.
*   **Styling**: Styled using utility-first classes from **Tailwind CSS**.
*   **Animations**: Liquid fluid layouts and transitions powered by the **Motion** library (`motion/react`).
*   **API Client**: Leverages the official modern `@google/genai` package for direct server-less proxy variables injected during the Vite build pipeline (`vite.config.ts`).
*   **Containerization**: Fully modular Docker setup utilizing multi-stage builds (Dev/Builder/Nginx-UAT).

---

## Mandatory completion check after every code change

Before declaring any code task complete, always run this operational sequence:

1. Stop any conflicting host-level processes or containers on ports 3000/8080.
2. Start the dev container:
   - `sudo docker compose up -d app-dev`
3. Verify reachability:
   - `curl -I http://127.0.0.1:3000/`
4. Verify container status and port listener:
   - `sudo docker compose ps`
   - `ss -tln | grep 3000`

Do not mark work complete unless the service is reachable and returning HTTP 200.
