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

**Answer:**

Steps:

1. Train model
2. Save model
3. Load model in API
4. Create prediction endpoint

**Example:**

```python

from fastapi import FastAPI
import joblib
import numpy as np

app = FastAPI()

model = joblib.load("model.pkl")

@app.post("/predict")
def predict(data: list):
    
    prediction = model.predict([data])
    
    return {"prediction": prediction.tolist()}
```

---

## 5. What is Async in FastAPI and why is it useful?

**Answer**

Async allows FastAPI to handle multiple requests simultaneously without blocking execution.

**Example:**

```python
@app.get("/data")
async def get_data():
    return {"message": "Hello"}
```

### Why important for ML:

- Many users sending requests
- Avoid blocking during IO operations
- Better scalability

However:

``ML inference itself is usually CPU/GPU bound, so async mainly helps for I/O operations.``

---

## 6. What is dependency injection in FastAPI?

FastAPI allows reusable components like:

- authentication
- database session
- model loading

**Example:**

```python
from fastapi import Depends

def get_model():
    return model

@app.post("/predict")
def predict(data: list, model=Depends(get_model)):
    
    pred = model.predict([data])
    
    return {"prediction": pred.tolist()}
```

---
## 7. How does FastAPI automatically generate documentation?

FastAPI uses OpenAPI specification.

Auto docs available at:
```
/docs       → Swagger UI
/redoc      → ReDoc
```

**Benefits:**

- Easy testing
- Clear API schema
- Helps frontend teams

---
## 8. How do you handle file uploads (important for CV engineers)?

Example: image inference API.

```python
from fastapi import UploadFile, File

@app.post("/predict-image")
async def predict_image(file: UploadFile = File(...)):

    contents = await file.read()
    
    return {"filename": file.filename}
```
**Used for:***

- Image classification
- OCR
- Face recognition
- Object detection
---
## 9. How do you deploy FastAPI?

FastAPI runs using ASGI servers.

Common deployment:
```
uvicorn main:app --reload
```

Production:

- Uvicorn + Gunicorn
- Docker
- Kubernetes
- AWS / GCP / Azure

Example:
`` gunicorn -k uvicorn.workers.UvicornWorker main:app ``

---
## 10. How do you optimize FastAPI for ML inference?

Good interview answer includes:
**1. Model loaded once at startup**
``
@app.on_event("startup")
def load_model():
``
2. Batch predictions
3. Use async for I/O
4. Use caching
5. GPU inference
6. Use background tasks

Example:

```python
from fastapi import BackgroundTasks
```

---
## 11. What is middleware in FastAPI?
Middleware processes request before and after execution.

**Example uses:**

- logging
- authentication
- request timing

Example:
```python
@app.middleware("http")
async def log_requests(request, call_next):

    response = await call_next(request)

    return response
```
---

## **Practical FastAPI Questions (Very Likely in AI/ML Interviews)**
Create a FastAPI API that loads a trained sklearn model and predicts house prices.

Solution
```python
from fastapi import FastAPI
from pydantic import BaseModel
import joblib
import numpy as np

app = FastAPI()

model = joblib.load("house_model.pkl")

class HouseFeatures(BaseModel):
    area: float
    bedrooms: int
    bathrooms: int

@app.post("/predict")
def predict_price(features: HouseFeatures):

    data = np.array([[features.area,
                      features.bedrooms,
                      features.bathrooms]])

    prediction = model.predict(data)

    return {
        "predicted_price": float(prediction[0])
    }
```
Example request:

```bash
POST /predict

{
 "area":1200,
 "bedrooms":3,
 "bathrooms":2
}
```

---
# **Practical Question 2**

Build a FastAPI endpoint that accepts an image and runs inference using a PyTorch model.

Solution
```python
from fastapi import FastAPI, UploadFile, File
import torch
from PIL import Image
import io
import torchvision.transforms as transforms

app = FastAPI()

model = torch.load("model.pth")
model.eval()

transform = transforms.Compose([
    transforms.Resize((224,224)),
    transforms.ToTensor()
])

@app.post("/predict-image")

async def predict_image(file: UploadFile = File(...)):

    contents = await file.read()

    image = Image.open(io.BytesIO(contents))

    tensor = transform(image).unsqueeze(0)

    with torch.no_grad():
        output = model(tensor)

    prediction = torch.argmax(output).item()

    return {
        "class": prediction
    }
```

---
# Very Smart Answer (Use in Interviews)

When they ask:

**"Why do ML engineers use FastAPI?"**

Answer like this:

``FastAPI is widely used for ML deployment because it supports async execution, automatic validation using Pydantic, high performance through ASGI servers, and auto-generated API documentation. This makes it ideal for exposing machine learning models as production-ready REST APIs.``

---
# Smart Interview Answer

***If an interviewer asks "What is async support in FastAPI?", answer like this:***

`Async support in FastAPI allows the server to handle multiple requests concurrently using Python's async and await syntax. It prevents blocking during I/O operations like database queries or external API calls, which improves scalability and performance when serving many users.`






