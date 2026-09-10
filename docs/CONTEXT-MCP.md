# Remote OX MCP Server

The Model Context Protocol (MCP) server for REMOTE-OX, a decentralized job relay network. This server translates JSON-RPC 2.0 messages from AI client environments into validated, secure HTTP requests dispatched to the upstream API Server.

## Language

**Candidate**:
A user searching for employment who browses jobs, manages applications, and grants consent.
_Avoid_: Applicant, job seeker, developer

**Recruiter**:
A hiring representative who creates recruiter profiles, purchases job credits, searches candidates, and posts job listings.
_Avoid_: Employer, hiring manager, issuer

**Job Listing**:
A public posting of a job opening containing requirements, description, area, and region.
_Avoid_: Post, job post, vacancy

**Application**:
A Candidate's formal submission to a specific Job Listing, including an optional cover letter and profile resume.
_Avoid_: Submission, job apply, proposal

**Job Credit**:
A prepaid balance token in a Recruiter's wallet used as currency to activate a new Job Listing.
_Avoid_: Credit, balance, coin

**Consent History**:
A log of data sharing and privacy consents granted by a Candidate regarding their PII.
_Avoid_: Privacy history, consent logs

## Relationships

- A **Candidate** submits one or more **Applications**
- An **Application** maps a single **Candidate** to a single **Job Listing**
- A **Recruiter** posts one or more **Job Listings**
- A **Recruiter** consumes **Job Credits** to activate **Job Listings**
- A **Candidate** tracks their data sharing events through their **Consent History**

## Example dialogue

> **Dev:** "When a **Recruiter** wants to post a new **Job Listing**, do they purchase **Job Credits** during the submission?"
> **Domain expert:** "No — the **Recruiter** must have existing **Job Credits** in their wallet prior to posting, which are consumed upon activation of the **Job Listing**."
> **Dev:** "And how does the **Candidate** apply?"
> **Domain expert:** "The **Candidate** submits an **Application** targeting the specific **Job Listing**, and the system logs this action under their **Consent History**."

## Flagged ambiguities

- **"API Key" vs "Session Credentials"**: Clarified that in Stdio transport mode, the server uses a single static `REMOTE_OX_API_KEY` from the environment or local config file. In SSE transport mode, the server extracts dynamic `X-API-Key` headers on each incoming `POST` request.
- **"HTML Input" vs "Markdown Input"**: In all string arguments, HTML tags are completely stripped out, whereas Markdown links are validated using AST parsing to reject local filesystem access (e.g. `file://`).
