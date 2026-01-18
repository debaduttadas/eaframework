# Architecture Principles - Guidance

## 1. Purpose

This document provides **guidance on developing and applying architecture principles** based on TOGAF methodology. It explains the structure, components, and best practices for creating effective architecture principles that guide decision-making across the organisation.

---

## 2. What are Architecture Principles?

Architecture principles are general rules and guidelines that inform and support the way an organisation sets about fulfilling its mission. They reflect a level of consensus across the enterprise and embody the spirit and thinking of the organisation's strategic direction.

**Key Characteristics:**
* Principles are **enduring** - they should remain stable over time
* Principles are **few in number** - typically 10-20 high-level principles
* Principles are **technology-agnostic where possible** - they guide decisions rather than prescribe solutions
* Principles provide **decision-making guardrails** - they help evaluate options and trade-offs

---

## 3. Components of an Architecture Principle

According to TOGAF, each architecture principle should contain four components:

### 3.1 Name

The name should represent the essence of the principle and be easily remembered. It should be clear and unambiguous.

**Guidelines:**
* Keep it concise (typically 3-7 words)
* Make it memorable and meaningful
* Avoid jargon where possible
* Use active, positive language

### 3.2 Statement

A formal statement of the principle. This should be a clear, concise declaration that can be understood by all stakeholders.

**Guidelines:**
* Use imperative language ("must", "should")
* Be specific enough to guide decisions
* Be general enough to remain durable
* Avoid implementation details
* Keep to 1-2 sentences

### 3.3 Rationale

The rationale explains **why** the principle is important and what benefits it provides. It highlights the business value and reasoning behind the principle.

**Guidelines:**
* Explain the business or technical drivers
* Articulate the benefits of following the principle
* Connect to organisational strategy or objectives
* Make the case for why this matters
* Keep to 2-3 sentences

### 3.4 Implications

The implications describe **what following the principle means** in practice. They outline the requirements, costs, and consequences of applying the principle.

**Guidelines:**
* Describe practical impacts on the organisation
* Identify what must change or be done differently
* Highlight potential costs or trade-offs
* Provide actionable guidance
* Use bullet points for clarity
* Include both positive and challenging implications

---

## 4. Developing Effective Principles

### 4.1 Principles Hierarchy

Principles typically exist at multiple levels:

1. **Enterprise Principles** - Apply across the entire organisation (business, data, application, technology)
2. **Domain-Specific Principles** - Apply to specific domains (e.g., Technology & Infrastructure, Data, Security)
3. **Implementation Guidelines** - Detailed guidance for specific contexts

### 4.2 Quality Criteria

Good architecture principles should be:

* **Understandable** - Clear to all stakeholders, not just technical staff
* **Robust** - Definitive and precise, not ambiguous
* **Complete** - Sufficient to guide decision-making
* **Consistent** - Not contradicting other principles
* **Stable** - Enduring over time, not subject to frequent change
* **Testable** - Possible to determine if they are being followed
* **Organisation-agnostic** - Avoid referencing specific teams, roles, or organisational units that may change; focus on activities and outcomes rather than who performs them

### 4.3 Common Pitfalls to Avoid

* **Too prescriptive** - Principles should guide, not dictate specific technologies or solutions
* **Too vague** - Principles must be specific enough to influence decisions
* **Too many** - A large number of principles becomes unmanageable
* **Conflicting** - Principles that contradict each other create confusion
* **Ignored implications** - Failing to acknowledge costs or challenges reduces credibility

---

## 5. Applying Principles in Decision-Making

### 5.1 When to Reference Principles

* During architecture design and review
* When evaluating technology choices
* When making build vs buy decisions
* When assessing exceptions or deviations
* When documenting Architecture Decision Records (ADRs)

### 5.2 Handling Principle Conflicts

When principles conflict in a specific situation:

1. **Acknowledge the conflict** - Be explicit about which principles are in tension
2. **Evaluate trade-offs** - Assess the implications of prioritising one principle over another
3. **Consider context** - Some principles may be more critical in certain situations
4. **Document the decision** - Explain the rationale in an ADR
5. **Escalate if needed** - Seek governance approval for significant deviations

### 5.3 Exception Management

Not all situations can conform to all principles. When exceptions are necessary:

* **Document the exception** - Explain why the principle cannot be followed
* **Justify the deviation** - Provide business or technical rationale
* **Assess the impact** - Understand the consequences of the exception
* **Define mitigation** - Identify how to minimise negative impacts
* **Obtain approval** - Follow the governance process for exceptions
* **Review periodically** - Exceptions should be temporary where possible

---

## 6. Governance and Maintenance

### 6.1 Principle Ownership

* **Enterprise Architecture** - Owns and maintains enterprise-level principles
* **Domain Architects** - Own domain-specific principles (aligned with enterprise principles)
* **Architecture Review Board** - Approves new principles and significant changes
* **All Architects** - Responsible for applying and promoting principles

### 6.2 Review and Evolution

Principles should be reviewed:

* **Annually** - Regular review to ensure continued relevance
* **When strategy changes** - Major business or technology shifts may require updates
* **When patterns emerge** - Repeated exceptions may indicate a principle needs revision
* **When feedback suggests** - Stakeholder input on principle effectiveness

### 6.3 Communication and Adoption

Effective principles require:

* **Clear documentation** - Accessible to all stakeholders
* **Training and education** - Help teams understand and apply principles
* **Visible leadership support** - Executive endorsement of principles
* **Consistent enforcement** - Apply principles fairly across the organisation
* **Success stories** - Share examples of principles driving good outcomes

---

---

## 7. Example: Anatomy of a Well-Formed Principle

**Principle Name:** Control Technology Diversity

**Statement:**
New technologies, services, and tools must be evaluated and approved from an enterprise perspective before adoption.

**Rationale:**
Uncontrolled technology proliferation increases complexity, operational overhead, security risk, and total cost of ownership. A standardised technology portfolio enables knowledge sharing, reduces duplication, and improves maintainability.

**Implications:**
* Technology choices must be evaluated against existing enterprise standards and capabilities
* Justification required for introducing technology that duplicates existing capabilities
* Approved technology stack maintained as part of enterprise architecture standards
* Exceptions require formal architecture review and approval
* Technology decisions must consider long-term support, skills availability, and integration complexity
* Consultation with architecture governance required before introducing new technologies

---

## 8. Document Control

| Field | Value |
|-------|-------|
| **Owner** | Enterprise Architecture Team |
| **Contact** | enterprise.architecture@company.com |
| **Current Version** | 1.0 |
| **Applies To** | All architecture principle development |

## 9. Change Log

| Version | Date | Author | Description |
|---------|------|--------|--------------|
| 1.0 | 2026-01-18 | Enterprise Architecture | Initial guidance based on TOGAF methodology |

---

## 10. References

* TOGAF Standard - Architecture Principles
* ISO/IEC/IEEE 42010:2011 - Systems and software engineering — Architecture description
* Organisation's Architecture Decision Record (ADR) template

---

## 11. Summary

This guidance provides a TOGAF-based approach to developing and applying architecture principles. By following the four-component structure (Name, Statement, Rationale, Implications) and adhering to quality criteria, organisations can create effective principles that guide decision-making and support strategic objectives.

