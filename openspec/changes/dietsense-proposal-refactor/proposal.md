# Proposal: DietSense Proposal Refactor using OpenSpec

## Summary

Refactor the DietSense project proposal into a spec-driven development structure using the OpenSpec framework. DietSense is a conversational domain AI agent for daily calorie intake analysis and personalized weight management, targeting Taiwanese users. This change converts the monolithic proposal document into modular, traceable OpenSpec artifacts—enabling incremental development, clear requirement traceability, and structured evaluation planning.

## Motivation

The current DietSense proposal exists as a single, large document that interleaves requirements, design decisions, literature review, and evaluation plans. This monolithic structure creates several problems:

1. **Traceability Gap** — It is difficult to trace which design decisions address which requirements, or which evaluation criteria validate which claims.
2. **Modularity** — Changes to one section (e.g., updating the architecture) may silently invalidate assumptions in another (e.g., evaluation methodology).
3. **Collaboration Friction** — Multiple contributors cannot easily work on independent sections without merge conflicts or duplicated effort.
4. **Incremental Development** — Without discrete, well-scoped specs, it is unclear what to implement first or how to validate progress.

By refactoring into OpenSpec artifacts, each concern—requirements, design, evaluation—becomes an independently versioned, cross-referenced document with clear ownership and acceptance criteria.

## Scope

### In Scope

- **Abstract (A–F):** Map the six abstract components (problem statement, approach, key innovations, target users, evaluation plan, expected contributions) to formal spec requirements.
- **Introduction:** Extract research questions, problem definition, and contribution claims into traceable requirement IDs.
- **Related Work:** Summarize prior art and position DietSense's novelty; link each gap identified to a corresponding spec requirement.
- **Deep Research (Q1–Q10):** Convert the ten deep-research questions and their findings into formal requirements with acceptance criteria and priority levels.
- **System Design (8-Module Architecture):** Document the eight-module architecture in the design artifact, with mermaid diagrams, data flow, component interactions, and formula specifications.
- **Evaluation Plan:** Define success criteria, RCT methodology, and benchmark design as verifiable acceptance criteria within specs.

### Out of Scope

- **Full Implementation Code** — This change produces planning artifacts only; no source code is written or modified.
- **Deployment Infrastructure** — Cloud provisioning, CI/CD pipelines, and production environment setup are deferred to a future change.
- **UI/UX Prototyping** — Wireframes, mockups, and frontend implementation are not included.
- **Dataset Collection** — Actual data gathering (e.g., Taiwan night market food photography, nutritional lab analysis) is deferred.

## Success Criteria

| # | Criterion | Verification |
|---|-----------|-------------|
| 1 | All proposal sections are mapped to at least one spec requirement | Cross-reference table in design.md |
| 2 | Every spec requirement has an ID, description, acceptance criteria, and priority | Review of spec.md |
| 3 | The 8-module architecture is fully documented with mermaid diagrams | Visual inspection of design.md |
| 4 | Design decisions are traceable to requirements | Requirement-to-module mapping table in design.md |
| 5 | Deep Research findings (Q1–Q10) are captured as formal requirements | Review of spec.md detail requirements section |
| 6 | Safety mechanisms are explicitly specified with quantitative thresholds | Acceptance criteria for safety-related requirements |
| 7 | Trade-off decisions are documented with rationale | Trade-offs section in spec.md |

## Key Innovations Addressed

1. **Natural Language Input (NLI)** — Users describe meals in free-form Chinese text; the system parses, disambiguates, and estimates nutritional content.
2. **Proactive Agentic Interaction (PAI)** — The agent initiates clarification questions, suggests portion adjustments, and provides unsolicited health alerts.
3. **Context Awareness (CA)** — Temporal, social, and behavioral context (meal timing, stress levels, dining companions) informs recommendations.
4. **Big Data-Driven Intelligence (BDI)** — Large-scale food composition databases, user history, and population-level dietary patterns power personalized advice.

## References

- OpenSpec Framework Documentation
- DietSense Original Proposal Document
- USDA FoodData Central API Documentation
- Taiwan Food and Drug Administration — Food Composition Database
- Mifflin-St Jeor et al. (1990). A new predictive equation for resting energy expenditure.
- Harris-Benedict (1918). A biometric study of basal metabolism in man.
