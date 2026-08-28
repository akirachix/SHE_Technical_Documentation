# 4. AI & Game Generation

---

## 4.1 AI Architecture
The AI workflow is designed around grounded generation: the model is instructed to use approved source material rather than relying on general knowledge.

```text
 [ Admin Prompt ] 
        │
        ▼
 [ FastAPI Backend ] ──► [ DB Source Document Lookup ]
        │
        ▼
 [ Gemini File Search Vector ]
        │
        ▼
 [ Gemini Model Inference ]
        │
        ▼
 [ Structured JSON Return ] ──► [ Admin Validation / Review Queue ]
```

---

## 4.2 Model
The platform interfaces directly with the Google GenAI SDK (`google-genai`). The system is locked to the stable version of `gemini-3.5-flash` using the explicit standard global API layout channel (`api_version="v1"`). 

This model selection balances:
* Structural JSON adherence
* Data extraction accuracy from dense files
* Rapid text-generation latency loops

---

## 4.3 Grounding / File Search

### 4.3.1 Seeded Registry & Metadata Mapping
The backend uses a specialized database model (`GameFile`) to store the metadata of uploaded Markdown documents. This model acts as a translation layer between administrative selections and the Gemini File Search store names. A foundational collection of verified digital-safety texts has been seeded directly into the PostgreSQL storage tier.

### 4.3.2 Ingestion & Dynamic File Operations
* **The Ingestion Endpoint**: Admins use a dedicated API endpoint to upload and register custom Markdown assets. During an upload, administrators manually supply a `parent_theme` and a specific `topic`. This metadata is stored alongside the document’s system parameters.
* **Indexing Pipeline**: Once captured by FastAPI, the file is pushed to Google's vector space and index boundaries. The generated unique remote identifier is stored in the database under the `gemini_file_name` property.
* **Fallback & Error Boundaries**: If an administrator attempts a payload generation cycle on a theme or topic combination that does not exist in the relational database metadata, the system executes an immediate short-circuit routine, returning a clean structural error code:
  ```json
  {"error": "Selected theme and topic configuration not found in database metadata"}
  ```

---

## 4.4 Prompt Architecture
The generation prompts are fully hardcoded on the backend within the services tier to prevent user tampering or injection vulnerabilities. The pipeline uses an automated execution decorator (`@_retry_gemini`) to build resilience against sporadic network timeouts or API rate boundaries.

### 4.4.1 Core Generation Orchestrator

**Source File:** [`backend/services/game_files.py`](https://github.com/akirachix/SHE_Backend/blob/main/dadasafe/services/game_files.py)

???+ code "View Full Code Implementation (Click to collapse)"
    ```python
    @_retry_gemini
    def generate_game_payload(prompt: str, parent_theme: str, topic: str) -> str:
        settings = get_settings()
        client = genai.Client(api_key=settings.gemini_api_key, http_options={"api_version": "v1"})

        db = SessionLocal()
        try:
            file_record = db.query(GameFile).filter(
                GameFile.parent_theme == parent_theme,
                GameFile.topic == topic
            ).first()

            if not file_record:
                return '{"error": "Selected theme and topic configuration not found in database metadata"}'

            context_instruction = (
                "You generate a comprehensive, multi-activity educational game payload for a young women's digital safety education platform.\n"
                f"The targeting focus has been strictly bounded to the theme '{parent_theme}' and topic '{topic}'.\n"
                "You must base the generated layout payload ONLY on the provided file search documents — never use outside knowledge.\n"
                "If the requested topic is not covered in the documents, respond exactly with: "
                '{"error": "not found in the provided materials"}.\n'
                "Otherwise, you must assemble and output a single, flat JSON object containing a topic overview and all three game types combined natively into this exact structural layout schema:\n\n"
                
                "{\n"
                '   "topic_description": "CRITICAL INSTRUCTION: Write a friendly, deeply detailed, and supportive educational introduction (about 2-3 paragraphs long) designed to teach a young female audience about this safety topic before they play. '
                '    Avoid dense technical jargon (or explain it in simple, everyday terms if unavoidable). '
                '    It must clearly explain: 1) What this digital safety concept means in plain English, 2) Why it matters to their personal safety, and 3) The core, practical strategies or warning signs they should know.",\n'
                
                '  "quiz": [\n'
                "    Generate exactly 5 distinct multiple-choice questions matching these inner element keys:\n"
                '    { "question": "Question text", "choices": ["choice1", "choice2", "choice3", "choice4"], "answer": "The verbatim correct choice string" }\n'
                "  ],\n"
                
                '  "puzzle": {\n'
                '    "wordsearch": { "grid_size": 12, "words": ["5", "safety", "words", "hidden", "here"] },\n'
                '    "scrambles": [\n'
                "      Generate exactly 5 inner elements matching these keys:\n"
                '      { "scrambled": "Jumbled string", "unscrambled": "Correct word string" }\n'
                "    ],\n"
                '    "crossword": [\n'
                "      Generate exactly 5 straightforward crossword clues based entirely on the safety concepts in your documents:\n"
                '      { "clue": "An educational safety question or clue statement text.", "answer": "UPPERCASEANSWERWORD" }\n'
                "    ]\n"
                "  },\n"
                
                '  "scenario": {\n'
                '    "scenario_text": "A short, narrative safety dilemma text highlighting choices and consequences for users.",\n'
                '    "options": [\n'
                "      Generate exactly 2 action pathways matching these keys:\n"
                '      { "choice": "Action taken description string", "consequence": "Outcome and educational feedback summary string" }\n'
                "    ]\n"
                "  }\n"
                "}\n\n"
                
                "CRITICAL FORMATTING BOUNDARY:\n"
                "Respond with ONLY the single, valid unified root JSON object containing all required fields. "
                "Never include markdown block backticks (like ```json), outer text, conversational greetings, summary descriptions, or citations. Output pure clean code structure."
            )

            response = client.models.generate_content(
                model="gemini-3.5-flash",
                contents=prompt,
                config=types.GenerateContentConfig(
                    system_instruction=context_instruction,
                    response_mime_type="application/json",
                    tools=[types.Tool(
                        file_search=types.FileSearch(file_search_store_names=[file_record.gemini_file_name])
                    )]
                )
            )
            return response.text
        finally:
            db.close()
    ```

The system instruction maps out explicit data boundaries: forcing a flat JSON string, commanding specific lengths for `topic_description`, setting a 12x12 matrix boundary for `wordsearch`, and requesting exactly five quizzes, five scrambles, five crosswords, and two scenario choices. It enforces strict compliance by passing `response_mime_type="application/json"` to the generation config.

### 4.4.2 Contextual Ancillary Operations
The system also exposes an isolated query framework (`ask_game_ai`) utilizing a static file search system parameters template. This allows support provider agents or general system calls to verify facts against the foundational index without triggering game configuration code paths.

**Source File:** [`backend/services/game_files.py`](https://github.com/akirachix/SHE_Backend/blob/main/dadasafe/services/game_files.py)

??? code "View Full Code Implementation (Click to expand)"
    ```python
    @_retry_gemini
    def ask_game_ai(user_question: str) -> str:
        settings = get_settings()
        client = genai.Client(api_key=settings.gemini_api_key, http_options={"api_version": "v1"})

        try:
            response = client.models.generate_content(
                model="gemini-3.5-flash",
                contents=user_question,
                config=types.GenerateContentConfig(
                    system_instruction=(
                        "You are an intelligent assistant. Answer ONLY using the provided file search documents. "
                        "Do not use any outside knowledge, even if you know the answer. "
                        "If the answer is not found in the documents, reply exactly with: "
                        "'not found in the provided materials'."
                    ),
                    tools=[types.Tool(
                        file_search=types.FileSearch(file_search_store_names=[FILE_SEARCH_STORE_NAME])
                    )]
                )
            )
            return response.text
        except Exception as e:
            return f"Error executing context query: {str(e)}"
    ```


---

## 4.5 Game Payload Schema
Before payload objects are processed or surfaced to client requests, data serialization and structural rules are strictly validated on the FastAPI layer using Pydantic models.

**Source File:** [`backend/services/game_files.py`](https://github.com/akirachix/SHE_Backend/blob/main/dadasafe/schemas/game_files.py)

??? code "View Full Code Implementation (Click to expand)"
    ```python
    import uuid
    from datetime import datetime
    from pydantic import BaseModel, Field
    from typing import Optional, List

    class GameFileCreate(BaseModel):
        file_name: str = Field(..., description="The exact matching name of the local markdown file.")

    class GameFileRead(BaseModel):
        file_id: uuid.UUID
        user_id: uuid.UUID
        file_name: str
        file_uri: str
        gemini_file_name: Optional[str] = None
        batch_id: Optional[int] = None
        parent_theme: Optional[str] = None
        topic: Optional[str] = None
        description: Optional[str] = None
        uploaded_at: datetime

        class Config:
            from_attributes = True

    class DropdownOptionsResponse(BaseModel):
        themes: List[str] = Field(..., description="Unique list of available top-level educational themes.")
        topics: List[str] = Field(..., description="Unique list of available specific sub-topics.")


    class GamePayloadGenerationRequest(BaseModel):
        parent_theme: str = Field(..., description="Selected theme from administrative dropdown list.")
        topic: str = Field(..., description="Selected specific sub-topic from administrative dropdown list.")
        prompt: str = Field(..., description="Creative generation directive prompt from admin input.")
    ```


---

## 4.6 Generation Pipeline
1. **Administrative Input**: The admin selects an active `parent_theme` and `topic` from a dropdown menu fueled by `DropdownOptionsResponse` and provides a manual prompting guide.
2. **API Routing**: Next.js sends a `GamePayloadGenerationRequest` payload to FastAPI.
3. **Context Integration**: FastAPI parses the database to resolve `gemini_file_name`. If missing, it halts execution. If found, it routes a tokenized payload out to `gemini-3.5-flash`.
4. **Staging Save**: The generated string returned from Gemini is initially written to the PostgreSQL database with an explicit state setting of `STATUS: PENDING`.
5. **Administrative Review**: The pending data object is displayed inside the Next.js admin administrative panel for human validation.

---

## 4.7 Human-in-the-Loop Review
To manage safety and liability risks, human review is strictly mandatory within the platform's core architecture.

* **State Locking**: Newly generated educational game nodes are stored as `PENDING` drafts and remain structurally locked from the Flutter client application APIs. A player can never access or play a game module until its status code undergoes a manual transformation to `APPROVED`.
* **Operational Controls**: The Next.js dashboard equips administrators with explicit **Approve** and **Reject** control pathways.
* **System Event Tracking**: Administrative interactions, state selections, and transaction changes are captured, aggregated, and logged through Heroku's native operational environment logging stack rather than internal database transaction tables. This allows developers to track review actions directly through `stdout` pipelines.

---

## 4.8 Evaluation
During development, the engineering team prioritized iterative, qualitative validation of the AI platform's mechanics. Rather than deploying disconnected statistical test frameworks, the payload structures were directly evaluated and debugged using continuous Postman iteration.

Engineers manually examined the output JSON objects under edge-case prompts to guarantee that arrays, answer keys, crossword text strings, and layout shapes mapped perfectly to the designated Pydantic validation structures.

---

## 4.9 Limitations
* **Grounding Quality Dependencies**: The generation engine’s educational fidelity is entirely dependent on the quality, vocabulary depth, and content accuracy of the underlying Markdown source document.
* **Rigid Material Failure Cases**: If an administrative prompt requests a sub-topic that does not exist in the attached file search data layer, the LLM will output a structural failure notification string rather than attempting general reasoning.
* **No Embedded Professional Liability**: All output elements are generated by software and intended solely for protective, entry-level educational enrichment. The platform explicitly highlights that its contents do not constitute binding legal documentation, formal law enforcement records, or clinical mental health diagnoses.
