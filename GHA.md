### The Absolute Basics

  * **Where Workflows Live:**
    Your workflows are `.yml` or `.yaml` files stored in the `.github/workflows/` directory of your repository. 

    ```
    my-repo/
    ├── .github/
    │   └── workflows/
    │       └── my-first-workflow.yml
    └── src/
    └── README.md
    ```

  * **The Workflow Structure:**
    Every workflow needs a name and a trigger.

    ```yaml
    name: My First CI Workflow # A human-readable name for your workflow

    on: # The event that triggers this workflow
      push: # This workflow runs on every 'push' to the repo
        branches:
          - main # Specifically, when changes are pushed to the 'main' branch

    jobs: # Workflows are made of one or more 'jobs'
      build: # This is the ID of our first job (you can name it anything)
        runs-on: ubuntu-latest # The type of runner to use (e.g., Ubuntu, Windows, macOS)

        steps: # Jobs are made of a sequence of 'steps'
          - name: Checkout code # A friendly name for this step
            uses: actions/checkout@v4 # Use a pre-built action to clone your repo

          - name: Say Hello # A simple custom step
            run: echo "Hello, GitHub Actions!" # The command to execute on the runner
    ```

-----

### Key Concepts

  * **`name`**: (Optional) A friendly name for the workflow, shown in the GitHub UI.

  * **`on`**: **Triggers** – Specifies when the workflow should run.

      * `push`, `pull_request`: Most common code-related events.
      * `workflow_dispatch`: Manual trigger (run from GitHub UI).
      * `schedule`: Runs on a cron schedule.
      * `release`: Runs when a release is published.
      * `workflow_call`: Allows one workflow to be called from another (reusable workflows\!).
      * **Filtering:** You can filter by `branches`, `tags`, `paths`, etc.
        ```yaml
        on:
          push:
            branches:
              - main
              - 'feature/**' # glob pattern for feature branches
          pull_request:
            types: [opened, synchronize, reopened] # Specific PR actions
          workflow_dispatch: # Enable manual run from UI
          schedule:
            - cron: '0 0 * * *' # Run daily at midnight UTC
        ```

  * **`jobs`**: The core execution units. Each job runs on a fresh instance of a `runs-on` runner.

      * **Dependency (`needs`):** Jobs can depend on each other, running sequentially.
        ```yaml
        jobs:
          build:
            # ...
          test:
            runs-on: ubuntu-latest
            needs: build # This job will only run after 'build' job completes successfully
            steps:
              # ...
          deploy:
            runs-on: ubuntu-latest
            needs: [test, lint] # Can depend on multiple jobs
            if: github.ref == 'refs/heads/main' # Only deploy if on main branch
            steps:
              # ...
        ```
      * **Concurrency:** Control how many workflow runs or jobs can execute simultaneously.

  * **`runs-on`**: The **Runner Environment** – The type of machine the  job executes on.

      * `ubuntu-latest`, `windows-latest`, `macos-latest` (GitHub-hosted runners)
      * `self-hosted` (Your own machines)

  * **`steps`**: A sequence of tasks within a job. Steps are executed in order.

      * **`name`**: A readable description for the step.
      * **`run`**: Executes a command-line script.
        ```yaml
        - name: Run a Bash command
          run: |
            echo "Building the project..."
            npm install
            npm run build
        ```
      * **`uses`**: Executes a GitHub Action (a reusable piece of code).
          * **Official Actions:** `actions/checkout@v4`, `actions/setup-node@v4`, `actions/upload-artifact@v4`.
          * **Community Actions:** Look for `owner/repo-name@tag` (e.g., `docker/build-push-action@v5`).
          * **Inputs:** Actions often accept inputs using `with:`.
            ```yaml
            - name: Set up Node.js
              uses: actions/setup-node@v4
              with:
                node-version: '20' # Specify Node.js version
                cache: 'npm'       # Cache npm dependencies
            ```

  * **`environment`**: Define an environment for a job, useful for managing secrets and deployments.

    ```yaml
    jobs:
      deploy:
        runs-on: ubuntu-latest
        environment: production # Links to a GitHub environment configured in repo settings
        steps:
          # ...
    ```

  * **`if`**: **Conditional Execution** – Control when a job or step runs.
    Uses Go template-like expressions.

    ```yaml
    - name: Deploy to Production
      if: success() && github.ref == 'refs/heads/main' # Only run if previous steps succeeded AND on main branch
      run: deploy_to_prod.sh
    ```

    Common `if` functions: `success()`, `always()`, `cancelled()`, `failure()`.

  * **`env`**: **Environment Variables** – Define variables for a job or step.

    ```yaml
    jobs:
      build:
        runs-on: ubuntu-latest
        env: # Job-level environment variable
          NODE_ENV: production
        steps:
          - name: Configure database
            run: echo "DB_HOST=${{ secrets.DB_HOST }}" # Accessing a secret
            env: # Step-level environment variable (overrides job-level if same name)
              API_KEY: ${{ secrets.MY_API_KEY }}
    ```

  * **`secrets`**: Securely store sensitive information (API keys, passwords). Never hardcode secrets\!
    Accessed via `${{ secrets.MY_SECRET_NAME }}`. Configure them in the repo settings.

-----

### Common Patterns & Recipes

  * **CI/CD Fundamentals (Build, Test, Deploy):**

    ```yaml
    name: Build, Test & Deploy

    on:
      push:
        branches: [ main ]
      pull_request:
        branches: [ main ]

    jobs:
      build:
        runs-on: ubuntu-latest
        steps:
          - uses: actions/checkout@v4
          - name: Setup Node.js
            uses: actions/setup-node@v4
            with: { node-version: '20' }
          - name: Install dependencies
            run: npm ci
          - name: Build project
            run: npm run build
          - name: Upload build artifacts
            uses: actions/upload-artifact@v4
            with:
              name: my-app-build
              path: dist/ # Path to your built files

      test:
        runs-on: ubuntu-latest
        needs: build # Ensure build artifact is ready
        steps:
          - uses: actions/checkout@v4
          - name: Setup Node.js
            uses: actions/setup-node@v4
            with: { node-version: '20' }
          - name: Download build artifacts
            uses: actions/download-artifact@v4
            with:
              name: my-app-build
              path: dist/
          - name: Run tests
            run: npm test # Or whatever your test command is

      deploy:
        runs-on: ubuntu-latest
        needs: test
        if: github.ref == 'refs/heads/main' # Only deploy from main branch
        environment: production # Link to a protected environment
        steps:
          - name: Deploy to my cloud provider
            run: |
              # Example: Using AWS CLI (configure AWS secrets in environment settings)
              aws s3 sync dist/ s3://my-prod-bucket --delete
              aws cloudfront create-invalidation --distribution-id E123456789 --paths "/*"
    ```

  * **Matrix Jobs (Run on Multiple Configurations):**
    Test code across different OS, language versions, etc.

    ```yaml
    jobs:
      test:
        runs-on: ${{ matrix.os }} # Use a variable from the matrix
        strategy:
          matrix:
            os: [ubuntu-latest, windows-latest]
            node-version: [18, 20] # Test with Node 18 and 20

        steps:
          - uses: actions/checkout@v4
          - name: Setup Node.js ${{ matrix.node-version }}
            uses: actions/setup-node@v4
            with:
              node-version: ${{ matrix.node-version }}
          - name: Install dependencies
            run: npm ci
          - name: Run tests
            run: npm test
    ```

  * **Reusable Workflows (`workflow_call`):**
    Make a workflow reusable by other workflows. Great for standardizing pipelines.

      * `reusable-workflow.yml` (in `.github/workflows/`):
        ```yaml
        name: Reusable Build Step
        on:
          workflow_call:
            inputs:
              node-version:
                required: true
                type: string
            outputs:
              build-id:
                description: "The ID of the build artifact"
                value: ${{ jobs.build.outputs.build-id }}

        jobs:
          build:
            runs-on: ubuntu-latest
            outputs:
              build-id: ${{ steps.upload.outputs.artifact-id }} # Output from the upload action
            steps:
              - uses: actions/checkout@v4
              - uses: actions/setup-node@v4
                with: { node-version: ${{ inputs.node-version }} }
              - run: npm install && npm run build
              - id: upload
                uses: actions/upload-artifact@v4
                with:
                  name: build-output
                  path: ./dist
                  # You might need a way to get artifact ID for output,
                  # this is simplified; usually, you'd use a unique ID.
        ```
      * `caller-workflow.yml` (in `.github/workflows/`):
        ```yaml
        name: My Application CI
        on: [push]
        jobs:
          app-build:
            uses: ./.github/workflows/reusable-workflow.yml # Call the reusable workflow
            with:
              node-version: '20'
            secrets: inherit # Pass all secrets from the caller to the reusable workflow

          deploy-app:
            needs: app-build
            runs-on: ubuntu-latest
            steps:
              - uses: actions/download-artifact@v4
                with:
                  name: build-output # Download the artifact from the reusable workflow
              - run: echo "Deploying build ID: ${{ needs.app-build.outputs.build-id }}"
        ```

  * **Scheduling Workflows:**

    ```yaml
    name: Daily Report Generation

    on:
      schedule:
        # Runs at 00:00 (midnight) UTC every day
        # Learn more about cron syntax: https://crontab.guru/
        - cron: '0 0 * * *'

    jobs:
      generate-report:
        runs-on: ubuntu-latest
        steps:
          - uses: actions/checkout@v4
          - name: Run report script
            run: python generate_report.py
          - name: Upload report
            uses: actions/upload-artifact@v4
            with:
              name: daily-report
              path: report.txt
    ```

-----

### Additional Tips and Tricks

  * **Use `actions/checkout@v4`**: Almost every workflow needs to clone your repo.
  * **Cache Dependencies**: Speed up builds using `actions/cache@v4` (e.g., for `node_modules`, `pip` packages).
  * **Artifacts**: Use `actions/upload-artifact` and `actions/download-artifact` to pass files between jobs.
  * **Secrets, Secrets, Secrets**: NEVER hardcode sensitive data. Use GitHub Secrets.
  * **Read the Logs**: The GitHub Actions UI provides excellent logs for debugging.
  * **Marketplace**: Explore the GitHub Actions Marketplace for pre-built actions.
  * **Self-Hosted Runners**: For specific hardware, networking, or large-scale private needs.
  * **`on: workflow_dispatch`**: Add this to workflows you want to run manually from the UI.
  * **`concurrency`**: Use to manage parallel runs of workflows or jobs to prevent race conditions or resource exhaustion.