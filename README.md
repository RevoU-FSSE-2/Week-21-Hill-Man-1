<<<<<<< HEAD
# FLASK SOCIAL MEDIA APP
Overview
This project is a Flask-based web application that utilizes SQLAlchemy as its ORM (Object-Relational Mapping) for database interactions. The application models include User, Tweet, and Following to implement a basic social media platform. The project structure is organized into three main files: pipfile, db.py, and model files (user_model.py, tweet_model.py, following_model.py).

## Getting Started
### Prerequisites
- Python 3.12
- PostgreSQL database (You can use the provided .env file for the connection details)

## Dependencies
The project utilizes the following Python packages:

```python
[[source]]
url = "https://pypi.org/simple"
verify_ssl = true
name = "pypi"

[packages]
flask = "*"
flask-sqlalchemy = "*"
psycopg2-binary = "*"
bcrypt = "*"
flask-bcrypt = "*"
marshmallow = "*"
injector = "*"
pyjwt = "*"

[dev-packages]

[requires]
python_version = "3.12"
```
### Installation
1. Clone the repository:
```bash
git clone https://github.com/RevoU-FSSE-2/Week-21-Hill-Man-1.git
cd flask-social-media-app
```
### Install dependencies:
```bash
pip install -r requirements.txt
```
2. Set up the database:
Uncomment the following lines in app.py to initialize the database:

``` python
# with app.app_context():
#     db_init()
```

### Create and populate the .env file:
Create a file named .env in the project root and add the following:

```env
DATABASE_URL=postgresql://postgres:RevouWeek21***@db.dxtvuacfifnhoufyzvca.supabase.co:5432/postgres
SECRET_KEY=thisis*****
```
Replace placeholders with your actual database connection details.

## Running the App
Run the Flask application with the following command:

```bash
python app.py
```
The app will be accessible at http://localhost:5000.

## API Endpoints
1 User Registration:

```http
POST /auth/registration
```
Register a new user with a unique username.

2. User Login:

```http
POST /auth/login
```

Log in with a registered username and password.

3. Get User Profile:

```http
GET /user/
```

Retrieve the user's profile information, including followers, following, and recent tweets.

4. Post a Tweet:

```http
POST /tweet/
```
Post a new tweet on the user's timeline.

5. Follow/Unfollow User:

```http
POST /following/
```
Follow or unfollow another user.

## Database Initialization (db.py)
The db.py file initializes the SQLAlchemy database and defines a base model class (Base) for other models to inherit from. Additionally, it provides a db_init function to drop and recreate all tables, useful during development and testing.

```python
from flask_sqlalchemy import SQLAlchemy
from sqlalchemy.orm import DeclarativeBase

class Base(DeclarativeBase):
    pass

db = SQLAlchemy(model_class=Base)

def db_init():
    db.drop_all()
    db.create_all()
```
## User Model (user_model.py)
The User model represents user data and relationships. It includes fields such as id, username, password, bio, followers, and following. The model also establishes a relationship with the Following model to manage follower/following associations.

```
from db import db
from following.model import Following

class User(db.Model):
    # ... (fields definition)
    followers = db.Column(db.Integer, default=0)
    following = db.Column(db.Integer, default=0)
    following_relations = db.relationship('Following', back_populates='user', foreign_keys=[Following.user_id])
```

## Tweet Model (tweet_model.py)
The Tweet model represents a user's tweet. It includes fields such as id, tweet, user_id, and publish_at. The model establishes a relationship with the User model to associate tweets with specific users.

```python
from db import db
import datetime

class Tweet(db.Model):
    # ... (fields definition)
    user = db.relationship("User", backref=db.backref('tweets', lazy=True))
    publish_at = db.Column(db.DateTime, nullable=False, default=datetime.datetime.utcnow())
```

## Following Model (following_model.py)
The Following model manages the relationships between users who follow each other. It includes fields such as id, user_id, and following_user_id. This model establishes relationships with the User model to maintain follower/following associations.

```python
from db import db

class Following(db.Model):
    # ... (fields definition)
    user = db.relationship('User', back_populates='following_relations', foreign_keys=[user_id])
```

## POSTMAN TEST
Check this API using Postman [CLICK HERE!](https://elements.getpostman.com/redirect?entityId=29017942-24880bd1-df51-421b-993a-91442ae2805c&entityType=collection)

## License
This project is licensed under the MIT License - see the LICENSE file for details.
