

# how to make website with gin


I love Gin. cuz it's too simple, I think a lot of ppls wonders what some libraries should be used.
I'm not specialist but tried to make some which are working as productions. I'd like to make this repo to explain how to make website with my experiences for beginners.

## branches

This repo has some branches. would like to keep some branches simple to explain.

- master => active repo with all stuff.
- [x] hello => almost pure gin with assets/glide.
- [x] gzip
- [x] templates => using ezgintemplate, it's supporting switching layouts.
- [ ] flash => sample of flash messages with templates.
- [ ] csrf => supporting csrf, with flash/templates.
- [ ] oauth/admin => google oauth sample for admin pages.
- [ ] cors => cors/JWT sample for APIs.
- [ ] login => login/logout sample, using gorm (db library), with csrf/templates/flash.
- [ ] json => parse json data and showing.
- [ ] docker => docker sample with alpine.
- [ ] cache => using memcached for json. it's including docker-compose sample and json/Unmarshall.
- [ ] deploy => deply sample to ElasticBeanstalk with CircleCI.

-----

## how to start Go

### install Go

You can install Go by Go official website or homebrew.
You might want to manage some Go versions by gvm. It's up to you.


### setup GOPATH

If you'd like to put repository on

> ~/src/github.com/XXX

you should setup GOPATH in .zshrc ( or .profile or .bashrc ) as below.

```
export GOPATH=$HOME
PATH=$PATH:$GOPATH/bin/
```

The path is very important in terms of importing libraries.

### setup glide

Some must wonder how to manage versioning. Some Go libraries are very fast, so Go support versioning.
Go loads
- vendor
firstly, then
- GOPATH
etcetc..

This mean, if you want to use specific versions of libraries, you can put the libraries under vender dir.

But I think it's not enough to manage versions. I strongly recommend to use some managers, like glide.

you should install glide firstly.

All branches have glide.yaml, so you might need to do
```
glide up
```

Then, you can download specific versions under vendor dir.

However, the repo includes vendor dir. so you might not need to `glide up`.

If you want to add more libraries, you can just do
```
glide get LIBRARY
```
instead of
```
go get LIBRARY
```




### setup ATOM with go-plus

I'm using ATOM editor with go-plus.


### install fresh

Go is compile language but you can use it as LL as follow.

```
go run *.go
```

You still need to re-run after rewrite your code.

To avoid re-run, you can use fresh. It enable you to live-reloading.

To install fresh, you can do
```
go get github.com/pilu/fresh
```



-----

## Hello Gin ( hello branch )

It's just `Hello Gin`. please have a look at the branch (hello).

### logging

`log.Print` & `log.Printf` are useful. and adding below, you can see line where you show the log.

```main.go
func init() {
	log.SetFlags(log.Lshortfile)
}
```


### start gin with static assets


```main.go

func main() {
	router := gin.Default()
	router.Static("/assets", "./assets")
	router.StaticFile("/favicon.ico", "./assets/favicon.ico")
	router.StaticFile("/robots.txt", "./assets/robots.txt")
	router.Use(gin.Recovery())
	router.GET("/", controllers.AppIndex)
	router.Run(":3000")
}

```

```app/controllers/app.go
package controllers

import "gopkg.in/gin-gonic/gin.v1"

//AppIndex ...
func AppIndex(c *gin.Context) {
	c.String(200, "hello gin")
}
```

you can just use `fresh`, then you can access http://localhost:3000/



----

## support gzip ( gzip branch )

### import gzip & setting

you should import `"github.com/gin-contrib/gzip"` in main.go, then,

```main.go
router.Use(gzip.Gzip(gzip.DefaultCompression))
```

You can check size of http://localhost:3000/assets/css/bootstrap/4.0.0-alpha.6/css/bootstrap.min.css

-----

## swithcing layouts ( templates branch )

### import ezgintemplate

import `"html/template"` and `"github.com/rakd/gin_sample/app/libs/ezgintemplate"`.

`"github.com/rakd/gin_sample/app/libs/ezgintemplate"` is originally https://github.com/michelloworld/ez-gin-template, customized by kaz. I wanted to switch layout for AMP/Admin/regular pages.


### prepare app/views

under app/views, you should prepare some directories/templates as follwoing.
- layouts/
 - admin.tmpl
 - base.tmpl
 - amp.tmpl
- partials
 - nav.tmpl
- errors
 - 404.tmpl
- app ( one of controllers )
 - index.tmpl

### prepare common logic for controllers and customize App controller.

prepare
- app/controllers/common.go
and edit
- app/controllers/app.go

### prepare admin controller

now, there is no authentication for admin. it's just sample to switch layout.

- app/controllers/admin.go

you can see
-  http://localhost:3000/admin/ with admin layout.

### prepare amp template for AppIndex

- app/views/app/index_amp.tmpl

then you can see AMP page on http://localhost:3000/?amp=1 

My ezgintemplate regard XXX_amp.tmp as tempalte for AMP.


### prepare errors.go and NoRoute in main.go

```main.go
router.NoRoute(controllers.NoRoute)
```

### prepare hoge/index.tmpl without controller

- app/views/about/index.tmpl
- app/views/hoge/index.tmpl
- app/views/search/index.tmpl

there are no controllers for above template. but you can see,
- http://localhost/about
- http://localhost/hoge
- http://localhost/search

If no template, you can see 404 error.

-----

## flash messages ( flash branch )


-----


## support csrf ( csrf branch )


----


## support oauth for admin page ( oauth branch )



-----

## cors/JWT ( cors branch )

Some must want to use JWT/cors for APIs.


----


## login/logout ( login branch )

----

## JSON  ( json branch )



----

## docker ( docker branch )

---

## using memcached for JSON with docker-compose ( cache branch )


----

## deply sample with CircleCI/ElasticBeanstalk (deploy branch )
