# ORION — Experimental Local AI Assistant

ORION is an experimental local-first AI assistant project focused on exploring how language models can interact with external tools, manage persistent memory, and operate within clearly defined constraints.

The project is a practical engineering experiment rather than a production-ready assistant. Its development focuses on system architecture, tool validation, local model integration, and evaluating model behavior through testing.

## Project Goals

The main goals of ORION are:

- Integrate locally running language models.
- Implement persistent memory using SQLite.
- Allow the assistant to interact with external tools through a controlled interface.
- Explore safe interaction with Home Assistant.
- Evaluate model behavior through structured experiments.
- Investigate whether fine-tuning can improve specific model capabilities.

## Current Status

| Component | Status |
| --- | --- |
| Local LLM integration | Implemented |
| SQLite memory | Implemented |
| ToolRegistry | Implemented, tests passed earlier |
| Home Assistant integration | Experimental, preview/confirm tested earlier |
| QLoRA experiment | Conducted, inconclusive, not adopted |
| Voice interaction | System TTS fallback; Silero not launched |
| Public source code | Not published |

## Architecture and Engineering

### 1. Local Language Model

ORION uses Ollama to run a local language model.

The model used during development:

- Qwen2.5-3B-Instruct

The local model approach allows experimentation without depending entirely on external AI APIs.

### 2. Persistent Memory

SQLite is used as the persistent storage layer for assistant-related information.

The goal is to separate conversational and application data from the language model itself, allowing information to persist between sessions.

### 3. ToolRegistry

ToolRegistry provides a controlled interface between the language model and external tools.

The implemented validation rules include:

- Rejecting unknown tools.
- Requiring arguments to be provided as dictionaries.
- Checking for missing and unexpected arguments.
- Validating argument types.
- Explicitly handling the fact that Python `bool` is a subclass of `int`.

The default registry previously contained seven tools.

This approach is intended to prevent the model from directly executing arbitrary operations without passing through application-level validation.

### 4. Home Assistant Integration

ORION includes an experimental integration with Home Assistant.

The implementation includes:

- An allowlist-based restriction mechanism.
- Service argument validation, including `entity_id` and `service`.
- A confirmation requirement for actions marked as sensitive.
- A preview endpoint: `POST /api/home-assistant/action`.
- A confirmation endpoint: `POST /api/actions/{32-hex-id}/confirm`.

The preview/confirmation flow and selected validation rules were tested during earlier development.

However, replay protection, confirmation-token expiration, and the complete execution security model have not yet been fully verified.

### 5. Voice Interaction

Voice interaction was explored as part of the project.

Silero TTS was not successfully launched because the model digest had not been verified. A system TTS fallback was used instead.

Voice interaction remains an area for further development.

## QLoRA Fine-Tuning Experiment

One of the experiments investigated whether QLoRA fine-tuning could improve the model's ability to classify statements as confirmed, refuted, or unknown.

### Experiment Setup

- Base model: Qwen2.5-3B-Instruct.
- Dataset: 96 examples.
- Training set: 76 examples.
- Control set: 20 examples.
- Evaluation suite: 15 tasks.
- Training configuration: 20 steps.

### Results

| Classification | Baseline | Adapter |
| --- | ---: | ---: |
| Total correct | 9/15 | 10/15 |
| Confirmed | 1/5 | 2/5 |
| Refuted | 4/5 | 4/5 |
| Unknown | 4/5 | 4/5 |

The adapter improved the total score by one task.

However, this difference is too small to establish a reliable improvement. The adapter was therefore not adopted as the default model.

The experiment demonstrated that a small improvement in aggregate accuracy does not necessarily establish that fine-tuning has produced a meaningful or generalizable improvement.

## Verification Status

### Previously implemented and tested

*These results come from earlier development work and have not been re-run specifically for this documentation.*

- ToolRegistry validation rules and tests.
- Home Assistant preview/confirm flow.
- QLoRA experiment and its documented results.

### Not yet verified

*These items will be checked and updated as verification progresses.*

- [ ] Current automated test count (last reported: 129).
- [ ] Overlap between the 15 validation tasks and the training/control datasets.
- [ ] Scoring details of an earlier rationale-support metric (12 vs 11, denominator unknown).
- [ ] Replay and expiry behavior of confirmation tokens.

## Development Principles

The project follows several engineering principles:

- Validate inputs before executing tools.
- Separate model-generated decisions from application-level execution.
- Prefer persistent external memory over relying exclusively on model context.
- Evaluate behavior through measurable experiments.
- Document inconclusive and unsuccessful experiments.
- Avoid treating experimental results as proof without sufficient evidence.

## Future Development

Potential areas of further work include:

- Improving tool execution and validation.
- Strengthening Home Assistant action security.
- Expanding automated test coverage.
- Improving memory retrieval and management.
- Evaluating model reliability on larger and more diverse test sets.
- Revisiting fine-tuning when there is sufficient evidence and a clear objective.
- Developing a more reliable voice interaction pipeline.

These are planned directions, not claims about completed functionality.

## Source Code

**Code: Not published yet.**

The public repository will be updated when the source code is cleaned up, reviewed, and prepared for publication.

---

*ORION is an ongoing engineering project focused on understanding, testing, and building reliable AI systems.*
