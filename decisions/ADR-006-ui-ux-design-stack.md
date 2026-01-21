# ADR-007: UI/UX Design Stack

**Project:** Enterprise Application Project (EAP)  
**Version:** 1.0  
**Date:** 2025-01-21  
**Status:** Accepted  

Link to Stitch AI: https://stitch.withgoogle.com/projects/10176214249598200718  
Link to Figma: https://www.figma.com/design/61giweY4P8ADs2sE8ib8Vl/EAP-Desk-X?node-id=0-1&t=bQVs190wiNR87RyS-1  

## Context

The project requires a design workflow for creating UI/UX artifacts that guide frontend development. Given our use of shadcn/ui components (per ADR-003), we need a design approach that:

- Enables rapid prototyping and iteration
- Produces designs that translate well to shadcn/ui components
- Fits within budget constraints
- Supports AI-assisted design generation

## Decision

We will use **Stitch AI** combined with **Figma** for UI/UX design work.

Designs will serve as sketches and drafts rather than pixel-perfect specifications. Developers will implement components using shadcn/ui, using the designs as visual references and guidelines.

## Rationale

- **Stitch AI** provides AI-powered design generation with generous free tier
- **Figma** offers industry-standard collaborative design capabilities
- This combination allows rapid creation of design drafts without requiring extensive design expertise
- The sketch-based approach aligns with shadcn/ui's component-driven development model

## Consequences

### Positive

- Fast design iteration with AI assistance
- Cost-effective solution within free tier limits
- Designs serve as clear guidance for shadcn/ui implementation
- Reduced dependency on dedicated UI/UX designers

### Negative

- Designs are drafts, not production-ready specifications
- Some interpretation required when translating to components
- AI-generated designs may need manual refinement

### Mitigation

- Establish clear conventions for design-to-component mapping
- Use shadcn/ui documentation as the source of truth for implementation details

## Alternatives Considered

### Option 1: v0 by Vercel (shadcn)

**Description:** AI-powered UI generation tool that outputs shadcn/ui code directly

**Pros:**
- Direct shadcn/ui code output
- Tight integration with our component library

**Cons:**
- Limited free tokens
- Restrictive usage limits for ongoing design work

**Rejection Reason:** Insufficient free tier allocation for sustained project use

### Option 2: Lovable

**Description:** AI design tool for generating UI components

**Pros:**
- AI-powered design generation
- Modern interface

**Cons:**
- Limited free tier tokens
- Similar cost constraints as v0

**Rejection Reason:** Free tier limitations make it unsuitable for ongoing design needs

## References

- [Stitch AI](https://stitch.ai/)
- [Figma](https://www.figma.com/)
- [Shadcn/ui Components](https://ui.shadcn.com/docs/components)
- [Shadcn/ui Forms](https://ui.shadcn.com/docs/forms/tanstack-form)
- [Shadcn/ui Dashboard example](https://ui.shadcn.com/examples/dashboard)
- ADR-003: Frontend Technology Stack
