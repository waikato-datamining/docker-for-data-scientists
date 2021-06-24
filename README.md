# docker-for-data-scientists

From one data scientist to another on how to utilize docker to make your life easier.

You can view the auto-generated documentation here:

https://fracpete.github.io/docker-for-data-scientists/


## Installation

*mkdocs* works with Python 2.7 and 3.x.

Best approach is to install mkdocs (>= 0.16.0) in a virtual environment 
(`venv` directory):

* Python 3.7

  ```
  virtualenv -p /usr/bin/python3.7 venv
  ```

* Install the mkdocs package

  ```
  ./venv/bin/pip install mkdocs
  ```


## Content

In order for content to show up, it needs to be added to the configuration, 
i.e., in the `pages` section of the `mkdocs.yml` file.

Some pointers:

* [Images and media](http://www.mkdocs.org/user-guide/writing-your-docs/#images-and-media)
* [Linking](http://www.mkdocs.org/user-guide/writing-your-docs/#linking-documents)
* [mkdocs.yml](http://www.mkdocs.org/user-guide/configuration/)


## Build

[mkdocs](http://www.mkdocs.org/) is used to generate HTML from the 
markdown documents and images:

```
./venv/bin/mkdocs build --clean
```


## Testing

You can test what the site looks like, using the following command
and opening a browser on [localhost:8000](http://127.0.0.1:8000):

mkdocs monitors setup and markdown files, so you can just add and edit
them as you like, it will automatically rebuild and refresh the browser.

```
./venv/bin/mkdocs build --clean && ./venv/bin/mkdocs serve
```


## Deploying

You can deploy the current state to Github pages with the following command:

```
./venv/bin/mkdocs gh-deploy --clean
```

