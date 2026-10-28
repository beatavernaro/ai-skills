---
name: readme-writer
description: Analyze a software repository and create or improve an accurate, developer-focused README.md following GitHub's README recommendations and evidence-based writing rules. Use when asked to create, rewrite, audit, or improve a project README.
---

# README Writer

Create an informative, approachable, technically accurate `README.md` that helps the repository's intended audience understand, run, use, and contribute to the project. Use GitHub Flavored Markdown. Write in the language requested by the user; otherwise follow the repository's existing documentation language, or use English if none is established.

## Operating principles

- **Repository evidence is the source of truth.** Inspect relevant files before writing. Do not execute untrusted repository scripts merely to discover facts.
- **Omit rather than fabricate.** Every technical statement, command, path, version, feature, compatibility claim, badge, and link must be supported by repository evidence or explicitly supplied by the user. Do not insert placeholders, `TBD`, speculative commands, or invented URLs.
- **Preserve intent.** Treat existing README content as a source of potentially useful facts, examples, and project voice, not as proof that claims are still accurate. Revalidate claims against the current workspace. Do not silently erase valuable project-specific information.
- **Make the README useful, not exhaustive.** Prioritize onboarding. Put detailed API reference, extensive troubleshooting, license text, and full contribution procedures in separate documents when those documents exist; link to them instead.
- **Avoid surprise writes.** Before replacing or substantially restructuring an existing README, present a short proposed outline and describe any important sections that would be removed or changed; ask for approval. For a new README, draft it when requested. Do not modify unrelated files without explicit permission.
- **No unnecessary dependencies.** This skill requires only repository inspection and Markdown authoring. Do not install tools, plugins, or packages unless separately authorized.

## Workflow

### 1. Inspect the repository

Determine the intended reader and the actual usage model (library, CLI, API, web application, service, infrastructure project, sample, etc.). Inspect relevant evidence, as available:

- Repository root, project structure, existing README and docs, LICENSE, CONTRIBUTING, security/support files.
- Manifests, lockfiles, runtime configuration, launch profiles, Dockerfiles, Compose files, infrastructure and CI workflows.
- Entrypoints, public interfaces, routes or commands, tests and runnable examples.
- Scripts, environment variable names and templates, required services, deployment and configuration instructions.

Read selectively: focus on files needed to substantiate the README. For large repositories, use targeted searches instead of dumping the entire codebase into context. Inspect git history only when available and useful; do not treat historical claims as current without corroboration.

Never expose secrets. Do not print `.env` values, tokens, credentials, private endpoints, or confidential operational details into the README. Environment variable *names* may be documented when appropriate; use safe dummy example values only when clearly labeled as examples.

### 2. Plan content according to evidence

Cover these questions, combining or omitting headings as appropriate:

1. **What does this project do?** Project name, succinct purpose and intended users.
2. **Why is it useful?** Concrete, evidenced capabilities and relevant benefits.
3. **How do I get started?** Prerequisites, installation, configuration, run commands and a working usage example, in the order a new developer needs them.
4. **Where can I get help?** Real issues, discussions, support or docs links, if they exist.
5. **Who maintains or contributes?** Confirmed maintainers, contribution instructions or links to existing contributing guidelines, if available.

Optional content only when useful and evidenced: architecture overview, project structure, test commands, environment variables, deployment, screenshots, API overview, status, roadmap and acknowledgments. Prefer linking to detailed documents rather than reproducing them. Do not create empty sections to match a template.

If important getting-started information is missing, report the gap separately instead of guessing. Do not imply a command is verified by execution if it was only found in a manifest or documentation.

### 3. Draft in a human, project-specific voice

Apply all these rules on both the first draft and the final editing pass:

**Avoid generic AI-sounding language.** Do not open with "Welcome to", "This project aims to", or broad scene-setting. Do not use unsupported superlatives such as "blazing fast", "seamless", "robust", "powerful", "cutting-edge", "state-of-the-art", "enterprise-grade", or "next-generation". If a benchmark exists, give the actual measured figure and its context instead of an adjective. Do not use marketing-like stack restatements such as "Built with modern technologies" or empty sign-offs such as "Happy coding!".

**Be concrete.** Refer to actual class, function, module, project and file names; real endpoints, flags and commands; genuine configuration keys. Vary sentence openings. Use concise, declarative sentences and short focused paragraphs. Match the voice and depth to the audience indicated by the repository. Do not pad thin sections with invented features.

**Make claims traceable.** Pull versions from manifests or lockfiles. Mention performance or coverage figures only when credible current evidence is available. Treat examples from the existing README as candidates for verification, not automatically as facts. If a claim cannot be corroborated, omit it or flag it for the user outside the README.

**Badges require evidence.** Only include a CI badge if a relevant workflow and its repository URL are known; a license badge if the license is established; a version badge if a real version exists. Do not invent coverage, quality or deployment badges. Verify badge URLs and branches as far as available tools allow. When the remote URL or account is unknown, omit dynamically generated badges.

### 4. Follow GitHub Markdown conventions

- Use GitHub Flavored Markdown with one clear H1 title, meaningful hierarchical headings, fenced code blocks with language identifiers and readable tables only where tables add clarity.
- Prefer relative links for files in the repository (`docs/guide.md`), and ensure links work from the README's location in a clone.
- Include copyable, OS-appropriate setup and usage commands only when backed by existing scripts, configuration or confirmed instructions. Mark illustrative commands as examples when necessary.
- Keep it scannable. GitHub's automatic table of contents uses heading structure; add a manual TOC only when the document genuinely needs one.
- Keep the README below 500 KiB; aim for substantially less. Avoid detailed API documentation, extensive troubleshooting, full license text and exhaustive contribution guidelines in the README.
- Add images or architecture diagrams only when existing assets are accurate and useful, or when explicitly requested. Do not invent diagrams or broken asset paths.

### 5. Verify before delivery

Check the draft against the repository:

- Each substantive claim and version has an identifiable source.
- Referenced files, examples, workflows and local links exist. Check external links when a supported browsing tool is available; otherwise clearly state they were not checked.
- Commands are grounded in actual project configuration; distinguish static inspection from commands really executed.
- No leaked secrets, fabricated badges, placeholders, stale instructions or unverified features remain.
- Heading order, code fences, link paths and GitHub Flavored Markdown render plausibly.
- Existing useful facts have been retained or intentionally explained as removed.

If execution is permitted and practical, run safe validation commands relevant to the project; report the outcome accurately. Do not claim a build, test or command succeeds unless actually executed and observed.

## Deliverable

- Produce the completed `README.md` in the repository when write access and approval requirements permit; otherwise provide a ready-to-copy Markdown draft.
- Briefly summarize the main changes and the primary evidence used.
- Separately identify material gaps that prevented accurate documentation and any validation that was not performed. Do not insert such caveats as fake content into the README.
- If updating an existing README, avoid changing unrelated project files.

## Final decision rule

A concise README grounded in the actual repository is better than a comprehensive-looking README containing assumptions. When in doubt, omit the unsupported statement.