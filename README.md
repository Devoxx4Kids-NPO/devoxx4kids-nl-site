# devoxx4kids-nl-site

This repository is used to generate the website for **Stichting Devoxx4Kids Nederland**.
A static website is generated with hugo. The content of the static website is pushed to 
branch *gh-pages* of the repository *Devoxx4kids-NPO.github.io*

## running locally

To run the website locally ***hugo*** must be installed.
More information for the installation can be found [here](https://gohugo.io/installation/)

You can run the website while developing with the following command: ```hugo server```

## events

To register a new event create a new file in the [content/events/](content/events/.) folder.
Use the following syntax for the file name : ```yyyymmdd-<city-of-event>-<companyName>.md```.
Store images for workshops in the folder [static/images/events/](static/images/events/.). 
Try to use the same naming convention as with the md-file.

For events the following parameters can be used in the md-file.

| Parameter  | required | description                                          |
|------------|----------|------------------------------------------------------|
| title      | yes      | Title of the event                                   |
| summary    | yes      | Short summary of the event used in lest of events    |
| date       | yes      | Date the event was registerd on the website          |
| eventDate  | yes      | Actual date of the event                             |
| author     | no       | Name of the person who registered the event          |
| category   | yes      | fixed text 'events'                                  |
| image      | no       | Image used in the detail page of the event           |
| imageWidth | no       | Width of the image as a percentage. Defaults to 100. | 
| thumbnail  | no       | Image of the event used in the list of events        |


The eventDate is used to discover upcoming or recent events. 
Since Hugo is generating a static site the events are not update dynamically.
To update the events the site must be generated again.
This can be done by running the GitHub action ```Deploy Hugo site```.


## workshops (gastlessen)

The menu ***gastlessen*** is holding information for workshops. 
At the end of the page a list of workshops is listed.
To register a new workshop create a new file in the [content/posts/gastlessen/](content/posts/gastlessen/.) folder.
Use the following syntax for the file name : ```yyyymmdd-<city-of-workshop>.md```.
Store images for workshops in the folder [static/images/posts/gastlessen/](static/images/posts/gastlessen/.).

For workshops the following parameters can be used.

| Parameter | required | description                                    |
|-----------|----------|------------------------------------------------|
| Title     | yes      | Title of the workshop                          |
| date      | yes      | Date of the workshop                           |
| author    | no       | Name of the person how registered the workshop |
| category  | yes      | fixed text 'gastlessen'                        |
| image     | no       | Image used in the detail page of the workshop  |
