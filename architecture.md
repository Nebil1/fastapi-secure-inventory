app/
│
├── main.py
│   ├─ Entry point of your FastAPI app
│   ├─ Creates FastAPI instance
│   ├─ Includes routers
│   └─ Wires up dependencies (composition root)
│
├── routers/                     # ← Presentation layer (API endpoints)
│   ├── auth.py
│   ├── items.py
│   └── sec_events.py
│   # These handle HTTP requests and responses only.
│   # They call use-cases, not the database directly.
│
├── schemas.py                   # ← DTOs / Pydantic models
│   # Defines request/response validation and serialization.
│   # Used at API boundaries between Routers and Use-Cases.
│
├── deps.py                      # ← Dependency injection wiring
│   # Provides reusable FastAPI Depends() for DB, repos, auth, pagination.
│   # Example: get_db(), get_current_user(), get_item_repo().
│
├── core/                        # ← Application core (business logic)
│   ├── __init__.py
│   ├── entities.py              # (optional) Domain entities / value objects
│   ├── interfaces.py            # Repository & service interfaces (ports)
│   └── use_cases/               # Application logic (use-cases)
│       ├── create_item.py
│       ├── list_items.py
│       ├── log_security_event.py
│       └── __init__.py
│   # No FastAPI, SQLModel, or JWT imports here!
│   # Pure Python logic and contracts.
│
├── infrastructure/              # ← Adapters (implementations of ports)
│   ├── __init__.py
│   ├── persistence/
│   │   ├── db.py                # Database engine + session setup
│   │   ├── models.py            # SQLModel ORM tables
│   │   └── repositories/
│   │       ├── item_repo.py     # Implements ItemRepository interface
│   │       ├── sec_event_repo.py# Implements SecEventRepository
│   │       └── user_repo.py     # Implements UserRepository
│   │
│   └── security/
│       ├── jwt_service.py       # JWT encode/decode adapter
│       └── password_service.py  # Password hashing/verify adapter
│
├── core/config.py               # ← Configuration / environment variables
│
├── utils/                       # ← Helper utilities
│   ├── pagination.py
│   └── __init__.py
│
└── __init__.py


routers  ───►  core.use_cases  ───►  core.interfaces  ◄─── infrastructure
       (API)        (Logic)              (Contracts)         (Adapters)
