# Backend - Full Stack Trivia API 

### Installing Dependencies for the Backend

1. **Python 3.7** - Follow instructions to install the latest version of python for your platform in the [python docs](https://docs.python.org/3/using/unix.html#getting-and-installing-the-latest-version-of-python)


2. **Virtual Enviornment** - We recommend working within a virtual environment whenever using Python for projects. This keeps your dependencies for each project separate and organaized. Instructions for setting up a virual enviornment for your platform can be found in the [python docs](https://packaging.python.org/guides/installing-using-pip-and-virtual-environments/)


3. **PIP Dependencies** - Once you have your virtual environment setup and running, install dependencies by naviging to the `/backend` directory and running:
```bash
pip install -r requirements.txt
```
This will install all of the required packages we selected within the `requirements.txt` file.


4. **Key Dependencies**
 - [Flask](http://flask.pocoo.org/)  is a lightweight backend microservices framework. Flask is required to handle requests and responses.

 - [SQLAlchemy](https://www.sqlalchemy.org/) is the Python SQL toolkit and ORM we'll use handle the lightweight sqlite database. You'll primarily work in app.py and can reference models.py. 

 - [Flask-CORS](https://flask-cors.readthedocs.io/en/latest/#) is the extension we'll use to handle cross origin requests from our frontend server. 

### Database Setup
With Postgres running, restore a database using the trivia.psql file provided. From the backend folder in terminal run:
```bash
psql trivia < trivia.psql
```

### Running the server

From within the `./src` directory first ensure you are working using your created virtual environment.

To run the server, execute:

```bash
flask run --reload
```

The `--reload` flag will detect file changes and restart the server automatically.

## ToDo Tasks
These are the files you'd want to edit in the backend:

1. *./backend/flaskr/`__init__.py`*
2. *./backend/test_flaskr.py*


One note before you delve into your tasks: for each endpoint, you are expected to define the endpoint and response data. The frontend will be a plentiful resource because it is set up to expect certain endpoints and response data formats already. You should feel free to specify endpoints in your own way; if you do so, make sure to update the frontend or you will get some unexpected behavior. 

1. Use Flask-CORS to enable cross-domain requests and set response headers. 


2. Create an endpoint to handle GET requests for questions, including pagination (every 10 questions). This endpoint should return a list of questions, number of total questions, current category, categories. 


3. Create an endpoint to handle GET requests for all available categories. 


4. Create an endpoint to DELETE question using a question ID. 


5. Create an endpoint to POST a new question, which will require the question and answer text, category, and difficulty score. 


6. Create a POST endpoint to get questions based on category. 


7. Create a POST endpoint to get questions based on a search term. It should return any questions for whom the search term is a substring of the question. 


8. Create a POST endpoint to get questions to play the quiz. This endpoint should take category and previous question parameters and return a random questions within the given category, if provided, and that is not one of the previous questions. 


9. Create error handlers for all expected errors including 400, 404, 422 and 500. 

## API Reference

### Getting Started

This Application can only be run locally. The backend app is hosted at default `http://127.0.0.1:5000/`, frontend proxy is configurated.

Authentication or API keys are not required.

### Error Handling

Three error types are implemented:

- 400: Bad request
- 404: Resource not found
- 422: Not processable
- 500: Internal server error

Errors arre returned in JSON format:

```
{
  "success": false, 
  "error": 404, 
  "message": "resource not found"
}
```


### Endpoints

#### GET ('/categories')
- General:
    - Fetches a dictionary of categories in which the keys are the ids and the value is the corresponding string of the category
    - Request Arguments: None
- Sample:
    - `curl http://127.0.0.1:5000/categories`

- Returns:
```
{
  "success": true, 
  "categories": {
    "1": "Science", 
    "2": "Art", 
    "3": "Geography", 
    "4": "History", 
    "5": "Entertainment", 
    "6": "Sports"
  }, 
  "total_categories": 6
}
```

#### GET ('/questions')
- General:
    - Fetches a dictionary of paginated questions. 10 questions per page 
    - Request Arguments: page (type=int), default = 1
- Sample:
    - `curl http://127.0.0.1:5000/questions?page=1`

- Returns:
```
{
  "success": true, 
  "questions": [
    {
      "id": 5, 
      "question": "Whose autobiography is entitled 'I Know Why the Caged Bird Sings'?", 
      "answer": "Maya Angelou", 
      "category": 4, 
      "difficulty": 2
    }
  ], 
  "totalQuestions": 20, 
  "categories": {
    "1": "Science", 
    "2": "Art", 
    "3": "Geography", 
    "4": "History", 
    "5": "Entertainment", 
    "6": "Sports"
  }, 
  "currentCategory ": "", 
  "page": 1
}
```

#### GET ('/category/<int:category_id>/questions')
- General:
    - Fetches a dictionary of paginated questions with the given Category(ID). 10 questions per page 
    - Request Arguments: page (type=int), default = 1
- Sample:
    - `curl http://127.0.0.1:5000/category/1/questions?page=1`

- Returns:
```
{
  "success": true, 
  "questions": [
    {
      "id": 20, 
      "question": "What is the heaviest organ in the human body?", 
      "answer": "The Liver", 
      "category": 1, 
      "difficulty": 4
    }, 
  ], 
  "totalQuestions": 20, 
  "currentCategory": "Science"
}
```

#### POST ('/questions')
- General:
    - (01.)Adds a question to the dictionary or (02.)search for a question in the dictonary

- 01.  Add a Question:    
    - Request data: {question(type=String),answer(type=String),category(type=Int),difficulty(type=Int)}
- Sample:
    - `curl -d '{"question":"question-text","answer":"answer_text","category":2,"difficulty":2}'\`
      `-H "Content-Type: application/json" \`
      `--request POST http://127.0.0.1:5000/questions`

- Returns:
```
{
  "success": true, 
  "question": {
    "id": 24, 
    "question": "question-text", 
    "answer": "answer_text", 
    "category": 2, 
    "difficulty": 2
  }
}
```
- 02.  Search for Questions:    
    - Request data: {search(type=String)} 
- Sample:
    - `curl -d '{"search":"Which"}' -H "Content-Type: application/json" --request POST http://127.0.0.1:5000/questions`

Returns:
```
{
  "success": true, 
  "questions": [
    {
      "id": 10, 
      "question": "Which is the only team to play in every soccer World Cup tournament?", 
      "answer": "Brazil", 
      "category": 6, 
      "difficulty": 3
    },  
  ], 
  "totalQuestions": 20, 
  "currentCategory": ""
}
```    

#### POST ('/quizzes')
- General:
    - Retrieve a question to play the quiz

- Request data: {previous_questions[type=int],quiz_category{'id': type=int}}
- Sample:
    - `curl -d '{"previous_questions":[1],"quiz_category":{"id":0}\`
      `-H "Content-Type: application/json" \`
      `--request POST http://127.0.0.1:5000/quizzes`

- Returns:
```
{
  "success": true, 
  "question": {
    "id": 2, 
    "question": "What movie earned Tom Hanks his third straight Oscar nomination, in 1996?", 
    "answer": "Apollo 13", 
    "category": 5, 
    "difficulty": 4
  }
}
```


#### DELETE ('/questions/<int:question_id>')
- General:
    - Deletes a question with the given ID from the dictionary, if it exists 
- Sample:
    - `curl --request DELETE http://127.0.0.1:5000/questions/24`

- Returns:
```
{
  "success": true
}
```

## Review Comment to the Students
```
This README is missing documentation of your endpoints. Below is an example for your endpoint to get all categories. Please use it as a reference for creating your documentation and resubmit your code. 

Endpoints
GET '/api/v1.0/categories'
GET ...
POST ...
DELETE ...

GET '/api/v1.0/categories'
- Fetches a dictionary of categories in which the keys are the ids and the value is the corresponding string of the category
- Request Arguments: None
- Returns: An object with a single key, categories, that contains a object of id: category_string key:value pairs. 
{'1' : "Science",
'2' : "Art",
'3' : "Geography",
'4' : "History",
'5' : "Entertainment",
'6' : "Sports"}

```


## Testing
To run the tests, run
```
dropdb trivia_test
createdb trivia_test
psql trivia_test < trivia.psql
python test_flaskr.py
```
