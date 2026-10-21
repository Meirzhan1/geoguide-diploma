# GeoGuide

**Design and Implementation of a Context-Aware Tourist Guide System Incorporating Machine Learning Recommendations and Generative AI**

GeoGuide is a mobile tourist guide for places in Kazakhstan. The tourist points the phone camera at a landmark and receives an explanation, asks an AI guide questions, gets a notification when arriving near an interesting place, and receives recommendations based on personal interests.

> Diploma project, Software Engineering, Astana IT University (AITU), groups SE-2409 and SE-2427.

---

## Problem Statement

A tourist in an unfamiliar place usually cannot quickly get reliable information about what they see. Printed guidebooks and audio guides are static and not tied to the visitor's position or interests. Searching the internet requires the tourist to know what to look for and distracts them from the visit. Human guides are expensive and not always available. Existing digital products cover parts of this need, but no single reviewed product combines camera recognition, a source-grounded AI guide, arrival notifications and interest-based recommendations for Kazakhstan (see the [existing systems comparison](docs/04_Existing_Systems_Comparison.docx)).

GeoGuide combines these in one application and answers only from a verified knowledge base, saying so when it does not know.

## Target Users

- **Primary:** tourists visiting Kazakhstan (including English- and Russian-speaking visitors) who want instant, reliable information about places they see.
- **Secondary:** local residents showing their city or region to guests.
- **Stakeholders:** city and tourism organizations that could provide verified content (no formal agreement exists).

## Key Features

1. **Camera recognition.** Show a landmark to the camera and get its name and a short explanation. The place is identified by comparing the photo with reference images of known objects, narrowed by GPS. If confidence is low, the app does not guess and offers retry, search or the AI chat.
2. **AI guide chat.** Ask questions about a place. Answers are generated from retrieved passages of a verified knowledge base, show their source, and the guide refuses when no source supports an answer.
3. **Arrival notifications.** When the tourist comes within a chosen distance (100, 200 or 500 m) of a place, a notification opens the place page.
4. **Personalized recommendations.** Places are suggested by declared interests, distance and opening hours, each with a reason ("Because you like History").

## Scope

**Coverage.** The project targets tourist places across Kazakhstan. The prototype is piloted in six regions with 12 verified objects (4 well-known and 8 less-visited places); new regions and objects are added as data, without code changes.

| Region | Pilot objects |
|---|---|
| Astana | Baiterek Tower, Khan Shatyr |
| Almaty (city) | Zenkov Cathedral |
| Almaty Region | Charyn Canyon, Kolsai Lakes, Lake Kaindy, Tamgaly Petroglyphs |
| Turkistan Region | Mausoleum of Khoja Ahmed Yasawi, Arystan Bab Mausoleum |
| Jambyl Region | Aisha Bibi Mausoleum, Akyrtas Palace Complex |
| Mangystau Region | Bozzhyra Tract |

Eight of the twelve objects are deliberately less visited places for which structured information is harder to find. Whether general tools (Google Maps, Lens, ChatGPT) cover them poorly is a hypothesis that is tested in Sprint 2 (see docs/09, coverage-gap check), not a finding.

**In scope:** recognition of the pilot objects with confidence threshold and fallback; AI chat over the knowledge base with sources and refusal; arrival notifications with adjustable distance; interest-based recommendations; saved places; backend API, database and mobile client.

**Out of scope:** recognition of arbitrary buildings, national coverage within the project, route planning and navigation, ticket booking and payments, AR overlays, gamification, audio narration, production deployment.

## Team and Supervisor

| Role | Name | Main responsibilities |
|---|---|---|
| Student, full-stack developer | Meirzhan Yerkebay (Yerkebay Meirzhan Yesbolatuly), SE-2409 | Backend API, database and knowledge base, AI guide (retrieval and answer generation), recommendations, requirements and architecture documents |
| Student, full-stack developer | Sarsenbek Beknazar, SE-2427 | Mobile client, landmark recognition and evaluation, geofencing and notifications, prototype |
| Supervisor | Alua Myrzakerimova | Supervision and review of key deliverables |

Both students are full-stack developers and can work on any component. Each work item in YouTrack has one responsible student and a reviewer from the other; individual contributions are visible in the commit history and pull requests.

## Current Status

**Stage: analysis and design (first pre-defense, 24 October 2026).** No runnable code yet.

- [x] Project proposal and scope ([docs/01](docs/01_Project_Proposal.docx))
- [ ] Evidence of relevance: desk evidence done, interview results to be added ([docs/02](docs/02_Evidence_of_Relevance.docx))
- [x] Literature review ([docs/03](docs/03_Literature_Review.docx))
- [x] Existing system comparison ([docs/04](docs/04_Existing_Systems_Comparison.docx))
- [x] Technology comparison ([docs/05](docs/05_Technology_Comparison.docx))
- [x] Requirements and acceptance criteria ([docs/06](docs/06_Requirements_Specification.docx))
- [x] Architecture diagrams ([docs/07](docs/07_Architecture_and_Diagrams.docx), [diagrams/](diagrams/))
- [x] UX prototype (clickable HTML prototype, see Links)
- [x] Verification and evaluation plan ([docs/09](docs/09_Verification_and_Evaluation_Plan.docx))
- [x] Project management and risks ([docs/10](docs/10_Project_Management_and_Risks.docx))
- [ ] Implementation

## Technologies (proposed)

Choices and alternatives are justified in the [technology comparison](docs/05_Technology_Comparison.docx). They are validated during implementation.

| Area | Technology |
|---|---|
| Mobile client | React Native with Expo (development build, because background location is needed) |
| Backend / REST API | Python, FastAPI, OpenAPI documentation |
| Database | PostgreSQL with PostGIS (distance queries) and pgvector (similarity search) |
| Landmark recognition | Image-embedding retrieval over a reference photo set, filtered by GPS; cloud landmark-detection API as a baseline |
| Generative AI (chat) | Hosted language model with retrieval-augmented generation, source labels and refusal (provider selected in Sprint 3) |
| Recommendations (ML) | Content-based ranking by interests, distance and opening hours |
| Geolocation and notifications | OS geofencing with a dynamic set of nearest places, local notifications |
| Packaging | Docker Compose |
| Version control and tasks | Git, GitHub, YouTrack (Scrum) |

## Links

- **Repository:** https://github.com/Meirzhan1/geoguide-diploma
- **YouTrack (Scrum, key GEO):** https://meirzhan.youtrack.cloud/projects/GEO
- **UX prototype:** https://claude.ai/artifact/PXmUW71qvCEPCHBa92Vz5S (clickable; sample data)
- **Documentation:** [`docs/`](docs/) (Word documents and the [interview guide](docs/interview-guide.md))
- **Diagrams:** [`diagrams/`](diagrams/) (editable `.dot` sources and exported `.png`/`.svg`)

## Repository Structure

```
geoguide-diploma/
├── src/          # application code, by component
├── docs/         # proposal, reviews, comparisons, requirements, architecture, plans
├── diagrams/     # editable sources and exports
├── tests/        # automated tests and test materials
├── config/       # dependency definitions and configuration examples
└── README.md
```

## Setup, Execution, and Testing

Instructions will be added as soon as runnable code is available.

## Contributing Workflow

- Every work item is a YouTrack task (`GEO-<number>`).
- Branch name: `GEO-23-short-description`. Commit message: `GEO-23: Add initial database schema`.
- Changes go to `main` only through a pull request that describes the change, references the task and explains how it was checked. The other student reviews it before merging.
- Secrets (API keys, passwords) are never committed; use configuration examples with placeholder values.
- Each pre-defense submission is marked with a tag: `pre-defense-1`, `pre-defense-2`, `final-pre-defense`.
