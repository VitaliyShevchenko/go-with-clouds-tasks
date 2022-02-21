## Key-value storage
Key-value storage is a non-relational database stores data as a collection of key-value pairs.

### Sub task1 (Simple key-value storage)

Build a simple, non-distributed key-value storage that can support next thins:
1. It must be able to store key-value pairs.
2. It must provide API for a user to put, get and delete key-value pairs.
3. It must be steady and always be available for storing the data.
4. It must be idempotent.

### Sub task2 (HTTP server + REST API)
Build a simple HTTP server/client for the key-value storage project.
1. Create a Go server to handle HTTP requests. (using **net/http** library)
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
 - Must respond with a 404 (Not Found) when a requested key does not exist.
 - Must respond with the requested value and a status 200 (Ok) if the key exists.
 - Must respond to unexpected errors with a 500 (Internal Server Error).
**DELETE**
 - Must only match DELETE requests for **specific_uri** (you will have to choose the URI).
 - Must respond with a status 200 (Ok) indicating  a successful deletion with additional information. In this case, the response body can contain the deleted resource or some details about the deletion.
 - Must respond with a status 204 (No content) indicating a successful deletion with no additional information (response body is empty).
 - Must respond with a status 202 (Accepted) is returned if the server accepted the request, but the deletion has not been completed.
 - Must respond with a 404 (Not found) if no resource exists at the given URI.

### Sub task3 (Concurrency)
Make key-value storage server concurrent save. Go's standard library provides mutual exclusion with **sync.Mutex**.
A general idea is when the key-value storage is being modified, the storage should be **Locked**. So that any other processes will wait until the current process is finished.
If it needs to read data from the storage, it should be **RLocked**(Read Locked).

### Sub task4 (Go templates)
The HTTP server should be allowed to return web pages with key-value storage key pairs. For that, each REST API handler must be able to return Web Pages.
There are two packages operating with templates in Go — **text/template** and **html/template**. Both provide the same interface, 
however the **html/template** package is used to generate HTML output safe against code injection.
TODO:vishevch:does it mean we will have to use **gorrila/mux** or something?

### Sub task5 (HTTPs server)
HTTP data between server and client is not encrypted, so it can be intercepted by third parties to gather data passed from the server to the client. 
This can be addressed by using a secure version called HTTPS. **The key-value storage server has to become HTTPs instead of HTTP.**
Small tip: **net/http** library allows to achieve that.

### Sub task6 (Logging)
The key-value storage server should have an ability for troubleshooting some issues/bugs and for identifying infrastructure problems.
This can be achieved using logging functionality, Go provides a library which helps easily integrate this to your application. The library name is **log**
TODO:vishevch: probably use zapr instead if log

### Sub task7 (Persisting Resource State)


### Sub task8 (Containerization)
#### Pre requirements
1. Install **Docker desktop**

#### Description
The key-value storage server should be containerized in Docker container. The result of this task should be a docker image, which everyone can run locally 
and show the key-value storage works as expected.

### Sub task9 (Build tool)
At this point, there are too many tasks which we have to run manually every time we want to do some action, such as `go build`, `go test`, `docker run`, `docker build`, etc.
A build tool (Gradle) helps to automate tasks that we would otherwise have to manually perform or "manually automate."
In the scope of this task, next gradle commands have to be implemented: 
1. `dockerBuild` — runs `docker build` command
2. `publish` — runs `docker push` command
3. `tests` — runs `go tests` and `go lint` commands

### Sub task10 (CI build)
Continuous Integration (CI) is a way to increase code quality without putting an extra burden on the developers. Tests and checks of your code are handled on a server and automatically reported back to you.
The project has to have ability to support CI. In the scope of this task, implement a project’s entire **build/test/deploy** pipeline in a **Jenkinsfile**.

### Sub task11 (Kubernetes getting started)
#### Pre requirements
1. Install **kubectl**, **Lens**

#### Exercises for getting familiar with Kubernetes world
1. Exercise creating clusters in **Docker desktop**
2. Exercise deploying **hello world image** in Kubernetes

#### Description of the task
Deploy key-value storage to Kubernetes. Needs to create a new folder in the project called `templates` and create `Deployment` resource for the key-value storage app.

### Sub task12 (kubebuilder)
Create a custom resource for key-value application using kubebuilder. Implement a controller for it.
The Kubernetes Custom Resource Definition  has the following schema:
```yaml
apiVersion: teamdev.com/v1
kind: KIND

spec:
  someField: "someValue"
  someField2: "someValue2"

status:
  conditions:
  - type: "TYPE"
    
    # True or False
    status: True
    
    reason: REASON
    
    message: MESSAGE
    
    lastUpdateTime: ""
```
TODO:vishevch:fill CRD definition

### Sub task13 (webhooks)
The CRD should be validated properly, to achieve that custom validating_webooh for the CRD should be implemented.
#### Validation rules:
TODO:vishevch:add validation rules


### Sub task14 (package managers)
The application should be packaged to Helm (one of the available package managers) chart. This will helm to define, install, 
and upgrade even the most complex Kubernetes application.


## IMPORTANT FOR EACH SUB TASK
#### A good level of documentation and unit test coverage are required.