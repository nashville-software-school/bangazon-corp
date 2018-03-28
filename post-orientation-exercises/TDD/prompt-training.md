## Instructions
Build a command line interface for adding/deleting/viewing a training program to a table in a SQLite db

You will need to construct a single table with these columns:

+ no_of_seats
+ instructor_name
+ start_date
+ end_date
+ course_category

The app should enable the following actions:
GET all/one course
DELETE a course
POST a course

Use test-driven development to build that logic of your app ( Do not test the UI functionality). Remember to practice `red green refactor` and write no feature before the test for that feature, and only write enough code in each iteraction to make the test pass.

### The UI
Use the [prompt npm package](https://www.npmjs.com/package/prompt)
to facilitate interaction with the user on the command line. You can use the [colors](https://www.npmjs.com/package/colors) package to spice up the terminal view

Example prompt: ( You do not need to make all of these actions available. Start with add new and view all, then move outward from there as stretch goals. )

```bash
> Welcome the the Bangazon Continuing Ed Course Creator
  Please choose an action from the following:
    1 create a new course
    2 edit an existing course
    3 remove a course
    4 view an upcoming course
    5 view all upcoming courses
    6 view a past course
    7 view all past courses
```

The above would be a single prompt. When the user enters a number, use that value to choose what action to take. If view all, then call the appropriate method for fetching all the classes ( which, of course, you build using TDD). If they want to add a new course,`prompt.get` again for that data:

```bash
  > Enter the course name:
  > Enter the instructor name:
  > Enter the start date:
  > Enter the end date:
  > Enter the number of seats:
```

Then take that data and save it to the db with a TDD-created save method.
