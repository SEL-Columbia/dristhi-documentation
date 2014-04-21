---
layout: page
title: "How to add new forms to Dristhi App"
---

# How to add new forms to Dristhi App
1. Forms are authored according to XLSForm standard (using Microsoft Excel)
2. These forms are then uploaded to http://formhub.org/ which converts them into XForm standard. All Dristhi forms can be found at http://formhub.org/drishti_forms/ (this requires login)
3. XForm is then converted to Enketo HTML form by running the following command with the <form_id> replaced with the actual value, ``curl -X POST -ssl3 -d "server_url=https://formhub.org/drishti_forms&form_id=<form_id>" https://enketo.formhub.org/transform/get_html_form``.
4. This generates a xml document which contains two nodes, *model* and *form*, rest of the nodes can be ignored. *model* is the XForm Model instance, this is used by Enketo to do all the validations and calculations in the forms. *form* is the HTML DOM which Enketo uses to render the form
5. *model* and *form* are saved as *model.xml* and *form.xml*. These files are then copied to the location <dristhi_app_root>/dristhi-app/assets/www/form/<form_name>/model.xml and <dristhi_app_root>/dristhi-app/assets/www/form/<form_name>/form.xml. When a form is launched, form_name is passed in the query parameter. Using this Enketo renders the form
6. Another important part of form is form_definition.json file. This is a mapping file that is written for every form which maps the fields within the form to Dristhi database. More about form_definition.json later
7. So *model.xml*, *form.xml* and *form_definition.json* comprise a Enketo Form
