# Remote OX API Domain Context

The headless job board backend API responsible for secure, low-latency, and ultra-low-cost job relay operations with AI-agent model context protocol integration.

## Language

**Consumer**:
Unified user record registering on the platform via Google OAuth.
_Avoid_: User, account, subscriber

**Candidate**:
A **Consumer** searching for vacancies, applying, and publishing their resume.
_Avoid_: Job seeker, applicant, worker

**Recruiter**:
A corporate **Consumer** who publishes vacancies and contacts candidates.
_Avoid_: Employer, hirer, corporate user

**Company Admin**:
A **Recruiter** with administrative rights to manage company profiles, manage associated recruiters, or request verification.
_Avoid_: Owner, account admin

**Company**:
A verified or unverified corporate entity associated with one or more recruiters.
_Avoid_: Corporation, enterprise, organization

**Job**:
An active, pending, or expired vacancy listing in a designated work area.
_Avoid_: Vacancy, posting, position

**Application**:
A candidate's explicit submittal to a specific **Job**, capturing resume and phone snapshots.
_Avoid_: Submission, apply request

**Wallet**:
An accounting record associated with a **Consumer** tracking their coin balance.
_Avoid_: Balance record, billing account

**Coins**:
The virtual currency unit of the platform (100 coins = $1.00 USD) used for purchases.
_Avoid_: Tokens, credits, points

**Message**:
A regulated communication payload sent between consumers, billed by payload size in kilobytes.
_Avoid_: Chat, text, email

---

## Relationships

- A **Consumer** has exactly one **Wallet**
- A **Consumer** can be associated with at most one **Company**
- A **Company** can have one or more associated **Recruiters**
- A **Recruiter** can create many **Jobs**
- A **Candidate** can submit many **Applications**
- A **Job** can receive many **Applications**
- A **Message** belongs to a sender and receiver **Consumer**, optionally referencing a **Job** context

---

## Example Dialogue

> **Dev**: "When a **Candidate** submits an **Application** to a **Job**, does the **Recruiter** pay to view their profile?"
> **Domain expert**: "No. The profile and contact details are snapshotted and shared for free during an active **Application**. The **Recruiter** only pays **Coins** from their **Wallet** if they choose to send a **Message** to a candidate outside of an application context or if they have exhausted their job's free message allowance."

---

## Flagged Ambiguities

- **Recruiter vs Company Admin**: Resolved that these are not separate tables or entities. They are roles under the `consumers` table. Creating a company upgrades a consumer to `company_admin` of that company, giving them permissions to add other recruiters.
- **Google OAuth access token**: Resolved that we do not persist OAuth tokens in SQLite for security. The token is checked on `/auth/mcp` and discarded immediately.
