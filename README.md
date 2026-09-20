# devoxx4kids-nl-site

This repository is used to generate the website for **Stichting Devoxx4Kids Nederland**.
A static website is generated with hugo. The content of the static website is pushed to 
branch *gh-pages* of the repository *Devoxx4kids-NPO.github.io*

## running locally

To run the website locally ***hugo*** must be installed.
More information for the installation can be found [here](https://gohugo.io/installation/)

You can run the website while developing with the following command: ```hugo server```

## events

To register a new event create a new file in the [content/events/](content/events/.) folder,
for example with ```hugo new content/events/yyyymmdd-<city>-<companyName>.md``` (uses [archetypes/events.md](archetypes/events.md)).
Use the following syntax for the file name : ```yyyymmdd-<city-of-event>-<companyName>.md```.
Store images for workshops in the folder [static/images/events/](static/images/events/.). 
Try to use the same naming convention as with the md-file.

For events the following parameters can be used in the md-file.
Everything practical (time, location, registration, ages, modules, day schedule) goes in these parameters, not in the text:
the event page builds its header, module cards and schedule from them. The text below the front matter is only for anything else a parent needs to know.

| Parameter  | required | description                                          |
|------------|----------|------------------------------------------------------|
| title      | yes      | Title of the event                                   |
| summary    | yes      | One to three sentences shown in the orange header of the event page; write it to the child |
| date       | yes      | Date the event was registerd on the website          |
| eventDate  | yes      | Actual date of the event                             |
| author     | no       | Name of the person who registered the event          |
| category   | yes      | fixed text 'events'                                  |
| flyer      | no       | Flyer made by the organiser (in `static/images/events/`); shown in its own section with a button to open it |
| photo      | no       | A photo taken at the event, shown under "Een foto van de dag". Only with consent for recognisable children |
| photoWidth | no       | Width of the photo as a percentage. Defaults to 100. |
| thumbnail  | no       | Image of the event (not shown by the current design) |
| city       | no       | City of the event; card title becomes "Devoxx4Kids <city>" |
| host       | no       | Organising company, shown on the event card          |
| location   | no       | Venue, shown in the page header with a route link: `name`, optional `note` (e.g. "Studio N"), `address` ("Straat 12"), `postcode` ("1234 AB"). The city comes from `city` |
| startTime  | no       | Start time, e.g. "09:30"                              |
| endTime    | no       | End time, e.g. "16:00"                                |
| contact    | no       | E-mail address for questions about this event         |
| ages       | no       | Age range, written as "8 t/m 14 jaar"                |
| registration | no     | Sign-up URL; shows an "Aanmelden" button while upcoming |
| status     | no       | `open`, `vol` or `binnenkort`; shown as a badge. Only set when known |
| modules    | no       | List of module file names from `content/modules/`, e.g. `[mbot2, microbit, scratch]`; shown as module cards that link to the module page |
| programma  | no       | The day schedule as a list of `tijd`, `titel`, `tekst`; shown as a step-by-step timeline. Write `tekst` to the child ("je"), aim for 5–7 steps |


The eventDate is used to discover upcoming or recent events. 
Since Hugo is generating a static site the events are not update dynamically.
To update the events the site must be generated again.
This can be done by running the GitHub action ```Deploy Hugo site```.


## modules

Every module (workshop) has its own page in [content/modules/](content/modules/.), e.g. `content/modules/mbot2.md`.
The file name is the module's id: events list it under `modules`, and the module page shows every event that used it.
The modules page (`/modules/`) shows all modules as a grid of cards; the home page shows the first six.
Create a new one with ```hugo new content/modules/<id>.md``` (uses [archetypes/modules.md](archetypes/modules.md)).

| Parameter    | required | description                                                                 |
|--------------|----------|-----------------------------------------------------------------------------|
| title        | yes      | Title written to the child, e.g. "Laat je robot rijden"                      |
| tool         | yes      | The real tool name, spelled exactly: `mBot2`, `micro:bit`, `Scratch`          |
| summary      | yes      | 1–3 short sentences to the child ("je"); shown on the card and the page      |
| image        | yes      | Illustration file in [static/images/illustrations/](static/images/illustrations/.) |
| weight       | no       | Order on the modules page                                                    |
| requirements | no       | What an organiser or teacher needs (list of sentences)                       |
| links        | no       | Lesson material and websites, as a list of `title` + `url`                   |

The text below the front matter is optional extra information, shown on the module page.

Illustrations are cartoon SVGs, 240×180, in the brand colours with a thick black outline, like the existing ones.
Draw one for every new module rather than using a product photo; the rules are in the design system (Illustrations group).

## workshops (gastlessen)

The menu ***gastlessen*** is the page for schools (Devoxx4Kids@School). It is built by `layouts/posts/gastlessen/list.html`:
an orange header with a request button, the three promises, module cards, the steps to request a guest lesson and the earlier guest lessons.
In [content/posts/gastlessen/_index.md](content/posts/gastlessen/_index.md) you set the header text (`summary`), the contact address (`contact`)
and which modules are shown (`modules`, file names from `content/modules/`).
The promises on this page follow the design system's Devoxx4Kids@School rules; don't add other promises (class size, dates, number of lessons).
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

## stichting and partners

These are the formal pages: a plain header (no orange band), flat cards and a facts panel.

- The foundation's details (address, phone, e-mail, KvK, RSIN, IBAN, board members and the documents shown on the page)
  live in [data/stichting.yml](data/stichting.yml). The text of the page is in [content/pages/stichting-d4k.md](content/pages/stichting-d4k.md) (`layout: stichting`).
- Partners are listed in [data/partners.yml](data/partners.yml), grouped (Donateur, Communities, Bedrijven).
  Per partner: `name`, optional `logo` (in `static/images/partners/`), `page` (internal link) or `url` (website) and `note`.
  The page itself is [content/pages/partners.md](content/pages/partners.md) (`layout: partners`).
  Logos are drawn in a fixed-height box on a white plate and scaled to fit (`object-fit: contain`), so a square badge
  and a wide wordmark end up the same size and a small raster is never blown up past its own resolution — any shape
  works, no need to trim or pad the file first. A partner without a `logo` shows its name as a wordmark instead.
- The ANBI page ([content/pages/anbi.md](content/pages/anbi.md), `layout: anbi`) is built from [data/anbi.yml](data/anbi.yml)
  (goal, activities, finance per year, policy plan, …) plus the shared details in `data/stichting.yml`.
  Add a new year at the top of `finance`; a document that is not published is left out and shows as a dash.
- The Over ons page ([content/pages/overons.md](content/pages/overons.md), `layout: overons`) shows what we do, the map of
  event locations from [data/d4k_events.yml](data/d4k_events.yml) and the text of the page.


## videos

The Videos page lives at `/events/videos/` and sits in the menu as a submenu of Events.

- The list of videos is in [data/videos.yml](data/videos.yml), newest first — the order in that file is the order on the
  page. Per video: `youtube` (the id from the YouTube URL, the part after `/embed/` or `?v=`), `title`, optional `kind`
  (a badge such as "Reportage" or "Aankondiging"), `date` (`YYYY-MM-DD`), `city`, `host` and `text`.
- The page itself is [content/videos.md](content/videos.md) (`layout: videos`, with `url: /events/videos/` so it nests
  under Events); the template is [layouts/_default/videos.html](layouts/_default/videos.html).
- Videos are embedded through `youtube-nocookie.com`, so YouTube only sets a cookie once a visitor presses play.

## menu and submenus

The navigation is defined in [hugo.toml](hugo.toml) under `[[menu.main]]`. To hang a page under an existing item, give
the parent an `identifier` and point the child at it with `parent`:

```toml
[[menu.main]]
  identifier = "events"
  name = "Events"
  url = "/events/"
  weight = 2
[[menu.main]]
  identifier = "videos"
  parent = "events"
  name = "Video's"
  url = "/events/videos/"
  weight = 1
```

Below 960px the whole menu folds behind a hamburger button and submenus are shown unfolded inside the panel; above it
the submenu is a dropdown that opens on hover or with the caret button next to the parent link.
