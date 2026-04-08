# Architecture Prototype

Mermaid diagrams to visualize system design before building.

## Process

1. Read spec.md for requirements and constraints
2. Generate HLD (high-level design): system boundaries, data flow, external integrations
3. Generate LLD (low-level design): component interactions, sequence diagrams
4. Save to `.claude/prototypes/{task-id}-architecture.md`
5. Present to user for feedback
6. Update spec/plan based on feedback

## Diagram Types

- **System Context**: what systems interact with this feature
- **Component Diagram**: internal components and their relationships
- **Sequence Diagram**: step-by-step flow for key operations
- **ER Diagram**: data model changes (if applicable)

Use Mermaid syntax for all diagrams.
