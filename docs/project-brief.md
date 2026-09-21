# Project brief

What this project is, why it exists, what's in and out of scope, and how the work is run. The system itself is described in the architecture overview.

## Background

My professional work is Salesforce development: custom applications for regulated clients, built mostly in Lightning Web Components and Apex. I'm moving into general full-stack web development, and I've been learning TypeScript, React, Node.js, Express and PostgreSQL in my own time. None of that work is public, so there's nothing a reviewer can look at or check.

This project changes that. It's a showcase, not a product: it's judged on what a reviewer finds in the repository and the live demo, not on how many people use it.

## What it is

A portal for a fictional B2B marketplace with two kinds of account: suppliers and buyers.

- Organisations sign up through a multi-step onboarding wizard. The early steps are the same for both, and one step depends on the account type.
- Submitting activates the account straight away. It's self-registration, with no approval step.
- Once the account is active, the same forms become an account editor.
- Later milestones add a proper login (password plus a one-time code by email) and a supplier directory, where buyers send requests and suppliers accept or decline them.

## Based on my Salesforce work

The design follows a greenfield B2B trade platform I've worked on professionally, a Salesforce build for a joint venture with a global bank. It's rebuilt from scratch on a general web stack, in a made-up domain. Nothing from the client is reused: no code, data, naming or design.

|Pattern from that work|Where it shows up here|
|---|---|
|One component serving both a multi-page onboarding wizard and the profile editor, from one form definition, one validation layer and one save path|Onboarding wizard (M1) and account editor (M2)|
|A component library themed with layered design tokens, with a second brand built from token overrides only|UI theming and the second brand (M1, M2)|
|An integration moved from a shared key to OAuth without changing any of its callers|Identity: sessions from the first milestone, with login swapped in later without changing the API (M1 to M3)|
|One login component for activation, returning login and password reset, all ending on an emailed one-time code|Login (M3)|
|A connections page of cards with actions such as accept and decline|Supplier directory and requests (M4)|

## Objectives

- A live, public application on the target stack, with a readable codebase, tests and CI, that a reviewer can make sense of in a few minutes.
- Visible planning: this brief, an architecture overview, decisions recorded in a decision log, a public backlog, small pull requests and automated deployment.
- Demonstrate the tech lead / solution architect side of the job alongside the development.
- Each milestone finishes as a tagged release with its exit criteria met.

## What this shows

Each technology I want to show, and where it's used. Some choices are still open and get settled as the build goes. The milestones are described further down.

|Area|Technology|Where it's used|From|
|---|---|---|---|
|Core|TypeScript|Everything: the browser app, the API, and the shared form definitions and validation rules|M0|
|Core|React|The browser app: onboarding wizard and account editor, later the login screens and request cards|M1|
|Core|Node.js and Express|The API: sessions, saving and submitting applications, later login and requests|M0|
|Core|PostgreSQL|Accounts, users, profiles and sessions, later one-time codes and requests|M0|
|Front end|Vite|Building the browser app and running it locally|M0|
|Front end|Tailwind CSS and shadcn/ui|UI components themed with design tokens, and a second brand|M1, M2|
|Shared|Zod|Validation rules written once and used by both the browser app and the API|M1|
|Data|To be decided in M0|Database access and migrations|M0|
|Testing|To be decided|Unit tests, API tests against a real database, and one end-to-end test|M0, M1|
|Delivery|GitHub Issues, Projects and pull requests|Backlog, board and milestones|M0|
|Delivery|GitHub Actions|Lint, type check, tests and build on every pull request, required before merge|M0|
|Delivery|Render and Neon|Hosting and managed Postgres, deploying on every merge to main|M0|

## Scope

The work is split into five milestones, described under Milestones below: foundations, onboarding, account management, login, and the supplier directory with requests. The first release, v0.1.0, is the onboarding milestone, with four steps: contact details, company details, supplier or buyer details, then consent and submit.

Not doing for now. Anything here that might come later sits on the board as backlog rather than being forgotten:

- file uploads, such as company logos
- admin or moderation tools
- more than one user per account
- translations
- native mobile apps

## Approach

Plan first, then build. This brief, the architecture overview and the first decision log entries go up before any feature code.

The first milestone gets an empty version of the app working end to end, with the frontend, API, database, CI and hosting all connected and live before there are any real features. After that, every merge goes straight to the live site. In M1, the first wizard step gets finished completely (saving, resuming and live) before the other steps are added.

Agile practices are kept where they make sense for one person and dropped where they don't.

|Practice|Used?|How, or why not|
|---|---|---|
|Prioritised backlog|Yes|Issues on a GitHub Projects board, tagged must, should or could. Anything cut stays visible and is marked as cut|
|Acceptance criteria|Yes|Every issue has them before work starts|
|Definition of done|Yes|A checklist in the pull request template|
|Trunk-based development|Yes|Short-lived branches off main, one small pull request per issue, squash merged|
|Continuous integration|Yes|Lint, type check, tests and build on every pull request, required before merge|
|Continuous deployment|Yes|Merging to main deploys|
|Retrospective|Yes|A short note at the end of each milestone|
|Decision log|Adapted|A lightweight version of ADRs: one file, with a short entry per significant decision|
|Sprint review|Adapted|Each milestone ends in a tagged release and an updated README|
|Code review|No|Sole person|
|Sprints|No|Fixed timeboxes don't mean much for one part-time person. Milestones end when their exit criteria are met|
|Estimates, story points, velocity|No|There's no team capacity to plan against|
|Daily stand-up|No|No team to sync with|

## Milestones

|Milestone|Release|Done when|
|---|---|---|
|M0 Foundations|none|A merged pull request reaches the live site with no manual step. The landing page shows the API and database are up. The board, README, this brief, the architecture overview and the first decision log entries (stack, repo layout and hosting) are public|
|M1 Onboarding|v0.1.0|A visitor can start as a supplier or buyer, complete every step, leave, come back in the same browser and submit. The browser and the API use the same validation rules. Tests run in CI. The README explains the design and its known limitations|
|M2 Account management|v0.2.0|An active account edits every section through the same forms and save path as the wizard. A reviewer can try it without onboarding. A second brand theme switches live|
|M3 Login|v0.3.0|Registration, returning login and password reset all go through one emailed-code step, and nothing in the existing API had to change for it|
|M4 Directory and requests|v0.4.0|A buyer sends a request to a listed supplier, the supplier accepts or declines, and both see the status. Contact details stay hidden until a request is accepted. Tests prove one account can't change another's records|

Detailed design for M3 and M4 happens when the milestone before is nearly finished, not now.