---
layout: page
title: "reminders and schedules"
---

# Reminders and Schedules

Reminders and Schedules are generated for various services for ANC and Child services. For example, when a new mother is
registered, the mother is enrolled for the ANC Visits, Hb Tests and TT doses. Similarly, when a new child is registered,
the child is enrolled for various immunizations like BCG, OPV etc.

Some of these reminders are dependent on each other. For example, when the mother does her first ANC Visit, the ANC
visit 1 is fulfilled and then the mother is enrolled for the second ANC visit.

The workflow of the reminders happens as follows. For reminders, let us take ANC Registration as an example:

* An ANC Registration form is filled and saved
* The ziggy on the app creates the corresponding mother entity
* The form gets submitted to the server
* The server processes the form and creates the corresponding mother entity on the CouchDB
* The server then enrols this mother to the list of services like (ANC Visit1, Hb/Test, TT dose)
* The reminders are generated at a stipulated time at the server (4 pm. in case of Dristhi) 
* These reminders are pushed to the app the next time the app syncs with the server
* These reminders get displayed in the app with a particular colour which corresponds to how soon is the reminder due

The reminders are colour coded as follows:

* Light Blue - The reminder is in the early window, which means that it is due but not anytime soon
* Dark Blue - The reminder is in the due window, the due date of the reminder/service is close
* Red - The reminder is in the late window, the due date for the reminder/service is already past


