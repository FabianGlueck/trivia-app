# Backend - Trivia API

## Setting up the Backend

### Install Dependencies

1. **Python 3.7** - Follow instructions to install the latest version of python for your platform in the [python docs](https://docs.python.org/3/using/unix.html#getting-and-installing-the-latest-version-of-python)

2. **Virtual Environment** - We recommend working within a virtual environment whenever using Python for projects. This keeps your dependencies for each project separate and organized. Instructions for setting up a virual environment for your platform can be found in the [python docs](https://packaging.python.org/guides/installing-using-pip-and-virtual-environments/)

3. **PIP Dependencies** - Once your virtual environment is setup and running, install the required dependencies by navigating to the `/backend` directory and running:

```bash
pip install -r requirements.txt
```

#### Key Pip Dependencies

- [Flask](http://flask.pocoo.org/) is a lightweight backend microservices framework. Flask is required to handle requests and responses.

- [SQLAlchemy](https://www.sqlalchemy.org/) is the Python SQL toolkit and ORM we'll use to handle the lightweight SQL database. You'll primarily work in `app.py`and can reference `models.py`.

- [Flask-CORS](https://flask-cors.readthedocs.io/en/latest/#) is the extension we'll use to handle cross-origin requests from our frontend server.

### Set up the Database

With Postgres running, create a `trivia` database:

```bash
createdb trivia
```

Populate the database using the `trivia.psql` file provided. From the `backend` folder in terminal run:

```bash
psql trivia < trivia.psql
```

### Run the Server

From within the `./src` directory first ensure you are working using your created virtual environment.

To run the server, execute:

```bash
flask run --reload
```

The `--reload` flag will detect file changes and restart the server automatically.

## To Do Tasks

These are the files you'd want to edit in the backend:

1. `backend/flaskr/__init__.py`
2. `backend/test_flaskr.py`

One note before you delve into your tasks: for each endpoint, you are expected to define the endpoint and response data. The frontend will be a plentiful resource because it is set up to expect certain endpoints and response data formats already. You should feel free to specify endpoints in your own way; if you do so, make sure to update the frontend or you will get some unexpected behavior.

1. Use Flask-CORS to enable cross-domain requests and set response headers.
2. Create an endpoint to handle `GET` requests for questions, including pagination (every 10 questions). This endpoint should return a list of questions, number of total questions, current category, categories.
3. Create an endpoint to handle `GET` requests for all available categories.
4. Create an endpoint to `DELETE` a question using a question `ID`.
5. Create an endpoint to `POST` a new question, which will require the question and answer text, category, and difficulty score.
6. Create a `POST` endpoint to get questions based on category.
7. Create a `POST` endpoint to get questions based on a search term. It should return any questions for whom the search term is a substring of the question.
8. Create a `POST` endpoint to get questions to play the quiz. This endpoint should take a category and previous question parameters and return a random questions within the given category, if provided, and that is not one of the previous questions.
9. Create error handlers for all expected errors including 400, 404, 422, and 500.

## Documentation of Endpoints

### GET /categories

**Description**
This endpoint retrieves all categories from the database.

**Parameters**
No parameters are required for this endpoint.

**Response**
A JSON object containing:

`success`: (boolean) Indicates whether the request was successful.
`categories`: (object) An object where each key is a category ID and the `corresponding` value is the category type.

**Error Handling**
If there are no categories in the database, the server will respond with a 404 status code.

**Example Request**
`GET /categories`

**Example Response**

```
{
    "success": True,
    "categories": {
        "1": "Science",
        "2": "Art",
        // More categories...
    }
}
```

### GET /questions

**Description**
This endpoint retrieves a paginated list of questions and all categories.

**Parameters**
page: (optional, integer) Specifies the page of questions to retrieve. Each page contains 10 questions. If not provided, defaults to 1.

**Response**
A JSON object containing:

`success`: (boolean) Indicates whether the request was successful.
`questions`: (array) A list of questions for the requested page. Each question is represented as a formatted object.
`total_questions`: (integer) The total number of questions available.
`categories`: (object) An object where each key is a category ID and the corresponding value is the category type.
`current_category`: (null) This field is always null in the current implementation.

**Error Handling**
If there are no questions for the requested page, the server will respond with a 404 status code.

**Example Request**
`GET /questions?page=2`

**Example Response**

```
 {
    "success": True,
    "questions": [
        {
            "id": 11,
            "question": "What is the capital of Australia?",
            "answer": "Canberra",
            "difficulty": 3,
            "category": 6
        },
        // More questions...
    ],
    "total_questions": 50,
    "categories": {
        "1": "Science",
        "2": "Art",
        // More categories...
    },
    "current_category": None
}
```

### DELETE /questions/{id}

**Description**
This endpoint deletes a question from the database.

**Parameters**
id: (required, integer) The ID of the question to be deleted.

**Response**
A JSON object containing:

`success`: (boolean) Indicates whether the request was successful.
`deleted`: (integer) The ID of the deleted question.

**Error Handling**
If the question with the provided ID does not exist, the server will respond with a 404 status code.

**Example Request**
`DELETE /questions/10`

**Example Response**

```
{
    "success": True,
    "deleted": 10
}
```

### POST /questions

**Description**
This endpoint adds a new question to the database.

**Request Body**
A JSON object containing:

`question`: (required, string) The text of the question.
`answer`: (required, string) The answer to the question.
`difficulty`: (required, integer) The difficulty level of the question.
`category`: (required, integer) The ID of the category the question belongs to.

**Response**
A JSON object containing:

`success`: (boolean) Indicates whether the request was successful.
`created`: (integer) The ID of the created question.

**Error Handling**
If any of the required fields are missing in the request body, the server will respond with a 422 status code.

**Example Request**
`POST /questions`

Request Body:

```
{
    "question": "What is the capital of Australia?",
    "answer": "Canberra",
    "difficulty": 3,
    "category": 6
}
```

**Example Response**

```
{
    "success": True,
    "created": 11
}
```

### POST /questions/search

**Description**
This endpoint searches for questions that contain a given search term.

**Request Bod**y
A JSON object containing:

`searchTerm`: (required, string) The term to search for in the questions.

**Response**
A JSON object containing:

`success`: (boolean) Indicates whether the request was successful.
`questions`: (array) A list of questions that contain the search term. Each question is represented as a formatted object.
`total_questions`: (integer) The total number of questions that contain the search term.
`current_category`: (null) This field is always null in the current implementation.

**Error Handling**
If the searchTerm field is missing in the request body, the server will respond with a 422 status code.

**Example Request**
POST `/questions/search`

Request Body:

```
{
    "searchTerm": "capital"
}
```

**Example Response**

```
{
    "success": True,
    "questions": [
        {
            "id": 11,
            "question": "What is the capital of Australia?",
            "answer": "Canberra",
            "difficulty": 3,
            "category": 6
        },
        // More questions...
    ],
    "total_questions": 5,
    "current_category": None
}
```

### GET /categories/{id}/questions

**Description**
This endpoint retrieves all questions for a specific category.

**Parameters**
`id`: (required, integer) The ID of the category for which to retrieve questions.

**Response**
A JSON object containing:

`success`: (boolean) Indicates whether the request was successful.
`questions`: (array) A list of questions for the requested category. Each question is represented as a formatted object.
`total_questions`: (integer) The total number of questions in the requested category.
`current_category`: (string) The type of the current category.

**Error Handling**
If the category with the provided ID does not exist, the server will respond with a 404 status code.

**Example Request**
GET `/categories/1/questions`

**Example Response**

```
{
    "success": True,
    "questions": [
        {
            "id": 1,
            "question": "What is the capital of Australia?",
            "answer": "Canberra",
            "difficulty": 3,
            "category": 1
        },
        // More questions...
    ],
    "total_questions": 10,
    "current_category": "Science"
}
```

### POST /quizzes

**Description**
This endpoint retrieves a random question for a quiz game. It ensures that the same question is not played twice.

**Request Body**
A JSON object containing:

`previous_questions`: (required, array) A list of question IDs that have already been played.
`quiz_category`: (required, object) An object containing the ID of the category for the quiz. If the ID is 0, questions from all categories are considered.

**Response**
A JSON object containing:

`success`: (boolean) Indicates whether the request was successful.
`question`: (object) A random question that has not been previously played. The question is represented as a formatted object.

**Error Handling**
If the previous_questions or quiz_category fields are missing in the request body, the server will respond with a 422 status code.

**Example Request**
POST `/quizzes`

Request Body:

```
{
    "previous_questions": [1, 2, 3],
    "quiz_category": {
        "id": 1
    }
}
```

**Example Response**

```
{
    "success": True,
    "question": {
        "id": 4,
        "question": "What is the capital of Australia?",
        "answer": "Canberra",
        "difficulty": 3,
        "category": 1
    }
}
```

## Testing

Write at least one test for the success and at least one error behavior of each endpoint using the unittest library.

To deploy the tests, run

```bash
dropdb trivia_test
createdb trivia_test
psql trivia_test < trivia.psql
python test_flaskr.py
```
