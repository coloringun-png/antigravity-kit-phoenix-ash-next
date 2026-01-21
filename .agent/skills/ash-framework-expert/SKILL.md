---
name: ash-framework-expert
description: Ash Framework expertise. Declarative resources, policies, and actions.
allowed-tools: Read, Write, Edit, Glob, Grep
---

# Ash Framework Expert

Ash Framework is a framework used to build declarative and highly customizable applications in Elixir.

## 1. Declarative Resources (Resources)

The heart of an Ash application is its resources. A resource defines the data (Attributes), relationships (Relationships), actions (Actions), and authorization (Policies).

### Example Resource Structure

```elixir
defmodule MyApp.Accounts.User do
  use Ash.Resource,
    domain: MyApp.Accounts,
    data_layer: AshPostgres.DataLayer

  attributes do
    uuid_primary_key :id
    attribute :email, :ci_string, allow_nil?: false
    attribute :name, :string
  end

  actions do
    defaults [:read, :destroy]

    create :register do
      accept [:email, :name]
    end
  end

  policies do
    policy action_type(:read) do
      authorize_if always()
    end
  end
end
```

## 2. Actions

Actions define how data mutations occur.

- **Create**: Create new record.
- **Read**: Read data including filtering and sorting.
- **Update**: Modify existing record.
- **Destroy**: Delete record.

## 3. Authorization (Policies)

Ash uses an "Actor" based authorization system. Who (Actor) can access which resource (Resource) with what action (Action) is defined in the `policies` block.

## 4. API Integration

- **AshJsonApi**: Automatically converts resources into JSON:API compliant endpoints.
- **AshGraphql**: Automatically generates GraphQL schema from resources.

## 5. Calculations & Aggregates

- **Calculations**: Dynamic values derived from existing data (e.g. `full_name`).
- **Aggregates**: Computations over related data (e.g. `comment_count`).

---

> **Principle:** "Don't repeat yourself." Always define business logic at the Resource level, ensuring consistency across all APIs (JSON, GraphQL, LiveView).
