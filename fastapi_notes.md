# FastAPI Notes
## 1. Setup
- Installed FastAPI and Uvicorn
- Created `main.py` with `FastAPI()`
- Mounted static files:
  ```python
  app.mount("/static", StaticFiles(directory="static"), name="static")
  ```

- Configured Jinja2 templates
  ```python
  templates = Jinja2Templates(directory="Templates")
  ```

## 2. Routing
- Basic routes with @app.get("/")
- Example returning template:
```python
return templates.TemplateResponse(
    request,
    "home.html",
    {"posts": posts, "title": "Home"}
)
```

## 3. Templates
- Template inheritance using layout.html
- Variable substitution with {{ variable }}
- Example: {{ post.title }} inside home.html

## 4. Path Parameters
- Example
```python
@app.get("/api/posts/{post_id}")
def get_post(post_id: int):
    for post in posts:
        if post.get("id") == post_id:
            return post
    raise HTTPException(
        status_code=status.HTTP_404_NOT_FOUND,
        detail=f"Post with id {post_id} not found"
    )
```
- Used for fetching single resources by ID.


## 5. Error Handling
- HTTPException for custom errors (e.g., 404 Not Found).
- Validation Errors (422) triggered automatically when input type mismatches.
- Custom exception handler:
```python
@app.exception_handler(StarletteHTTPException)
async def custom_http_exception_handler(request, exc):
    return templates.TemplateResponse(
        "error.html",
        {"request": request, "detail": exc.detail},
        status_code=exc.status_code
    )
```

## 6. Validation
- Example: /posts/hello → returns 422 error with JSON detail.
- Difference:
- Validation errors → automatic, type mismatch.
- HTTP exceptions → manual, resource not found.




