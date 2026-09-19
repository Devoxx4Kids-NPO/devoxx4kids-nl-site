+++
date = '{{ .Date }}'
draft = true
title = '{{ replace .File.ContentBaseName "-" " " | title }}'
summary = ''
eventDate = ''
city = ''
host = ''
startTime = ''     # e.g. '09:30'
endTime = ''       # e.g. '16:00'
ages = '8 t/m 14 jaar'
contact = ''       # e-mail for questions, optional
registration = ''
# status: open | vol | binnenkort (leave empty when unknown)
status = ''
# module file names from content/modules/, e.g. ['mbot2', 'microbit', 'scratch']
modules = []
# day schedule, written to the child; aim for 5-7 steps
# [[programma]]
# tijd = '09:30'
# titel = 'Ontvangst'
# tekst = 'Je komt binnen en zoekt je plek.'
author = 'svermeer'
category = 'events'
flyer = ''         # organiser's flyer, e.g. '/images/events/20261107-Apeldoorn-Kadaster.png'
photo = ''         # photo taken at the event (with consent)
thumbnail = '/images/events/Verwacht-D4k-event.gif'

# tables go last in TOML
[location]
name = ''
note = ''          # optional, e.g. 'Studio N'
address = ''       # 'Straat 12'
postcode = ''      # '1234 AB'
+++
