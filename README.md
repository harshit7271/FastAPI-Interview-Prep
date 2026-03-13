# **FastAPI Interview Preparation for AI/ML Engineers**
## 1. What is FastAPI and why is it used in ML systems?
**ANSWER:**

FastAPI is a modern Python web framework used to build APIs with high performance. It is built on top of Starlette (for web handling) and Pydantic (for data validation).

Key reasons ML engineers use FastAPI:

- Very fast (comparable to NodeJS/Go)

- Automatic data validation using type hints

- Auto-generated API docs (Swagger UI & ReDoc)

- Async support

- Easy model serving

Typical ML workflow:
`Train Model → Save Model → Load in FastAPI → Create /predict endpoint`

Example use case:

- Deploying a sentiment analysis model

- Serving computer vision inference

- Hosting LLM inference APIs

---
## 2. Difference between FastAPI, Flask, and Django

  | Feature         | FastAPI             | Flask       | Django           |
| --------------- | ------------------- | ----------- | ---------------- |
| Performance     | Very High           | Medium      | Medium           |
| Async Support   | Native              | Limited     | Added later      |
| Data Validation | Built-in (Pydantic) | Manual      | Forms/Serializer |
| Auto Docs       | Yes                 | No          | No               |
| Best Use        | ML APIs             | Simple APIs | Full web apps    |

Interview Tip

For ML serving:

``FastAPI is preferred because it provides automatic validation, async support, and higher throughput.``

---

## 3. What is Pydantic in FastAPI?

**Answer:**

Pydantic is used for data validation and serialization using Python type hints.

It ensures:

- Correct input format

- Automatic error messages

- Clean request parsing

**Example:**

```python
from pydantic import BaseModel

class InputData(BaseModel):
    age: int
    salary: float
```

If client sends:

``{"age" : "twenty"}``

**FastAPI automatically returns a validation error.**

---

## 4. How do you serve a Machine Learning model using FastAPI?
