---
layout: page
title: "How to update forms on Dristhi app"
---

# How to update forms on Dristhi app
1. Find the id of the form that needs to be updated. This can be done by locating the form at http://formhub.org/drishti_forms/
2. Find the name of the form on Dristhi app. This can be done by locating the form at <dristhi_app_root>/dristhi-app/assets/www/form directory
3. Run the following command by replacing <form_id> with the actual form_id, ``curl -X POST -ssl3 -d "server_url=https://formhub.org/drishti_forms&form_id=<form_id>" https://enketo.formhub.org/transform/get_html_form``
4. Copy the *model* node contents to <dristhi_app_root>/dristhi-app/assets/www/form/<form_name>/model.xml and the *form* node contents to <dristhi_app_root>/dristhi-app/assets/www/form/<form_name>/form.xml
5. Update the form_definition.json according to the changes made to *model*
