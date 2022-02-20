## Key-value storage
Key-value storage is a non-relational database stores data as a collection of key-value pairs.

### Sub task1

Build a simple, non-distributed key-value storage that can support next thins:
1. It must be able to store key-value pairs.
2. It must provide API for a user to put, get and delete key-value pairs.
3. It must be steady and always be available for storing the data.
4. It must be idempotent.

Unit Tests and Documentation
A good level of documentation and unit test coverage are required.

### Sub task2

Add the next functionality to the Sub task1:
1. Create a Go server to handle HTTP requests.
2. Create a client for testing.
3. Write a table with a specification for REST API. (see example below)

   | Functionality     | Method | URI    | Status code |
   |-------------------|--------|--------|------------ |
   | Create a user     | POST   | /users | 200         |

4. Add the next handlers to the server: **GET**, **PUT**, **DELETE**.
**PUT**
 - Must only match PUT requests for **specific_uri** (you will have to choose the URI). 
 - Must respond with a 201 (Created) when a key-value pair is created.
 - Must respond to unexpected errors with a 500 (Internal Server Error).
**GET**
 - Must only match GET requests for **specific_uri** (you will have to choose the URI).
 - Must respond with a 404 (Not Found) when a requested key doesn’t exist.
 - Must respond with the requested value and a status 200 (Ok) if the key exists.
 - Must respond to unexpected errors with a 500 (Internal Server Error).
**DELETE**
 - Must only match DELETE requests for **specific_uri** (you will have to choose the URI).
 - Must respond with a status 200 (Ok) indicating  a successful deletion with additional information. In this case, the response body can contain the deleted resource or some details about the deletion.
 - Must respond with a status 204 (No content) indicating a successful deletion with no additional information (response body is empty).
 - Must respond with a status 202 (Accepted) is returned if the server accepted the request, but the deletion has not been completed.
 - Must respond with a 404 (Not found) if no resource exists at the given URI.

### Sub task3
Add MUTEX support for the key-value storage project. Go's standard library provides mutual exclusion with **sync.Mutex**.


## IMPORTANT FOR EACH SUB TASK
#### A good level of documentation and unit test coverage are required.