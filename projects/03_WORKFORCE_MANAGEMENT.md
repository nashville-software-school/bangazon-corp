# Workforce Asset and Training Application

## Table of Contents

1. [Prerequisites](#prerequisites)
1. [What You Will Be Learning](#what-you-will-be-learning)
1. [Requirements](#requirements)
1. [Resources](#resources)

## Prerequisites
* Migrations
<!-- * [View templates/Layouts](https://docs.asp.net/en/latest/mvc/views/overview.html) -->
* Forms
* Controllers
* ERD development
* Express routing
<!-- * Yeoman, and the ASP.NET generator, [installed](#installing-yeoman-and-the-aspnet-generator) -->

## What You Will Be Learning

### Server Generated Application

All views in this application will be generated on the server, using templates, and delivered to the client as a complete HTML document.

### View Templates

You learned about view templates when you used Angular, but now your application server will be serving a fully built template to any client request instead of the client building the view.

### Layouts
To keep your code DRY between different view templates, you should create a standard layout with your `index.html` Pug template for the application so that the structure of each page is consistent. Define the general structure of your site in that template, then the individual page templates will be injected into the overall layout.

### Forms

Express has no built-in functionality for easily handling the payload, or request data, on its own, but there are npm modules you can install and include to take care of it. Look at `body-parser` as the most widely used example.

## Requirements

You will be building a fairly straightforward, server generated MVC application for performing CRUD operations on the following resources:

* Employees
* Departments
* Computers
* Training Programs

The following relationships must be established.

1. Over time, a computer can belong to many employees, but only one at a time.
1. An employee can attend many training programs.
1. A training program can have many employees attending.
1. Many employees can be assigned to a department, but an employees may only be in one at a time.

Produce the following views.

### Employee Views

1. List employees.
1. Create an employee, and assign to a department.
1. View employee, which shows their department, computer, and any training programs they are attending.
1. Edit employee, which should also allow the user to:
    1. Reassign the employee to a different department
    1. Change which computer they are using from a list of unused computers.
    1. Add and/or remove training programs that the employee is attending.

### Department Views

1. List departments.
1. Create a department.
1. Department details, which should show all employees in that department.

### Computer Views

1. List computers, and be able to delete a computer from the list view.
1. Create a computer.

### Training Program Views

1. List training programs.
1. Create training program.
1. View training program details. This should display any employees that are currently in the training program

### Common Views

1. Basic navigation bar that allows the user to view the list of each type of resource.
1. Footer containing the company name.

## Resources

### Sequelize Models and Migrations

Using the requirements above use Sequelize to create a [model](http://docs.sequelizejs.com/manual/tutorial/models-definition.html) for each resource, and use [migrations](link to sqlize docs) to ensure your database structure is up to date. Remember to start with a `.sequelizerc` file, and to run `sequelize init` to generate the needed directories and files.   
[Sequelize CLI docs](https://github.com/sequelize/cli)

### Templates

Getting started with [Pug](https://pugjs.org/api/getting-started.html)

### Familiar Friends
Pug, like Angular and Handlebars, has many built-in helper tags and filters when building the site templates. We strongly recommend reading this documentation while building your templates.  
+ [Iteration](https://pugjs.org/language/iteration.html)  
+ [Conditionals](https://pugjs.org/language/conditionals.html)  
+ [Includes](https://pugjs.org/language/conditionals.html)  
+ [Interpolation](https://pugjs.org/language/interpolation.html)
