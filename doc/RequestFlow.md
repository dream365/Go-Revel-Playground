The Request Flow
===================

We look at how Revel handles the HTTP request.

Routes
-------------

The first thing that Revel does is check the `conf/routes` file :

```GET    /    App.Index```

this tells Revel to invoke the **`Index`** method of the **`App`** controller when it receives a http **`GET`** request to **`/`**.

Controller Methods
-------------

Let's follow this call to the code, in **app/controllers/app.go**:

```
package controllers

import "github.com/revel/revel"

type App struct {
    *revel.Controller
}

func (c App) Index() revel.Result {
    return c.Render()
}
```

All controllers must be a `struct` that embeds a `*revel.Controller` in the first slot.
Any method on a controller that is exported and returns a `revel.Result` may be used as part of an **Action**, in the above example **App.Index** is the **Action**.

The Revel controller provides many useful methods for generating Results.
In this example, it calls `Render()`, which tells Revel to find and render a template as the response with http `200 OK`.

Templates
-------------

Templates are in the **app/views** directory. 
When an explicit template name is not specified, Revel look for a template matching the controller/method.
In this case, Revel finds the **app/views/App/Index.html** file, and renders it using the Template engine.

{% capture ex %}{% raw %}
{{set . "title" "Home"}}
{{template "header.html" .}}

<header class="jumbotron" style="background-color:#A9F16C">
  <div class="container">
    <div class="row">
      <h1>It works!</h1>
      <p></p>
    </div>
  </div>
</header>

<div class="container">
    <div class="row">
    <div class="span6">
        {{template "flash.html" .}}
    </div>
    </div>
</div>

{{template "footer.html" .}}
{% endraw %}{% endcapture %}
{% highlight htmldjango %}{{ex}}{% endhighlight %}

Beyond the functions provided by the Go templates package, Revel adds a few helpful ones also.

The template above : 

1. Adds a new **title** variable to the render context with set.
2. Includes the **header.html** templates, which uses the **title** variable.
3. Displays a welcome message.
4. Includes the **flash.html** template, which shows any flashed messages.
5. Includes the **footer.html**.

If you look at **header.html**, you can see some more template tags in action:

```
<!DOCTYPE html>

<html>
  <head>
    <title>{{.title}}</title>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link rel="stylesheet" type="text/css" href="/public/css/bootstrap-3.3.6.min.css">
    <link rel="shortcut icon" type="image/png" href="/public/img/favicon.png">
    <script src="/public/js/jquery-2.2.4.min.js"></script>
    <script src="/public/js/bootstrap-3.3.6.min.js"></script>
    {{range .moreStyles}}
      <link rel="stylesheet" type="text/css" href="/public/{{.}}">
    {{end}}
    {{range .moreScripts}}
      <script src="/public/{{.}}" type="text/javascript" charset="utf-8"></script>
    {{end}}
  </head>
  <body>
```

You can see the set `.title` being used, and also see that it accepts JS and CSS files included from calling templates in the **moreStyles** and **moreScripts** variables.

HOT-RELOAD
-------------

Revel has `watchers` that check for changes to files and recompiles as part of the development cycle.

To demonstrate this, change the welcome message.  In **Index.html**, change

`<h1>It works!</h1>`

to

`<h1>Hello Revel</h1>`

Refresh the browser, and you should see the change immediately! Revel noticed that your template changed and reloaded it.

Revel watches  - see [config](/manual/appconf.html#watchers):

* All go code under **app/**
* All templates under **app/views/**
* The routes file: **conf/routes**

Changes to any of those will cause Revel to update and compile the running app with the
latest change in code.  Try it right now: open **app/controllers/app.go** and introduce an error.

Change

`return c.Render()`

to

`return c.Renderx()`

Refresh the page and Revel will display a helpful error message:

![HelpfulError](https://revel.github.io/img/helpfulerror.png)

Lastly, let's pass some data into the template.

In **app/controllers/app.go**, change:

`return c.Renderx()`

to

```
greeting := "Aloha World"
return c.Render(greeting)
```

And in the **app/views/App/Index.html** template, change:

`<h1>Hello Revel</h1>`

to

`<h1>{{.greeting}}</h1>`

Refresh the browser and to see a Hawaiian greeting.

![AlohaWorld](https://revel.github.io/img/AlohaWorld.png)

***
#### [Home](https://dream365.github.io/Go-Revel-Playground)