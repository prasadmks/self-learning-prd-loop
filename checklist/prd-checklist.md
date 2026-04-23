# PRD Self-Evaluation Checklist

A 20-point quality standard for agentic AI product requirements documents.
Used as the evaluation baseline for the Self-Learning PRD Loop.

---

## Section 1 — Problem Definition

- [ ] Have I clearly articulated the problem and job-to-be-done?
- [ ] Have I defined the customer persona(s) with specific roles, industries, and needs?
- [ ] Have I validated the problem with real data or market research?
- [ ] Have I explained why this problem is worth solving, especially for my target user?
- [ ] Have I defined a clear and defensible moat?
- [ ] Have I justified the use of agentic AI over rule-based systems?
- [ ] Have I listed types of unstructured data involved and explained the need for ML/LLMs over rule-based solutions?
- [ ] Have I differentiated the solution from ChatGPT, copilots, or other tools?

## Section 2 — Solution Definition

- [ ] Have I included a visual user flow (input → processing → output)?
- [ ] Have I addressed potential AI drawbacks (hallucination, explainability, etc.)?
- [ ] Have I listed core functional requirements in user story format?
- [ ] Have I clearly described agent capabilities and system behavior?

## Section 3 — Core Metrics

- [ ] Have I defined a clear North Star metric?
- [ ] Have I listed at least 1–2 primary metrics (e.g. accuracy, time saved)?
- [ ] Have I included secondary or supporting metrics (e.g. user adoption, cost reduction)?
- [ ] Are metrics measurable and trackable over time using the SMART framework?

## Section 4 — Prioritization

- [ ] Have I broken the agentic workflow into components (e.g. ingestion, extraction, summarization)?
- [ ] Have I conducted a risk assessment for each component?
- [ ] Have I specified ML feasibility, data availability, accuracy expectations, and explainability?
- [ ] Have I prioritized features based on risk, cost, and value?
- [ ] Have I clearly narrowed scope to an MVP with rationale?

## Section 5 — Roadmap

- [ ] Have I listed clear phases: MVP, MVP1, Launch, Iteration?
- [ ] Have I defined feature sets and target durations for each phase?
- [ ] Have I linked roadmap items back to priorities and risks?

## Section 6 — Evaluation Plan

- [ ] Have I described how AI performance will be evaluated over time?
- [ ] Have I identified datasets or ground truth sources for benchmarking?
- [ ] Have I defined accuracy thresholds or HHH (Helpful, Honest, Harmless) scores?
- [ ] Have I included a plan for maintaining an evaluation dataset?

## Section 7 — Prompt Strategy

- [ ] Have I described few-shot, CoT (Chain-of-Thought), or conditional prompting where applicable?
- [ ] Have I defined prompt constraints and output formats?
- [ ] Have I included plans for improving prompts based on user feedback?

## Section 8 — Responsible AI Risks & Mitigation

- [ ] Have I assessed risks across Accountability, Transparency, Fairness, and Reliability?
- [ ] Have I planned for human-in-the-loop and fallback paths?
- [ ] Have I listed sensitive use cases and mitigation strategies?
- [ ] Have I addressed bias, hallucination, and safe failure modes?

## Section 9 — Pricing & Cost Structure

- [ ] Have I explained tradeoffs between accuracy and cost?
- [ ] Have I provided a breakdown of development costs (infrastructure + manpower)?
- [ ] Have I listed ongoing operational costs?
- [ ] Have I calculated market size (TAM, SAM)?
- [ ] Have I defined potential revenue scenarios?
- [ ] Have I selected a pricing model (subscription, usage-based, freemium, etc.) with rationale?
- [ ] Have I included a directional pricing recommendation?
