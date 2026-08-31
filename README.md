# badge-action-sample

![badge](badge/badge.svg)

---

Any commit to the Repo triggers the workflow below, it requests and saves a `badge.svg` file, which this README.md file references.
>Note: See the [badges.yml](.github/workflows/badges.yml) workflow for details.

```mermaid
sequenceDiagram
    autonumber
    actor Dev as Developer
    participant Repo as GitHub Repo
    participant Readme as README.md
    participant GHA as GitHub Action
    participant Akamai as Akamai (Edge/Proxy)
    participant Backend as Backend Service

    Dev->>Repo: Push commit / open PR
    Repo->>GHA: Trigger workflow (on: push, pull_request)
    GHA->>GHA: Read Action credentials (secrets)
    GHA->>Akamai: POST /badge (payload + credentials)
    Akamai->>Backend: Forward POST request
    Backend-->>Akamai: 200 OK + Badge (SVG)
    Akamai-->>GHA: Badge (SVG)
    GHA->>Repo: Commit status.svg
    Readme->>Repo: Reference ![status](status.svg)
    Repo-->>Dev: README renders badge
```
