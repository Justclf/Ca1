# CA1
## Table of Contents
- [CA1](#ca1)
  - [Table of Contents](#table-of-contents)
  - [Overview](#overview)
  - [Features](#features)
  - [Setup](#setup)
  - [Endpoints](#endpoints)
    - [Section A: Bug Bounty System](#section-a-bug-bounty-system)
    - [Section B: Gamification](#section-b-gamification)
  - [Testing](#testing)





## Overview
This project is a backend server on Bug Bounty System and infusing gamification on it. Using Node.js, Express, MySQL2, and nodemon. 

## Features
- User features
- Vulnerability features
- Report features
- Gameusers features
- Quests features

## Setup
**Installation**
1. Clone the repository:
   ```bash
   git clone https://github.com/ST0503-BED/bed-ca1-Justclf.git
   ```
2. Navigate to the project directory:
   ```bash
   cd bed-ca1-Justclf
   ```
3. Install the dependencies:
   ```bash
    npm install express
    ```
    ```bash
    npm install mysql2
    ```
    ```bash
    npm install nodemon
    ```
    ```bash
    npm install dotenv
    ```
4. Create a `.env` file in the root directory and add your database configuration:
    ```bash
    DB_HOST="localhost"
    DB_USER="Username"
    DB_PASSWORD="Password"
    DB_NAME="DatabaseName"
    ```
5. Input the following command to ensure smooth running of the server in `.package.json`:
   ```bash
    "init_tables": "node src/configs/createSchema.js && node src/configs/initTables.js",
    "start": "node index.js",
    "dev": "nodemon index.js"
   ```
6. Initialize the database:
   ```bash
   npm run init_tables
   ```
7. Start the server:
   ```bash
   npm run dev
   ```

## Endpoints
### Section A: Bug Bounty System
- **Users**
  1. **POST /users**
     - Create a new user by providing the required details in the request body.
     - Example request body:
        ```json
        {
        "username": "Pentester123"
        }
        ```
     - Example response:
        ```json
        {
        "id": 1,
        "username": "Pentester123",
        "reputation": 0
        }
        ```
      - **Status Code: 201 Created**
      - **Error Handling:**
        - If the provided username is already associated with anothe user, return 409 Conflict
        - If the request body is missing username, return 400 Bad Request
   
   2. **GET /users**
      - Retrieve a list of all users.
      - Example response:
        ```json
        [
          {
            "id": 1,
            "username": "Pentester123",
            "reputation": 0
          },
          {
            "id": 2,
            "username": "BugHunter456",
            "reputation": 10
          }
        ]
        ```
      - **Status Code: 200 OK**

    3. **GET /users/{user_id}**
       - Retrieve a specific user by their ID.
       - Example response:
         ```json
         {
           "id": 1,
           "username": "Pentester123",
           "reputation": 0
         }
         ```
       - **Status Code: 200 OK**
       - **Error Handling:**
         - If the requested user id does not exist, return 404 Not Found

    4. **PUT /users/{user_id}**
       - Update a specific user by their ID.
       - Example request body:
         ```json
         {
           "username": "turnWhitehat",
           "reputation": 100
         }
         ```
       - Example response:
         ```json
         {
           "id": 1,
           "username": "turnWhitehat",
           "reputation": 100
         }
         ```
       - **Status Code: 200 OK**
       - **Error Handling:**
         - If the requested user id does not exist, return 404 Not Found
         - If the provided username is already associated with another user, return 409 Conflict




- **Vulnerabilities**
    1. **POST /vulnerabilities**
       - Create a new vulnerability by providing the required details in the request body.
       - Example request body:
        ```json
        {
          "title": "XSS",
          "description": "The search bar is vulnerable to cross-site scripting.",
          "points": 75
        }
        ```
        - Example response:
        ```json
        {
          "id": 1
        }
        ```
      - **Status Code: 201 Created**
      - **Error Handling:**
        - If the request body is missing any required fields, return 400 Bad Request

    2. **GET /vulnerabilities**
       - Retrieve a list of all vulnerabilities.
       - Example response:
        ```json
        [
          {
            "id": 1,
            "title": "XSS",
            "description": "The search bar is vulnerable to cross-site scripting.",
            "points": 75
          },
          {
            "id": 2,
            "title": "SQL Injection",
            "description": "The login form is vulnerable to SQL injection.",
            "points": 100
          }
        ]
        ```
        - **Status Code: 200 OK**
  

- **Reports**
    1. **POST /reports**
       - Create a new report linking a user to a vulnerability. Provide user_id and
vulnerability_id in the request body. The initial status of the report should be set to 0
       - Example request body:
        ```json
        {
            "user_id": 2,
            "vulnerability_id": 5
        }
        ```
        - Example response:
        ```json
        {
            "id": 1, // this is the report ID
            "user_id": 2, // the ID of the user who submitted the report
            "vulnerability_id": 5, // the ID of the vulnerability being reported
            "status": 0, // 0 for open, 1 for resolved
            "user_reputation": 100, // Assuming user 2 had 0 reputation and vulnerability 5 has 100 points
        }
        ```
        - **Status Code: 201 Created**
        - **Error Handling:**
          - If the request body is missing any user_id or vulnerability_id, return 400 Bad Request
          - If the user_id or vulnerability_id does not exist, return 404 Not Found

    2. **PUT /reports/{report_id}**
       - Update the status of a report by its ID. The status can be set to 1 (resolved).
       - Example request body:
        ```json
        {
            "status": 1,
            "user_id": 7 // ID of the user who is closing the report
        }
        ```
        - Example response:
        ```json
        {
            "id": 1, // report ID
            "status": 1, // 1 for resolved
            "closer_id": 7, // ID of the user who closed the report
            "user_reputation": 150 // updated reputation of user 7
        }
        ```
        - **Status Code: 200 OK**
        - **Error Handling:**
          - If the requested report_id or user_id does not exist, return 404 Not Found
          - If the request body is missing status or user_id, return 400 Bad Request


### Section B: Gamification
- **Gameusers**
    1. **POST /gameusers**
       - Create a new game user by providing the required details in the request body.
       - Example request body:
        ```json
        {
          "username": "GameUser123",
        }
        ```
        - Example response:
        ```json
        {
          "id": 1,
          "username": "GameUser123",
          "xp": 0,
          "Rank": "E-Rank Hunter"
        }
        ```
        
        - **Status Code: 201 Created**
        - **Error Handling:**
          - If the provided username is already associated with another game user, return 409 Conflict
          - If the request body is missing username, return 400 Bad Request


    2.  **GET /gameusers**
       - Retrieve a list of all game users.
       - Example response:
        ```json
        [
          {
            "id": 1,
            "username": "GameUser123",
            "xp": 0,
            "Rank": "E-Rank Hunter"
          },
          {
            "id": 2,
            "username": "GameUser456",
            "xp": 100,
            "Rank": "D-Rank Hunter"
          }
        ]
        ```
         - **Status Code: 200 OK**


    3.  **GET /gameusers/{gameuser_id}**
       - Retrieve a specific game user by their ID.
       - Example response:
         ```json
         {
           "id": 1,
           "username": "GameUser123",
           "xp": 0,
           "Rank": "E-Rank Hunter"
         }
         ```
         - **Status Code: 200 OK**
         - **Error Handling:**
           - If the requested gameuser_id does not exist, return 404 Not Found


    4.  **PUT /gameusers/{gameuser_id}**
       - Update a specific game user by their ID.

        - Example request body:
        ```json
        {
            "username": "GameUserUpdated"
        }
        ```
        - Example response:
        ```json
        {
            "id": 1,
            "username": "GameUserUpdated",
            "xp": 0,
            "Rank": "E-Rank Hunter"
        }
        ```
        - **Status Code: 200 OK**
        - **Error Handling:**
            - If the requested gameuser_id does not exist, return 404 Not Found
            - If the provided username is already associated with another game user, return 409 Conflict
            - If the request body is missing username, return 400 Bad Request

    5.  **POST /gameusers/pvp**
       - Create a new PvP match between two game users by providing their IDs in the request body.
       - Example request body:
        ```json
        {
          "challenger_id": 1,
          "opponent_id": 2
        }
        ```
        - Example response:
        ```json
        {
          "challenge_id": 1,
          "opponent_id": 2,
          "winner_id": 1,
          "winner_rank": "S-Rank Hunter",
          "loser_id": 2,
          "loser_rank": "A-Rank Hunter",
          "message": "Player 1 wins"
        }
        ```
        - **Status Code: 201 Created**
        - **Error Handling:**
          - If the request body is missing any required fields, return 400 Bad Request
          - If either challenger_id or opponent_id does not exist, return 404 Not Found



- **Quests**
    1. **POST /quests**
       - Create a new quest by providing the required details in the request body.
       - Example request body:
        ```json
        {
          "title": "Complete 5 reports",
          "description": "Complete 5 reports to earn points.",
          "xp_reward": 100,
          "recommended_rank": "C-Rank Hunter"
        }
        ```
        - Example response:
        ```json
        {
          "id": 1,
          "title": "Complete 5 reports",
          "description": "Complete 5 reports to earn points.",
          "xp_reward": 100,
          "recommended_rank": "C-Rank Hunter"
        }
        ```
      - **Status Code: 201 Created**
      - **Error Handling:**
        - If the request body is missing any required fields, return 400 Bad Request
        - If same quest title already exists, return 409 Conflict

    2. **GET /quests**
       - Retrieve a list of all quests.
       - Example response:
        ```json
        [
          {
            "id": 1,
            "title": "Complete 5 reports",
            "description": "Complete 5 reports to earn points.",
            "points": 50
          },
          {
            "id": 2,
            "title": "Find a critical vulnerability",
            "description": "Find a critical vulnerability to earn points.",
            "points": 100
          }
        ]
        ```
        - **Status Code: 200 OK**

    3. **GET /quests/{quest_id}**
       - Retrieve a specific quest by its ID.
       - Example response:
         ```json
         {
           "id": 1,
           "title": "Complete 5 reports",
           "description": "Complete 5 reports to earn points.",
           "xp_reward": 100,
           "recommended_rank": "C-Rank Hunter"
         }
         ```
         - **Status Code: 200 OK**
         - **Error Handling:**
           - If the requested quest_id does not exist, return 404 Not Found
  
    4. **PUT /quests/{quest_id}**
       - Update a specific quest by its ID.
       - Example request body:
         ```json
         {
           "title": "Complete 10 reports",
           "description": "Complete 10 reports to earn points.",
           "xp_reward": 200,
           "recommended_rank": "B-Rank Hunter"
         }
         ```
         - Example response:
         ```json
         {
           "id": 1,
           "title": "Complete 10 reports",
           "description": "Complete 10 reports to earn points.",
           "xp_reward": 200,
           "recommended_rank": "B-Rank Hunter"
         }
         ```
         - **Status Code: 200 OK**
         - **Error Handling:**
           - If the requested quest_id does not exist, return 404 Not Found
           - If the request body is missing any required fields, return 400 Bad Request
           - If same quest title already exists, return 409 Conflict

    5. **POST /quests/{quest_id}/start**
       - Start a quest by providing the quest_id in the request body.
       - Example request body:
       ```json
        {
           "user_id": 1
        }
        ```
       - Example response:
        ```json
        {
            "id": 1,
            "user_id": 1,
            "quest_id": 1,
            "status": "User {username} has started his quest",
            "started_at": "2023-10-01T12:00:00Z"
        }
        ```
        - **Status Code: 201 Created**
        - **Error Handling:**
            - If the requested quest_id does not exist, return 404 Not Found
            - If the request body is missing user_id, return 400 Bad Request
            - If the user has already started the quest, return 409 Conflict

    6.  **POST /quests/{quest_id}/complete**
       - Complete a quest by providing the quest_id in the request body.
       - Example request body:
        ```json
        {
           "user_id": 1
        }
        ```
        - Example response:
        ```json
        {
            "message": "Quest completed",
            "user_id": 1,
            "quest_id": 1,
            "rewarded_xp": 100,
            "total_xp": 200,
            "rank": "C-Rank Hunter",
            "xp_to_next": 100,
            "completed_at": "2023-10-01T12:00:00Z" 
        }
        ```
        - **Status Code: 201 Created**
        - **Error Handling:**
            - If the requested quest_id does not exist, return 404 Not Found
            - If the request body is missing user_id, return 400 Bad Request
            - If the user has not started the quest, return 409 Conflict


## Testing
- Use Postman to test the endpoints.