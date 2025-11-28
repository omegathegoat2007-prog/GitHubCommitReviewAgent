# GitHubCommitReviewAgent

This repository is a small test project whose purpose is to validate webhooks and ensure an automated agent can receive pull request events and review code.

## Purpose

- Test receiving GitHub webhook events (especially `pull_request` and `pull_request_review`).
- Validate that an automated agent can process PR webhooks and perform automated code reviews or comments.

## Features

- Listener for GitHub webhook events
- Basic handling for PR opened/edited/synchronized events
- Hooks for running automated code review logic (e.g., linting, static analysis)

## Quickstart

1. Configure a webhook on your GitHub repository:
   - Payload URL: http://<your-agent-host>/webhook
   - Content type: application/json
   - Events: Let me select `Pull requests` (or select "Let me select individual events" and pick `pull_request` and `pull_request_review`).
   - Secret: (optional) set a secret and configure the agent to validate it.

2. Run the agent locally or on a server:
   - Ensure the agent is reachable from GitHub. For local testing, use a tunnel such as ngrok:
     - ngrok http 3000
     - Use the HTTPS forwarding URL as the Payload URL in the webhook configuration.

3. Create a pull request in the repository. The webhook should trigger and the agent should receive the `pull_request` event. The agent can then run whatever review checks you implement and post comments or review results.

## Example: Minimal webhook handler (pseudocode)

- On `pull_request` event:
  - Validate signature (if secret configured)
  - Parse PR number, head/ref, and changed files
  - Run static checks or call your review agent
  - Post comments or reviews via GitHub API

## Useful tips

- Test with `pull_request` events: open a PR, push new commits to the branch (which triggers `synchronize`), edit PR title/body, or close/reopen the PR.
- For review automation, consider using GitHub Checks API or create review comments on the PR via the REST API.
- If your agent needs to run CI-like checks, run them in an isolated environment and upload results to checks or PR comments.

## Example webhook events to watch

- `pull_request` (opened, reopened, synchronize, edited)
- `pull_request_review` (submitted, edited)
- `pull_request_review_comment` (created, edited)

## Development

- Add your review logic in `src/` (or similar).
- Add tests that simulate webhook payloads and assert the agent produces expected comments/reviews.

## Security

- Verify webhook signatures when a secret is used.
- Restrict actions your agent can perform via a GitHub App or a fine-scoped token.

## License

This project is for testing purposes. Add a license file if you want to open-source this code.

## Contact

If you have questions about configuring webhooks or the agent, add an issue or reach out to the repository owner.
