# Trace Desk AI Prompt Documentation

## Overview

Trace Desk uses structured prompts to guide Generative AI in analyzing business requirements and generating Business Analyst and QA artifacts.

The prompts were designed to provide context, define the AI role, specify expected outputs, and encourage validation of assumptions.

---

# Prompt 1: Requirement Gap Analysis

## Objective

Identify missing information, ambiguity, risks, and assumptions within a business requirement.

## AI Role

Act as an experienced Business Analyst reviewing a software requirement.

## Prompt Approach

The AI was instructed to:

- Analyze requirement completeness
- Identify missing business rules
- Highlight unclear statements
- Identify security or compliance concerns
- Generate stakeholder clarification questions

## Expected Output

- Requirement gaps
- Impact analysis
- Clarification questions

---

# Prompt 2: User Story Generation

## Objective

Convert analyzed requirements into Agile user stories.

## AI Role

Act as a Business Analyst working in an Agile Scrum team.

## Prompt Approach

The AI was instructed to create:

- User story format
- User role
- Business value
- Supporting details

## Expected Output

Example:

As a customer,
I want to reset my password,
so that I can regain account access.

---

# Prompt 3: Acceptance Criteria Generation

## Objective

Create testable acceptance criteria.

## AI Role

Act as a QA Analyst collaborating with a Business Analyst.

## Prompt Approach

The AI was instructed to:

- Create Given/When/Then scenarios
- Ensure criteria are measurable
- Identify missing validation rules

## Expected Output

Gherkin acceptance criteria.

---

# Prompt 4: QA Test Scenario Generation

## Objective

Generate test coverage from requirements.

## AI Role

Act as a Quality Assurance Analyst.

## Prompt Approach

The AI was instructed to create:

- Positive test scenarios
- Negative test scenarios
- Security-related scenarios
- Expected results

---

# Prompt Improvement Approach

Prompt quality was improved by:

- Providing clear context
- Defining AI responsibilities
- Specifying output format
- Reviewing and refining results
- Adding human validation steps

---

# Human Review

AI-generated outputs were reviewed before being added to project documentation.

Validation included:

- Requirement accuracy
- Business relevance
- Testability
- Completeness
