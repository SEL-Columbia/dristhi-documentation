---
layout: page
title: "Smart Registers"
---

# Smart Registers

All the clients that the ANM serves are displayed in Smart Registers.

## Structure of a Smart Register

![][smart_register_structure_diagram]
[Structure of Smart Register][1]

### Navigation Bar

* **Back**  
  When tapped navigates to previous screen.  
* **Title**  
  Displays the title of the current register that is open. For example EC is the title of EC Register. When tapped navigates to previous screen.  
* **Report Month**  
  Displays the start and end date of current report month. When tapped navigates to previous screen.  
* **Service Mode**  
  Displays the current service mode. When tapped all the available service modes are displayed
* **Sort**  
  When tapped displays all the available sort options.  
* **Filter**  
  When tapped displays all the available filter options.  
* **Register (Plus icon)**  
  When tapped opens a registration form that is appropriate to the register. For example, on EC Register this will open EC Registration form, where as on ANC Register this will open ANC Registration OA form.  
* **Search**  
  This allows the user to search for clients. On EC Register this searches in wife name, EC number fields.      

### Status Bar

* **Applied Sort**  
  Displays the current sort that is applied.  
* **Applied Filter**  
  Displays the current filter that is applied.    

### Client list header

This displays the column headers of client list.

### Client list

This displays the client list. The left part of the list has identification information, like photo, name, EC number, etc. The right part of the list has health content. The health content is displayed according to the currently selected service mode.   

### Pagination Bar

At any given time, only twenty clients are shown per page. Pagination Bar shows the current page that is displayed and the total number of pages. It also allows the user to navigate the client list using **Previous** and **Next** buttons.  


[1]: {{root_url}}/images/custom/dristhi_app/smart_register_structure.png
[smart_register_structure_diagram]: {{root_url}}/images/custom/dristhi_app/smart_register_structure.png "Structure of Smart Register"