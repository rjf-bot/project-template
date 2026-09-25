# Rules — FastAPI Structure

The disclosed reference for [`fastapi-structure`](SKILL.md). Read in full before writing or reviewing any FastAPI file.

---

## Project Layout

```text
app/
├── main.py               # initialization only
├── api/
│   └── v1/
│       ├── router.py     # aggregates sub-routers for this version
│       └── endpoints/
│           └── items.py  # one file per resource
├── services/
│   └── item_service.py   # one file per domain concept
├── schemas/
│   ├── request/
│   │   └── item_request.py
│   └── response/
│       └── item_response.py
├── models/
│   └── item.py           # ORM or domain model
├── dependencies/
│   └── auth.py           # FastAPI Depends() callables
└── core/
    └── settings.py       # Pydantic BaseSettings
```

One resource = one router file + one service file + one request schema + one response schema.

---

## Layer Rules

### main.py

- Contains only: `FastAPI()` instantiation, `app.include_router()` calls, lifespan/startup/shutdown hooks, and global middleware.
- No business logic, no direct DB calls, no inline route handlers.

```python
# correct
from fastapi import FastAPI
from app.api.v1.router import router as v1_router

app = FastAPI(title="My API")
app.include_router(v1_router, prefix="/v1")
```

### Routers

- One `APIRouter` per resource file. Prefix and tags declared at `APIRouter()` construction, not at `include_router()`.
- Route handlers are thin: validate input (via Pydantic schema), call one service function, return the response schema. No business logic in route handlers.
- All path parameters and query parameters are explicitly typed.
- Return type annotated with the response schema.

```python
# correct
router = APIRouter(prefix="/items", tags=["items"])

@router.post("/", response_model=ItemResponse, status_code=status.HTTP_201_CREATED)
async def create_item(payload: ItemCreateRequest, svc: ItemService = Depends(get_item_service)) -> ItemResponse:
    return await svc.create(payload)
```

```python
# wrong — business logic leaking into router
@router.post("/")
async def create_item(payload: ItemCreateRequest, db: Session = Depends(get_db)):
    item = Item(**payload.dict())
    db.add(item)
    db.commit()
    return item
```

### Services

- Plain Python classes or modules — no FastAPI imports.
- Receive typed inputs (Pydantic schemas or primitives), return typed outputs (Pydantic schemas or domain objects).
- Own all business logic: validation beyond schema, orchestration, error handling with domain-specific exceptions.
- Injected into routers via `Depends()` — never instantiated inside route handlers.

```python
# correct
class ItemService:
    def __init__(self, repo: ItemRepository) -> None:
        self._repo = repo

    async def create(self, payload: ItemCreateRequest) -> ItemResponse:
        if await self._repo.exists(payload.name):
            raise ItemAlreadyExistsError(payload.name)
        item = await self._repo.save(Item.from_request(payload))
        return ItemResponse.from_orm(item)
```

### Schemas

- Separate request and response schemas — never reuse the same class for both.
- All fields explicitly typed; `Optional` fields carry a default value.
- Use `model_config = ConfigDict(from_attributes=True)` (Pydantic v2) for response schemas that map from ORM models.
- No business logic inside schemas — validators (`@field_validator`) handle format/structural checks only.
- Schemas live under `schemas/request/` or `schemas/response/` — never under `models/` or `api/`.

```python
# correct
class ItemCreateRequest(BaseModel):
    name: str
    quantity: int = Field(gt=0)

class ItemResponse(BaseModel):
    id: int
    name: str
    quantity: int
    model_config = ConfigDict(from_attributes=True)
```

### Models

- Domain or ORM models in `models/` — no Pydantic schemas, no FastAPI imports.
- Factory class methods (`from_request`, `from_dict`) keep construction logic out of route handlers and services.

### Dependencies

- Every `Depends()` callable lives in `dependencies/` — not inlined in route handlers.
- Dependencies return a fully constructed object (service, repository, current user) — not a raw DB session unless the dependency is specifically a DB session provider.
- Typed return annotation required.

```python
# correct
def get_item_service(db: Session = Depends(get_db)) -> ItemService:
    return ItemService(ItemRepository(db))
```

### Settings

- All environment variables declared in `core/settings.py` as a Pydantic `BaseSettings` subclass.
- No `os.environ` or `os.getenv` calls outside `settings.py`.
- Instantiated once; imported as a module-level singleton.

```python
# correct
from app.core.settings import settings

DATABASE_URL = settings.database_url
```

---

## Smells

Run these checks in order for every file under review. Record each hit as `file:line — rule`.

| # | Smell | Rule violated |
| --- | ------- | -------------- |
| 1 | Business logic in a route handler (DB calls, conditionals, loops) | Routers — thin handlers |
| 2 | `APIRouter` defined but prefix/tags set at `include_router()` | Routers — prefix at construction |
| 3 | Same Pydantic class used for request and response | Schemas — separate schemas |
| 4 | `os.environ` or `os.getenv` outside `settings.py` | Settings — single source |
| 5 | `Depends()` callable defined inline in a route function | Dependencies — live in `dependencies/` |
| 6 | FastAPI import inside a service or model file | Services — no FastAPI imports |
| 7 | Business logic in a `@field_validator` (DB calls, external I/O) | Schemas — validators are structural |
| 8 | `main.py` contains route handlers or business logic | main.py — initialization only |
| 9 | Response schema missing `from_attributes=True` when mapping from ORM | Schemas — ORM mapping |
| 10 | Service instantiated with `ServiceClass()` inside a route handler | Services — injected via Depends |
