# Clara Answers: Zero-Cost Automation Pipeline

## Overview
This pipeline converts unstructured conversational data (demo and onboarding calls) into version-controlled, production-ready Retell AI agent configurations. It simulates the real-world operational challenge of moving from exploratory client conversations to rigid, safe AI voice agent deployments.

## Architecture & Tech Stack ($0 Cost)
To meet the hard constraint of zero cost while handling large context windows, the stack is optimized as follows:
* **Orchestrator:** **n8n** (Self-hosted locally via Docker). Selected for its native file-system capabilities, allowing direct reading/writing of local JSON files to simulate database transactions.
* **LLM Engine:** **Google Gemini 1.5 Flash (Free Tier API)**. Transcripts are long, and Gemini’s free tier provides a 1M token context window with excellent structured JSON output capabilities at $0 cost.
* **Storage:** **Local File System**. Simulating a structured document database.

## Data Flow
1. **Pipeline A (Demo -> v1):** - Ingests demo transcript.
   - LLM extracts `memo.json` via a strict JSON schema. It is instructed to leave missing data as `null` and populate a `questions_or_unknowns` array to prevent hallucinations.
   - LLM generates a preliminary `agent_spec.json`.
2. **Pipeline B (Onboarding -> v2):** - Ingests new onboarding transcript AND the existing `v1/memo.json`.
   - LLM acts as a diffing engine, resolving unknowns, applying new constraints, and outputting a complete `v2/memo.json` alongside a clean `changelog.md`.
   - Generates the final, production-ready `v2/agent_spec.json`.

## Setup Instructions
1. Clone this repository.
2. Ensure Docker Desktop is running on your machine.
3. Run `docker-compose up -d` in the root directory to spin up the n8n instance.
4. Access the n8n UI at `http://localhost:5678`.
5. Create a free API key at [Google AI Studio](https://aistudio.google.com/) and add it to your n8n environment or HTTP Request nodes.
6. Import the workflows from the `/workflows` directory into n8n.

## Running the Dataset
* Place your transcript `.txt` files in a local directory accessible to the n8n Docker volume (`/outputs`).
* Trigger the pipelines manually via the n8n UI. 
* All outputs automatically generate in `/outputs/accounts/{account_id}/...`

## Known Limitations & Production Improvements
* **Audio Processing:** This pipeline assumes text transcripts are provided or generated via a free local Whisper instance. Native audio transcription inside n8n requires heavier compute or paid APIs.
* **Storage:** For production, I would migrate storage from local JSON files to a managed PostgreSQL database (like Supabase) to query account states dynamically.
* **Triggers:** I would replace manual n8n webhook triggers with automated listener webhooks tied to CRM (HubSpot/Salesforce) deal stage changes (e.g., triggering Pipeline B when a deal moves to "Closed Won").