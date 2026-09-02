You are working as a senior Python/backend engineer and tooling architect.

Your task is to study my existing local project and create a complete Cookiecutter boilerplate based on it, so that I can quickly generate NEW projects with the same architecture, technology stack, engineering practices, and infrastructure.

The source project is located here:

<ENTER_PROJECT_PATH_HERE>

Important: the source project is a real working project. You must not simply copy the entire project into a template. You need to extract the universal architectural and infrastructure foundation from it and turn that into a clean, reusable boilerplate.

## Main goal

In the future, I want to be able to run something like:

```bash
cookiecutter /path/to/my-cookiecutter-template
```

or:

```bash
cookiecutter https://github.com/.../my-template
```

answer a few questions, and get a new fully working project with the same architecture and stack as the current project.

The newly generated project should look as if it was originally built from scratch according to these architectural rules, not copied from an existing product.

---

# Stage 1. Fully analyze the source project first

Before making any changes, perform a complete audit of the project.

Study:

* directory structure;
* architectural layers;
* Python packages/modules;
* application entry point;
* FastAPI configuration, if FastAPI is used;
* dependency injection;
* database layer;
* repositories;
* Unit of Work;
* entities/models;
* DTOs / schemas;
* services;
* use cases;
* domain layer;
* application layer;
* infrastructure layer;
* presentation/API layer;
* configuration;
* settings;
* environment variables;
* `.env.example`, if present;
* migrations;
* Alembic;
* SQLAlchemy;
* Redis/cache;
* message brokers;
* Celery / Dramatiq / FastStream / Kafka / RabbitMQ / NATS, etc., if used;
* Dockerfile;
* docker-compose;
* Makefile;
* pyproject.toml;
* dependency manager;
* Ruff;
* MyPy;
* Pyright;
* pytest;
* pre-commit;
* CI/CD;
* GitHub Actions;
* logging;
* observability;
* metrics;
* tracing;
* health checks;
* scripts;
* CLI;
* test infrastructure;
* fixtures;
* factories;
* lint/typecheck/test commands;
* development workflow;
* production/development configuration.

Also identify all architectural conventions used in the project.

For example:

* where a new use case should be placed;
* where a repository interface should be created;
* where repository implementations live;
* how a new dependency is registered;
* how DI is organized;
* how a new table is added;
* how migrations are created;
* how a new API endpoint is added;
* how DTOs are organized;
* how exceptions are structured;
* where business logic belongs.

Do not make assumptions when the answer can be determined from the existing codebase.

---

# Stage 2. Separate boilerplate from product-specific code

This is a critical part of the task.

Do not move everything into the Cookiecutter template blindly.

Split the project contents into two categories:

## A. Universal foundation

This should be included in the boilerplate.

For example:

* architecture;
* directory structure;
* base classes;
* abstractions;
* dependency injection setup;
* database initialization;
* Unit of Work;
* generic repositories;
* application bootstrap;
* middleware;
* error handling;
* logging setup;
* configuration system;
* Docker;
* docker-compose;
* Makefile;
* Alembic setup;
* linting;
* formatting;
* typing;
* test infrastructure;
* CI;
* pre-commit;
* health checks;
* common utilities;
* reusable base exceptions;
* startup/shutdown lifecycle;
* project conventions.

## B. Current product-specific code

This should NOT be copied automatically.

For example:

* real users;
* current business entities;
* product-specific use cases;
* company names;
* product names;
* real domains;
* real API endpoints;
* production URLs;
* customer data;
* product-specific integrations;
* API keys;
* tokens;
* credentials;
* passwords;
* `.env`;
* secrets;
* production database configuration;
* real queue/topic names;
* real bucket names;
* real email addresses;
* real service URLs;
* database dumps;
* local data;
* logs;
* caches;
* `.venv`;
* IDE configuration;
* temporary files;
* generated files;
* build artifacts;
* `__pycache__`;
* `.pytest_cache`;
* `.mypy_cache`;
* `.ruff_cache`;
* `.coverage`;
* volumes;
* node_modules, if present;
* any artifacts specific to current development.

If it is unclear whether a file belongs in the reusable boilerplate, analyze its purpose first.

The main principle is:

> The new project should inherit the architecture and engineering decisions of the source project, but not its business logic or data.

---

# Stage 3. Define Cookiecutter parameters

Create a well-designed `cookiecutter.json`.

At minimum, parameterize:

```text
project_name
project_slug
package_name
project_description
author_name
author_email
python_version
```

If the project stack makes it reasonable, add optional parameters such as:

```text
use_postgresql
use_redis
use_docker
use_tests
use_pre_commit
use_github_actions
```

Do not create meaningless options just to make the template more configurable.

If a technology is a fundamental part of the project architecture, it is better to keep it mandatory than to turn everything into an optional configuration matrix.

Values should be used throughout the template using Cookiecutter/Jinja:

```text
{{ cookiecutter.project_name }}
{{ cookiecutter.project_slug }}
{{ cookiecutter.package_name }}
```

There must be no hardcoded references to the current project name.

---

# Stage 4. Create the Cookiecutter template

Create a separate template directory.

The desired structure should look approximately like this:

```text
cookiecutter-template/
├── cookiecutter.json
├── hooks/
│   ├── pre_gen_project.py
│   └── post_gen_project.py
├── {{cookiecutter.project_slug}}/
│   ├── ...
│   ├── src/
│   ├── tests/
│   ├── pyproject.toml
│   ├── Dockerfile
│   ├── docker-compose.yml
│   ├── Makefile
│   ├── README.md
│   ├── .gitignore
│   └── .env.example
└── README.md
```

Adapt the exact structure to match my project.

---

# Stage 5. Make sure the generated project is actually a new project

After generation, there must be no traces of the original product.

Explicitly check that none of the following remain:

* old project name;
* old package name;
* old Docker container names;
* old database names;
* old repository names;
* old service names;
* old domains;
* old URLs;
* old emails;
* production credentials;
* secrets;
* business-specific constants;
* business entities that are not intentional examples;
* internal names from the current product.

Perform a global search across the template.

If the source project was named, for example:

```text
super_backend
```

then after generating a project called `example_api`, the string `super_backend` should not appear anywhere unless it is actually the name of an external dependency/package.

---

# Stage 6. Remove product-specific business logic

If the source project contains a large amount of business logic, do not copy all of it.

If useful, keep only a minimal demonstration vertical slice that shows the correct way to work with the architecture.

For example:

```text
ExampleEntity
ExampleRepository
CreateExampleUseCase
ExampleDTO
ExampleAPIEndpoint
```

However, if an example would add unnecessary noise, prefer keeping architectural abstractions without artificial business logic.

Choose the approach that best matches the architecture of the source project.

---

# Stage 7. Environment variables and secrets

Never copy the real `.env`.

Create a safe:

```text
.env.example
```

with placeholder values.

For example:

```env
APP_ENV=local
POSTGRES_DB={{cookiecutter.project_slug}}
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres
SECRET_KEY=change-me
```

Use safe placeholders for production secrets.

There is no need to inspect Git history. Work only with the current project contents unless otherwise required.

---

# Stage 8. Hooks

Use Cookiecutter hooks only where they provide real value.

For example, `post_gen_project.py` may:

* remove optional files;
* create `.env` from `.env.example`;
* rename certain entities;
* remove CI configuration if disabled;
* remove Docker configuration if disabled;
* perform additional cleanup.

Hooks must not perform dangerous operations outside the generated project directory.

Do not automatically run destructive commands.

---

# Stage 9. README for the generated project

The generated project must have its own README containing the new project name.

The README should include at least:

1. Project purpose.
2. Architecture.
3. Directory structure.
4. Requirements.
5. Installation.
6. How to run the development environment.
7. How to run with Docker.
8. Migrations.
9. Tests.
10. Linting.
11. Type checking.
12. Pre-commit.
13. Main Makefile/CLI commands.
14. How to add a new API endpoint.
15. How to add a new entity/model.
16. How to add a repository.
17. How to add a use case.
18. How to create a migration.

Do not blindly copy the product-specific README from the source project.

---

# Stage 10. README for the boilerplate itself

The Cookiecutter template itself must also have a README.

Explain:

* what the boilerplate generates;
* requirements;
* how to install Cookiecutter;
* example usage;
* the list of Cookiecutter questions;
* an example of non-interactive generation;
* which technologies are included in the template;
* what needs to be done after generation.

For example:

```bash
cookiecutter .
```

and:

```bash
cookiecutter . --no-input \
  project_name="Example API" \
  project_slug="example-api" \
  package_name="example_api"
```

---

# Stage 11. Validate the template

Do not consider the task complete after simply creating the files.

You must test the boilerplate.

Generate a temporary project using Cookiecutter, for example:

```text
project_name = Test Project
project_slug = test-project
package_name = test_project
```

After generation:

1. Verify the directory structure.
2. Search for leftovers from the original project name.
3. Check for unresolved Jinja placeholders.
4. Validate Python imports.
5. Validate `pyproject.toml`.
6. Validate Docker configuration.
7. Validate environment variables.
8. Validate Alembic configuration.
9. Install dependencies if possible in the current environment.
10. Run linting.
11. Run type checking.
12. Run tests.
13. Try importing the application.
14. If possible, start the application.
15. If Docker Compose is used and the environment allows it, validate the configuration with:

```bash
docker compose config
```

Do not leave the temporary generated project inside the boilerplate if it was created only for validation.

---

# Stage 12. Do not break the source project

Use the source project as a reference.

Do not perform major refactoring inside the source project just to create the boilerplate.

All main changes should be made inside the new Cookiecutter template directory.

---

# Stage 13. Work autonomously

Do not ask me for confirmation for every small decision.

If the architecture of the project makes the correct approach clear, make the decision yourself.

If you discover inconsistencies, technical debt, or questionable historical decisions in the source project, you do not have to preserve them in the boilerplate simply because they exist.

Use this principle:

> Preserve intentional architecture, not accidental complexity.

In other words, preserve deliberate architectural decisions, but do not copy bugs, hacks, temporary workarounds, obsolete code, or historical clutter.

---

# Stage 14. Before creating files, provide a brief analysis

Before implementation, show me a short summary containing:

```text
1. Detected technology stack
2. Project architecture
3. What will be included in the boilerplate
4. What will be excluded
5. Proposed Cookiecutter parameters
6. Proposed template structure
```

After that, immediately proceed with implementation. You do not need additional confirmation from me.

---

# Stage 15. Final report

After completing the task, show:

```text
Created:
- ...

Excluded from original project:
- ...

Cookiecutter variables:
- ...

Validation performed:
- ...

Commands tested:
- ...

Known limitations:
- ...
```

Also provide the exact command I can use to generate a new project.

For example:

```bash
cookiecutter /path/to/cookiecutter-template
```

Also provide an example of fully non-interactive generation.

---

# Success criteria

The task is considered complete only if this workflow works:

```text
existing working project
        ↓
architecture analysis
        ↓
removal of project-specific parts
        ↓
parameterization
        ↓
Cookiecutter template
        ↓
cookiecutter ...
        ↓
new clean project
        ↓
install / lint / tests pass
```

I do not want an archive of the current project with the project name replaced.

I want a real reusable project generator.

Start by analyzing the project located at:

<ENTER_PROJECT_PATH_HERE>
