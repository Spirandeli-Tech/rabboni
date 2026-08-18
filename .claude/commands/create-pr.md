Push the current branch and open a Pull Request for the ticket given as argument.

Steps:
1. Confirm the working tree is committed (`git status`); if there are uncommitted
   changes related to the ticket, commit them with a clear message first.
2. Push the current branch to origin (`git push -u origin HEAD`).
3. Open the PR into `main` with `gh pr create` — title `[<TICKET>] <short summary
   of the delivery>`, body describing what was done, choices made, and a link to
   the Jira ticket (https://spirandeli.atlassian.net/browse/<TICKET>).
4. Print the PR URL on its own line at the end (the runner extracts it).

This repo is worked by autonomous agents: never end asking a question — act on
the recommended path and record choices in the PR body.
