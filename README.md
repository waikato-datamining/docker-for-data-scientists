# docker-for-data-scientists

From one data scientist to another on how to utilize docker to make your life easier.

You can view the auto-generated documentation here:

https://www.data-mining.co.nz/docker-for-data-scientists/


## Installation

Best approach is to install mkdocs in a virtual environment (`venv` directory):

* Python 3

  ```
  virtualenv -p /usr/bin/python3 venv
  ```

* Install mkdocs and dependencies

  ```
  ./venv/bin/pip install mkdocs==1.6.1 jinja2==3.1.6 Markdown==3.10.2 MarkupSafe==3.0.3 mkdocs-material==9.7.6 mkdocs-material-extensions==1.3.1 Pygments==2.20.0 pymdown-extensions==10.21.2 click==8.2.1
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

Any push will trigger a rebuild of the site on github via github actions:

[.github/workflows/main.yml](.github/workflows/main.yml)
