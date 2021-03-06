## Key-value storage
Key-value storage is a non-relational database that stores data as a collection of key-value pairs.

### Sub task1 (Simple key-value storage)

#### Description
Build a simple, non-distributed key-value storage that can support the next things:
1. It must be able to store key-value pairs.
2. It must provide API for a user to `put`, `get` and `delete` key-value pairs.
3. It must be steady and always be available for storing the data.
4. It must be idempotent.

**Note**: A good level of documentation and unit test coverage is required.

[Lecture Notes](https://docs.google.com/presentation/d/1AC5O6HXgu7M1-NNktqrrjdgWbSM2KmZB/edit?usp=sharing&ouid=104154946265606394823&rtpof=true&sd=true)
#### Something to read

* [Cloud Native](https://cloudnative101.dev/concepts/cloud-native/)
* [How to design a good API](http://www.cs.bc.edu/~muller/teaching/cs102/s06/lib/pdf/api-design) — must read
* [Why Golang?](https://thechief.io/c/editorial/why-golang-is-widely-used-in-the-devops-and-cloud-native-space/)
* [API Design](https://www.infoq.com/articles/API-Design-Joshua-Bloch/)

### Sub-task 2 (HTTP server + REST API)

Build a simple HTTP server/client for the key-value storage project.

1. Create a Go server to handle HTTP requests. (using **net/http** library)
2. Use JSON to accept requests and reply to them
3. Write a table with a specification for REST API. (see example below)

   | Functionality     | Method | URI    | Status code |
   |-------------------|--------|--------|------------ |
   | Create a user     | POST   | /users | 200         |

4. Add the next handlers to the server: **GET ALL**, **GET**, **PUT**, **DELETE**.

* **GET ALL**

- Must only match GET requests for **specific_uri** (you will have to choose the URI).
- Must respond with a 404 (Not Found) when the storage is empty.
- Must respond with all the data of key-value storage and a status 200 (Ok).
- Must respond to unexpected errors with a 500 (Internal Server Error).

* **GET**

- Must only match GET requests for **specific_uri** (you will have to choose the URI).
- Must respond with a 404 (Not Found) when a requested key does not exist.
- Must respond with the requested value and a status 200 (Ok) if the key exists.
- Must respond to unexpected errors with a 500 (Internal Server Error).

* **PUT**

- Must only match PUT requests for **specific_uri** (you will have to choose the URI).
- Must respond with a 201 (Created) when a key-value pair is created.
- Must respond to unexpected errors with a 500 (Internal Server Error).

* **DELETE**

- Must only match DELETE requests for **specific_uri** (you will have to choose the URI).
- Must respond with a status 200 (Ok) indicating a successful deletion with additional information. In this case, the response
  body can contain the deleted resource or some details about the deletion.
- Must respond with a 404 (Not found) if no resource exists at the given URI.

**Note**: A good level of documentation and unit test coverage is required.

**Hint**: Learn to separate interfaces and interface consumers from the interface implementations
with [this article](https://medium.com/applike/golang-interfaces-809f2746a24f). This might be useful during implementation of HTTP
handlers which depend on the Storage interface.

**Notes**: [Lecture Notes](https://docs.google.com/presentation/d/1CxWvqUCgxkY0ivp9VCGhV80-0sfnbSSE/edit?usp=sharing&ouid=104154946265606394823&rtpof=true&sd=true)

#### Something to read

* [TCP/IP](https://www.objc.io/issues/10-syncing-data/ip-tcp-http/) — must read
* [TCP Technical overview](https://medium.com/@jimmyclem/a-technical-overview-of-tcp-pt-1-62e777c0d01)
* [Data serialization](https://towardsdatascience.com/what-why-and-how-of-de-serialization-in-python-2d4c3b622f6b)
* [Data serialization formats](https://study-ccna.com/data-serialization-formats-json-yaml-xml/)
* [URL & URI](https://danielmiessler.com/study/difference-between-uri-url/)
* [GoLang & JSON](https://gobyexample.com/json)

### Sub-task 3 (Go templates)

The HTTP server should have a REST API, which will return all data from the key-value storage as HTML page.  
The html page should show all key pairs as html table or divs with CSS styles.  
If the key-value storage is empty, an appropriate message should be shown to a user.

**Hint**: two packages are operating with templates in Go — **text/template** and **html/template**. Both provide the same
interface, however, the **html/template** package is used to generate HTML output safe against code injection.

#### Some examples of output

![empty key-value storage](examples/empty%20key-value.png)
![not empty key-value storage](examples/not-empty-key-value.png)

Info on Go templates:

* [A quick overview of go templates](https://betterprogramming.pub/how-to-use-templates-in-golang-46194c677c7d):
  read and execute code snippets in the article, play with them to learn the API.
* Read [Full spec of go template package](https://pkg.go.dev/text/template).

### Sub-task 4 (Concurrency)

Add client package which is capable of connecting to the server and initiating requests.

Add client benchmark tests.  
Two tests: one which is adding records to the server sequentially, another one which runs requests concurrently using goroutines.

- initiate client requests to the server to put some records on it
- wait when all requests are finished (tip: `sync.WaitGroup` can be used)
- get all records from the server and assert they are equal to the input data
- use benchmark ability of go `testing` lib to output performance results
- be able to provide the number of requests on the command line when running requests to get performance results for different
  number of requests
- attach benchmarking results to the PR with number of requests equal to 10 and 50 for both sequential and concurrent types of
  tests.

In Go maps are not concurrently-safe, so when you run the above concurrent test you'll see some unexpected results. After running
the tests against the simple server improve it to be concurrently-safe and run the tests again.
Use [this article](https://medium.com/@luanrubensf/concurrent-map-access-in-go-a6a733c5ffd1) to help you in the implementation.

Info on Go concurrency:

* [Why go concurrency is great?](https://www.slideshare.net/jsimnz/concurrency-with-go) - an overview
* [Concurrency patterns](https://talks.golang.org/2012/concurrency.slide#1) - read carefully, execute code snippets

### Sub-task 5 (Transaction Logger)

If the key-value service was crashed/restarted or found itself in an inconsistent state, it should have the ability to **recover**
the system. The requirement here is to use **go channels**.

**Hint**:

* log **PUT** and **DELETE** actions as entries in the file.
* at the start of the server, if the file is present -> start a goroutine to load entries into the memory and send them to a
  channel.
* the channel is used by another function to receive entries and make API requests to the key-value storage to restore these
  records into the storage.

### Sub-task 6 (HTTPS server)

HTTP data between server and client is not encrypted, so it can be intercepted by third parties to gather data passed from the
server to the client. This can be addressed by using a secure version called HTTPS. **The key-value storage server has to become
HTTPS instead of HTTP.**

Small hint: **net/http** library allows to achieve that.

**Notes**: [Lecture Notes](https://docs.google.com/presentation/d/1px4YUry67PEDBm-Sta-5T_iaBpdvWkmX/edit?usp=sharing&ouid=104154946265606394823&rtpof=true&sd=true)

#### Something to read
* [What is HTTPs?](https://habr.com/ru/post/593507/)
* [Example of TLS + HTTPs implementation in GoLang](https://gist.github.com/denji/12b3a568f092ab951456)
* [Certificates](https://smallstep.com/blog/everything-pki/#:~:text=A%20certificate%20is%20a%20data,certificate%20is%20called%20the%20subject.)


### Sub-task 7 (Logging)
The key-value storage server should have the ability for troubleshooting some issues/bugs and for identifying infrastructure
problems. This can be achieved using logging functionality, Go provides a library that helps easily integrate this to your
application. The library name is **log**.

#### Something to read

* [What is logging and why it's needed](https://towardsdatascience.com/why-should-you-care-about-logging-442a195b80a1)
* [Logging Best Practices](https://www.dataset.com/blog/the-10-commandments-of-logging/)

### Sub-task 8 (Containerization)

#### Pre requirements

1. Install **Docker desktop**

### Sub-task 8.1 (Get familiar with Linux)

Docker is built on Linux system, most programs in cloud run on Linux, so it's vitally important to learn it. Here is a
full-fledged tutorial: [unix-quick-guide](https://www.tutorialspoint.com/unix/unix-quick-guide.htm). Better to get familiar with
all topics in the tutorial but the most important ones are the following:

* "Unix - File Management" till "Unix - File Permission / Access Modes"
* "Unix - Environment"
* "Unix - Pipes and Filters"
* The vi Editor Tutorial
* "Unix - What is Shells?" till "Unix - Shell Manpage Help"
* "Unix - File System Basics"

The task is to write a shell script `kv_upload_items.sh` which:

1. Accepts source text file containing JSON key-value pairs per line
2. Reads the file and uses `curl` command to store these records on the running key-value server
3. Uses `curl` command to get all records from the server
4. Checks if all the records from the source file are in the GET-ALL response
5. Outputs "All records stored" if it was successful and "Failed to store records" otherwise

To develop it use ubuntu container with mounted project repo directory, commit it to the repo and put the instruction how to run
the script in the Readme.md file:

```sh
PROJECT_DIR=/path/to/project_dir
docker run -it --mount src="$PROJECT_DIR",target=/project_dir,type=bind ubuntu
# inside the container
cd /project_dir
./kv_upload_items.sh /path/to/file_with_json_key_value_pairs
```

### Sub-task 8.2 (Package key-value storage as a docker image)

The key-value storage server should be containerized in a Docker container. The result of this task should be:

1. `Dockerfile` — a text file that contains all commands, in order, needed to build a given image.
2. pushed docker image into a public docker repository (which you have to create)
3. everyone should be able to pull the image and run it locally and show that the key-value storage works as expected.

**Notes**: [Lecture Notes](https://docs.google.com/presentation/d/19oJTwt9juglbeAWR07T6CNp1NBUeGMRT/edit?usp=sharing&ouid=104154946265606394823&rtpof=true&sd=true)

#### Something to read

* [Docker getting started](https://medium.com/@kmdkhadeer/docker-get-started-9aa7ee662cea#:~:text=Docker%20is%20a%20set%20of,other%20through%20well%2Ddefined%20channels.)
* [What's Docker repository and how to work with it](https://docs.docker.com/docker-hub/repos/#:~:text=To%20push%20an%20image%20to,docs%2Fbase%3Atesting%20)
* [Container terminology in details](https://developers.redhat.com/blog/2018/02/22/container-terminology-practical-introduction/)
* [Dockerfile tutorial](https://takacsmark.com/dockerfile-tutorial-by-example-dockerfile-best-practices-2018/)
* [Dockerfile best practices](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)

### Sub-task 9 (Build tools)

At this point, there are too many tasks that we have to run manually every time we want to do some action, such as `go build`
, `go test`, `docker run`, `docker build`, etc. A build tool (Gradle) helps to automate tasks that we would otherwise have to
manually perform or "manually automate".  
Also, the problem may arise for collaborators on the project that develop using different platforms (Linux, Windows, etc) and
using different tools and their versions to run the project. To solve it you'll use Vagrant.

### Sub-task 9.1 (Vagrant)

Install [vagrant](https://www.vagrantup.com/) and pass [Getting Started](https://learn.hashicorp.com/vagrant) guide. This tool
allows configuring and sharing engineering workspace and solves issues like “it doesn't work on my machine”.

Add `Vagrantfile` to the project and configure it. You can use VirtualBox or docker provider for your vagrant instance. All
commands that you’ll need to add for `gradle` need to be run in `vagrant`, and `Vagrantfile` should have provisioning instructions
for the environment and tools you’re using.

Install tools needed for key-value project:

- `golang`
- `docker`
- `gradle`

As a result you should be able to do `vagrant destroy`, `vagrant up`, `vagrant ssh` and then verify you can execute `go version`
, `docker version`, `gradle -v` on the vagrant machine.

**Notes**: [Lecture notes](https://skilshot.notion.site/Vagrant-cb1582365885471186fd9dcff9addf0c)

### Sub-task 9.2 (Gradle)

Gradle is a build tool. It allows automating tasks such as building, testing and publishing a project.

[Install gradle](https://docs.gradle.org/current/userguide/installation.html) (you did it in task 9.1) and
execute `gradle wrapper` to generate a [wrapper](https://docs.gradle.org/current/userguide/gradle_wrapper.html), specify gradle
version and commit it to the repo.  
Follow [official docs](https://docs.gradle.org/current/userguide/userguide.html) to get familiar with it.

Add the following gradle tasks for the project:

- cleanCode (meant to run before committing code)
    - Format code with `gofmt`
    - Optimize dependencies (`go mod tidy`)
    - Run [staticcheck](https://staticcheck.io/) tool
- build
    - Build key-value server go binary and puts it into `buildDir`
- test
    - Run go tests
- testingDone
    - Run go tests with coverage report and fail if doesn’t match the low boundary of 80 percent
- sanityCheck (we can use this task on CI to ensure PR changes don’t break the program)
    - Run key-value server and execute `kv_upload_items.sh` script.
      In future this task will be used to fail CI if upload is not successful.
- dockerBuild
    - Build key-value docker image using Dockerfile
- publish
    - Push the key-value docker image to dockerhub

**Notes**: [Lecture notes](https://skilshot.notion.site/Gradle-82d5e7822a6a451e83e7631554e9c633)

### Sub-task 10 (CI build)
Continuous Integration (CI) is a way to increase code quality without putting an extra burden on the developers.
Tests and checks of your code are handled on a server and automatically reported back to you.
The project has to have the ability to support CI. In the scope of this task, you will have to do next:
1. Implement a project’s entire pipeline in a **Jenkinsfile**. It should allow running **unit tests** and send an error with a description of what's failed.
2. Set up a project on GitHub to run CI for every PR or commit on the master branch.

**Notes**: [Lecture notes](https://docs.google.com/presentation/d/1l1Z8FvSELESts3SFfu-Sb2FMq--HvB_4/edit?usp=sharing&ouid=107330001931294770933&rtpof=true&sd=true)

### Sub-task 11 (Kubernetes)
#### Pre requirements
1. Install **kubectl**, **Lens**

#### Description of the task 11.1
A kubernetes cluster in docker should be created. The key-value storage should have ability to be deployed/undeployed on/from this cluster. 
To achieve that, you will have to do next: 
1. Create `templates` folder in the project. (most of the time all k8s related files are located here)
2. Create a YAML file with `Deployment` manifest, which will deploy key-value storage on the cluster. 
3. Add `deploy` gradle task — deploys the key-value storage app on the Kubernetes cluster. (Hint: `kubectl apply -f path_to_the_deployment_file`)
4. Add `undeploy` gradle task  — removes the key-value storage app from the Kubernetes cluster. (Hint: `kubectl delete -f path_to_the_deployment_file`)

#### Description of the task 11.2
The key-value replicas must be scaled up in case if there are more requests and load on the server growing. If the load is low,
the key-value replicas must be downscaled. Kubernetes has a resource (HPA), which aims to automatically scale the workload to match demand.
Need to create a new file (`hpa.yaml`) should be created in the `templates` folder and declare HPA manifest in this file. In addition to that,
you have to show that HPA works correctly if the load is growing (using a benchmark test).

**Notes**: [Lecture Notes](https://docs.google.com/presentation/d/1sYVamnWdMkq_B8Sbw-Ebp672CxkeytID/edit?usp=sharing&ouid=104154946265606394823&rtpof=true&sd=true)

#### Something to read&watch
* [Kubernetes Full Course in 7 Hours](https://www.youtube.com/watch?v=0j-iIW3_sbg&list=WL&index=15)
* [Kubernetes overview](https://kubernetes.io/docs/concepts/overview/)
* [Workloads](https://kubernetes.io/docs/concepts/workloads/)
* [Services, Load Balancing, and Networking](https://kubernetes.io/docs/concepts/services-networking/)
* [Configuration](https://kubernetes.io/docs/concepts/configuration/)
* [Security](https://kubernetes.io/docs/concepts/security/)
* [Volumes](https://kubernetes.io/docs/concepts/storage/volumes/)

### Sub-task 12 (K8s Operator using Kubebuilder)
We now build a k8s extension using Operator concept 
where we use Custom Resource Definition and controller-manager pod 
which allow working with key-value storage like with a native k8s resource.
[Kubebuilder](https://book.kubebuilder.io/introduction.html) is a k8s operator framework which makes it easy. 

**Notes**: [Lecture Notes](https://docs.google.com/presentation/d/1NyHlQP04iOwQi2RxatyQrt8YKUy9Q0P1QFyQ94y56Rg/edit?usp=sharing)

#### Sub-task 12.1 (CRD)
The task is to bootstrap kubebuilder project and define a CRD 
which allows to populate key-value storage. 

You need to handle all CRUD actions on your Custom Resource: 
* if user creates CR it should put that data in the storage;
* if updates - it should remove/add/update corresponding key-value pairs;
* if deletes - it should remove those keys from the storage.

The Kubernetes Custom Resource need to have the following schema:

```yaml
apiVersion: teamdev.com/v1
kind: KeyValueData
spec:
  data:
    <key1>: "someValue"
    <key2>: "someValue2"
status:
  # you can come up with your own status fields, below is just an example
  conditions:
  - type: "Added"
    # True or False
    # True, if the items were added to the key-value storage
    # False, if the items were not added to the key-value storage
    status: True/False
    # If status is False, the reason should be populated
    reason: REASON
    # If status is False, the message should be populated
    message: MESSAGE
    # A time when an object moves in this state
    lastUpdateTime: ""
```

#### Sub-task 12.2 (Webhooks)
The CRD should be validated properly, to achieve that custom validating_webhook for the CRD should be implemented.

Validation rules:
* it should deny adding `KeyValueData` resource with a key that is already defined in another resource

Make sure to have these validations for both create and update actions.

### Sub-task 13 (package managers)
The application should be packaged to Helm (one of the available package managers) chart. This will allow defining, installing,
and upgrade even the most complex Kubernetes application.
The helm chart should contain a parent chart and two sub-charts (**operator** and **storage**). All k8s manifests should be
located under appropriate charts and under `templates` resources.

In addition to that the following Gradle tasks should be added:
1. `createChart` — creates a Helm chart with all needed resources for the key-value storage
2. `installChart` — installs chart on Kubernetes cluster
3. `uninstallChart` — uninstalls chart from Kubernetes cluster

A list of fields which a user can dynamically populate:
1. Operator
```yaml
replicaCount: <replicaCount>

imageName: <imageName>
resources:
  limits:
    cpu: <cpu_limit>
    memory: <memory_limit>
  requests:
    cpu: <cpu_request>
    memory: <memory_request>

autoscaling:
  enabled: <enabled>
  minReplicas: <minReplicas>
  maxReplicas: <maxReplicas>
  targetCPUUtilizationPercentage: <targetCPUUtilizationPercentage>
```
2. Storage
```yaml
replicaCount: <replicaCount>

imageName: <imageName>
resources:
  limits:
    cpu: <cpu_limit>
    memory: <memory_limit>
  requests:
    cpu: <cpu_request>
    memory: <memory_request>

containerPort: <containerPort>
```

#### Something to read
* [Helm official documentation](https://helm.sh/)
* [What is, and what use cases have the dot "." in helm charts?](https://stackoverflow.com/questions/62472224/what-is-and-what-use-cases-have-the-dot-in-helm-charts)

**Notes**: [Lecture Notes](https://docs.google.com/presentation/d/1LcitbyO4RIIkea6bkdH5R6eOH6UjM11k/edit?usp=sharing&ouid=104154946265606394823&rtpof=true&sd=true)

### Sub-task 14 (move away from makefile in the operator component)
By default, all kubebuilder projects are scaffolded with a Makefile. In the scope of this task, you will need to move away from makefile and start using Gradle.
Gradle is really powerful and flexible. It entrusts the most core build rules to Plugin to complete. 
In this way, the versatility of the build process is easily realized.

The next tasks should be moved from Makefile to Gradle:
1. `manifests`
2. `generate`
3. `tests`
4. `dockerBuild`
5. `dockerPush`
6. `deploy`
7. `undeploy`

### Sub-task 15 (improve CI build) — Optional
At this point, the CI can be improved and can be smarter. It should be able to do next:
1. Run unit tests (functionality from the Sub task10).
2. Create docker image for the key-value storage app.
3. Push the docker image to the docker artifactory.
4. Create a helm chart for the key-value storage app.
5. Send an email that the chart was generated successfully, add the code coverage and attach the helm chart to the email.


## IMPORTANT FOR EACH SUBTASK
#### A good level of documentation and unit test coverage is required.
