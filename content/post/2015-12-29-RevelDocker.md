+++
categories = ["docker", "go", "api", "mongo"]
date = "2015-12-29T16:16:50-03:00"
description = ""
keywords = ["docker", "go", "revel", "api", "mongo", "godep"]
title = "Mounting Go Revel API + MongoDB on Docker"
+++

Go is fast, modern and fun to use. There are good web frameworks like [martini](https://github.com/go-martini/martini) or [gin](https://github.com/gin-gonic/gin). The last time I tried `gin` I had lot of troubles declaring the routes, so I moved to Revel.

It is known that Go doesn't have a very good dependency management, so we will use [github.com/tools/godep](https://github.com/tools/godep).

I am a fan of Docker 🐳, because I like to keep things simple and easy to use (and deploy). So I will write about how to mount a Revel app using Docker.

> See the complete project on: [**github.com/mrpatiwi/revel-mongo-api**](https://github.com/mrpatiwi/revel-mongo-api)

## Preparation

* Make sure you have Go installed and your `$GOPATH` is set.
* You will need [Docker-Machine](https://docs.docker.com/machine/) if you are on OSX.
* Have [Docker](https://www.docker.com/) and [Docker-Compose](https://docs.docker.com/compose/) installed.
* [MongoDB](https://www.mongodb.org/) installed.

## Revel

You can follow the official steps on [revel.github.io](https://revel.github.io), but in short words it goes like this:

Let's install the basic dependencies:

```sh
go get github.com/revel/revel
go get github.com/revel/cmd/revel
```

> **From here and forward:**
>
> *   **Replace `mrpatiwi` with your Github username**
> *   **Replace `revel-mongo-api` with you app name**

Generate our app:

```sh
revel new github.com/mrpatiwi/revel-mongo-api
```

```sh
cd $GOPATH/src/github.com/mrpatiwi/revel-mongo-api
```

To start our app and see if it's working:

```sh
revel run github.com/mrpatiwi/revel-mongo-api
```

{{% figure src="/images/reveldocker-helloworld.png"%}}

## Installing Godep

You can find project's repository at: [github.com/tools/godep](https://github.com/tools/godep).

To install:

```sh
go get github.com/tools/godep
```

This basically manage your project dependencies saving in a file `Godeps/Godeps.json` all the necessary information.

Every time we require a new Go dependency we have to save it with:

```sh
godep save ./app
```

Make sure to ignore the directory: `Godeps/_workspace`:

```sh
# Append this to your .gitignore
Godeps/_workspace
```

When someone clones a clean version of your project, they will not have the dependencies installed. To do so:

```sh
godep go install ./app
```

Make sure to document that 👍

## Docker

We must put a `Dockerfile` on the root of our project:

```dockerfile
FROM golang:1.5.2

# Move current project to a valid go path
COPY . /go/src/github.com/mrpatiwi/revel-mongo-api
WORKDIR /go/src/github.com/mrpatiwi/revel-mongo-api

# Install Revel CLI
RUN go get github.com/revel/cmd/revel

# Install project dependencies
RUN go get github.com/tools/godep
RUN godep go install ./app

# Run app in production mode
EXPOSE 9000
ENTRYPOINT revel run github.com/mrpatiwi/revel-mongo-api prod 9000

```

To make deploying easier, we will use Docker-Compose. This will be even more useful when we integrate our app with MongoDB. We create a file named `docker-compose.yml` with:

```yml
# docker-compose.yml

web:
  build: .
  restart: always
  ports:
    - '80:9000'
```

To build and run our project:

```sh
# Run in 'detached' (background) mode
docker-compose up -d
# Building web
# Step 1 : FROM golang:1.5.2
# ...
# Step 8 : ENTRYPOINT revel run github.com/mrpatiwi/revel-mongo-api prod 9000
#  ---> Running in 04dc37d33f14
#  ---> ef842993944f
# Removing intermediate container 04dc37d33f14
# Successfully built ef842993944f
# Creating revelmongoapi_web_1
```

{{% figure src="/images/reveldocker-helloworld2.png"%}}

> `192.168.99.100` is the IP of my local machine ran by Docker-Machine.

See the logs:

```sh
docker-compose logs
# Attaching to revelmongoapi_web_1
# web_1 | ~
# web_1 | ~ revel! http://revel.github.io
# web_1 | ~
# web_1 | Listening on :9000...
```

Stop and remove:

```sh
docker-compose stop && docker-compose rm -f
# Stopping revelmongoapi_web_1 ... done
# Going to remove revelmongoapi_web_1
# Removing revelmongoapi_web_1 ... done
```

## MongoDB

When developing, install this database in your computer. If you are on OSX it's easy with `brew`:

```sh
brew install mongodb
```

On *production* environment we will use MongoDB Docker Image.

Back to our project, let's use [go-mgo/mgo](https://labix.org/mgo) as our database driver.

```sh
go get gopkg.in/mgo.v2
```

### Model

Just to exemplify, we will create a `Book` model:

```go
// app/models/book.go

package models

import "gopkg.in/mgo.v2/bson"

/*
Book model
*/
type Book struct {
	ID    bson.ObjectId `json:"_id,omitempty" bson:"_id,omitempty"`
	Title string        `json:"title" bson:"title"`
	Pages int           `json:"pages" bson:"pages"`
}
```

Note the marshaling attributes:

*   `json`: How we want the key when encoding and decoding from and to JSON. The attribute `omitempty` hide the field when it is empty.
*   `bson`: How we want the MongoDB key. The attribute `omitempty` same as above.

### Database connection

We have to setup our database, so we will crate a package named `database` in charge of this:

```go
// app/database/setup.go

package database

import "gopkg.in/mgo.v2"

/*
Database session
*/
var Session *mgo.Session

/*
Book's model connection
*/
var Books *mgo.Collection

/*
Init database
*/
func Init(uri, dbname string) error {
	session, err := mgo.Dial(uri)
	if err != nil {
		return err
	}

	// See https://godoc.org/labix.org/v2/mgo#Session.SetMode
	session.SetMode(mgo.Monotonic, true)

	// Expose session and models
	Session = session
	Books = session.DB(dbname).C("books")

	return nil
}
```

Remember to use *Upper Camel Case* to make a variable, function or struct **public outside the package**.

#### Settings

Our database have at least two environment: *production* and *development*, so we need different setups for each one of this.

First, we will modify `conf/app.conf` and set how we *dial* the database in *development*:

```conf
# conf/app.conf

# ...

################################################################################
# Section: dev
# This section is evaluated when running Revel in dev mode. Like so:
#   `revel run path/to/myapp`
[dev]
# This sets `DevMode` variable to `true` which can be used in your code as
#   `if revel.DevMode {...}`
#   or in your templates with
#   `<no value>`
mode.dev = true

# ...

# Database
database.uri  = "mongodb://localhost:27017"  # <- HERE!
database.name = "revelapp"                   # <- HERE!
```

Now we need to init our database inside `app/init.go` using `revel.OnAppStart(...)`:

```go
// app/init.go
package app

import (
	"github.com/mrpatiwi/revel-mongo-api/app/database"
	"github.com/revel/revel"
)

func init() {
	revel.Filters = []revel.Filter{
		revel.PanicFilter,
		// ...
		revel.ActionInvoker,
	}

	// Startup
	revel.OnAppStart(InitDB)
}

/*
InitDB to connection to database
*/
func InitDB() {
	// The second argument are default values, for safety
	uri := revel.Config.StringDefault("database.uri", "mongodb://localhost:27017")
	name := revel.Config.StringDefault("database.name", "revelapp")
	if err := database.Init(uri, name); err != nil {
		revel.INFO.Println("DB Error", err)
	}
}

var HeaderFilter = func(c *revel.Controller, fc []revel.Filter) {
	c.Response.Out.Header().Add("X-Frame-Options", "SAMEORIGIN")
	c.Response.Out.Header().Add("X-XSS-Protection", "1; mode=block")
	c.Response.Out.Header().Add("X-Content-Type-Options", "nosniff")

	fc[0](c, fc[1:]) // Execute the next filter stage.
}
```

### Routes

Setup the endpoints in `conf/routes`, then we will code the controller:

```txt
# conf/routes

# Routes
# This file defines all application routes (Higher priority routes first)
# ~~~~

module:testrunner

GET     /                                       App.Index
GET     /books/                                 Books.Index
POST    /books/create                           Books.Create
GET     /books/:id                              Books.Show

# ...
```

### Controller

Create a file at: `app/controllers/books.go`:

```go
// app/controllers/books.go

package controllers

import (
	"encoding/json"
	"io/ioutil"
	"log"
	"net/http"

	"github.com/mrpatiwi/revel-mongo-api/app/database"
	"github.com/mrpatiwi/revel-mongo-api/app/models"
	"github.com/revel/revel"
	"gopkg.in/mgo.v2/bson"
)
```

Declare this is a controller named `Books`:

```go
/*
Books controller
*/
type Books struct {
	*revel.Controller
}
```

#### POST /books/create

To have some data to show, first we will create the handler controller for `POST /books/create`:

```go
/*
Create book
*/
func (c Books) Create() revel.Result {
	book := &models.Book{}
	if body, err := ioutil.ReadAll(c.Request.Body); err != nil {
		return c.RenderText("bad request")

	} else if err := json.Unmarshal(body, book); err != nil {
		return c.RenderText("could not parse request")

	} else if err := database.Books.Insert(book); err != nil {
		// Internal Server Error
		log.Fatal(err)
		c.Response.Status = http.StatusInternalServerError
		return c.RenderText("could not be saved")
	}
	c.Response.Status = http.StatusCreated
	return c.RenderJson(book)
}
```

We can test it using `curl` from the command line:

```sh
curl -H "Content-Type: application/json" \
  -X POST -d '{ "title": "Animal Farm", "pages": 100 }' -i \
  http://localhost:9000/books/create
```

Response:

```txt
HTTP/1.1 201 Created
Content-Length: 44
Content-Type: application/json; charset=utf-8
Date: Wed, 30 Dec 2015 00:52:24 GMT
Set-Cookie: REVEL_FLASH=; Path=/
X-Content-Type-Options: nosniff
X-Frame-Options: SAMEORIGIN
X-Xss-Protection: 1; mode=block

{
  "title": "Animal Farm",
  "pages": 100
}
```

#### GET /books

Let's create a Index where we will return every book in the database:

```go
/*
Index of all books
*/
func (c Books) Index() revel.Result {
	results := []models.Book{}
	if err := database.Books.Find(bson.M{}).All(&results); err != nil {
		// Internal Server Error
		log.Fatal(err)
	}
	return c.RenderJson(results)
}
```

Let's see our created book:

```sh
curl -i http://localhost:9000/books
```

Response:

```txt
HTTP/1.1 200 OK
Content-Length: 277
Content-Type: application/json; charset=utf-8
Date: Wed, 30 Dec 2015 00:55:38 GMT
Set-Cookie: REVEL_FLASH=; Path=/
X-Content-Type-Options: nosniff
X-Frame-Options: SAMEORIGIN
X-Xss-Protection: 1; mode=block

[
  {
    "_id": "56832ac8c52f68da8cae6818",
    "title": "Animal Farm",
    "pages": 100
  }
]
```

#### GET /books/:id

To show a individual Book:

```go
/*
Show particular book
*/
func (c Books) Show(id string) revel.Result {
	result := models.Book{}
	if !bson.IsObjectIdHex(id) {
		c.Response.Status = http.StatusBadRequest
		return c.RenderText("id is not valid")

	} else if obj := bson.ObjectIdHex(id); !obj.Valid() {
		c.Response.Status = http.StatusBadRequest
		return c.RenderText("id is not valid")

	} else if err := database.Books.Find(bson.M{"_id": obj}).One(&result); err != nil {
		// Internal Server Error
		log.Fatal(err)
	}
	return c.RenderJson(result)
}
```

We can get the `id` from the previous request. So:

```sh
curl -i http://localhost:9000/books/56832ac8c52f68da8cae6818
```

Response:

```txt
HTTP/1.1 200 OK
Content-Length: 81
Content-Type: application/json; charset=utf-8
Date: Wed, 30 Dec 2015 00:57:22 GMT
Set-Cookie: REVEL_FLASH=; Path=/
X-Content-Type-Options: nosniff
X-Frame-Options: SAMEORIGIN
X-Xss-Protection: 1; mode=block

{
  "_id": "56832ac8c52f68da8cae6818",
  "title": "Animal Farm",
  "pages": 100
}
```

### MongoDB Image

On production, we need a container running MongoDB. We have to modify our `docker-compose.yml`:

```yml
web:
  build: .
  restart: always
  links:
    - mongo
  ports:
    - '80:9000'

mongo:
  image: mongo
```

Back to our `conf/app.conf` file, we have to add `mongo` as our host running the database:

```conf
# conf/app.conf

# ...

################################################################################
# Section: dev

# ...

################################################################################
# Section: prod
# This section is evaluated when running Revel in production mode. Like so:
#   `revel run path/to/myapp prod`
# See:
#  [dev] section for documentation of the various settings
[prod]
mode.dev = false

# ...

# Database
databaseuri  = "mongodb://mongo:27017"
databasename = "revelapp"
```

### Last details

We add the new dependencies to `Godep/Godep.json` with:

```sh
godep save ./app
```

## Run it

This is the most simple setup, but it is a good starting point.

To launch our app:

```sh
docker-compose up -d
```

# Conclusions

Revel is a well-thinked framework, although still in beta.

Left pending in this post:

*   Remove unused middleware, if we want to develop an API, we do not need cookies or HTML rendering.


## Pros:

*   A true framework. It calls you, not not the other way around.
*   Excellent routing system.
*   It is focused on full-stack web apps. I have to try Meteor.js and compare it to Revel before replacing Ruby on Rails 😄
*   Config file with environments 👌

## Cons:

*   Manual database integration, but they [have something in plans](https://revel.github.io/#Wishlist).
*   You must start the app with Revel CLI, this can cause some integration problems.
*   Does it scale? 📈
