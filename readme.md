## Capstone 1 - [Recipe eBox](https://jimg-recipe-ebox.herokuapp.com)

Recipes may exist in multiple places – printed form, unshared browser bookmarks on desktop computers and mobile devices, and email. Was the recipe enjoyed? How long did it take to prepare? What ingredients do I need to purchase? 

With Recipe eBox, all of your recipes can exist in one place. A registered person can create, edit and delete their recipes.

Bored and desire something different? Search Edamam for different recipes!

This project was completed and submitted on May 16, 2021 in a private repository. This readme.me file was part of that original submission. I still iterate on this project as it is something I plan on using and enhancing. The ability to reset a password with a tokened link in an email was completed in late May 2021. I look forward to better integrating the Edamam api as well.

**NOTE**: code requires a `config.py` file with 
- `EDAMAM_API_KEY=your_api_key` in order to access Edamam functions.
- `MAIL_PASSWORD=mailpassword` in order to use emailing functions. Well, you can use them, they just will not work!


## Database
Recipe eBox's database currently consists of 6 tables:
`users` - table has the email, password, display name, city, and preferred unit of measure. When registering, a new customer provides a unique email address, password, and a display name. Customer can specify their preferred unit of measurement (imperial or metric). 

`recipes` - table contains the high-level details of the recipe -- the owning customer, recipe title, main protein, source url, customer comments about recipe, their rating (0 - 5, 5 is best), number of times prepared, date added, and date last prepared.

`recipe_ingredients` - table contains the ingredients and quantities used in the recipe. 

`recipe_directions` - table contains the steps required to prepare the recipe.

`ingredients` - table contains the ingredients for all recipes saved in Recipe eBox. When a customer saves their recipe, their named ingredients are checked against this table. Any ingredients on the customer's recipe that do not exist in the table are added. 

`codes` - table contains common codes for Recipe eBox. Currently, codes exist for metric, imperial measurement systems and 11 possible food categories. The design belief is to have all the codes in one table instead of having multiple tables for the code. 


## Routes
[`/`](https://jimg-recipe-ebox.herokuapp.com) - Recipe eBox root. Redirects to `/register`

[`/register`](https://jimg-recipe-ebox.herokuapp.com/register) - Recipe eBox registration page. Page includes a link to the login page for those customers who are already registered. Upon successful user creation, customer is redirected to their empty recipe listing page. 

[`/login`](https://jimg-recipe-ebox.herokuapp.com/login) - Recipe eBox login page for customer's login credentials. Page includes a link to the registration page for those visitors who are not registered. Upon successful login, customer is redirected to their recipe listing page.

[`/passwordreset`](https://jimg-recipe-ebox.herokuapp.com/passwordreset) - Recipe eBox starting point for password reset requests. The link to password reset page was added to the login page.

`/logout` - Recipe eBox logout. Logout is a navigation option that appears in a dropdown when the customer clicks on their name.

`/user` - a rerouting mechanism to catch when customer manipulates the url

`/recipes` - formerly `/user/<userid>/recipe`, the customer's home page that lists all of their recipes stored in Recipe eBox. Customer can view existing recipes or add a new recipe from this page.

`/recipes/add` - formerly `/user/<userid>/recipe/add`, the page for adding a new recipe. Customer minimally needs to provide a recipe title and the number of times prepared. Customer can enter any number of ingredients and steps required to create their masterpiece! An ingredient must have a quantity and a step with details must have a step number. Customer is redirected to their home page after a recipe was successfully added.

`/recipes/<recipeid>` - formerly `/user/<userid>/recipe/<recipeid>`, the page for viewing the details (overview, ingredients, and directions) for a recipe. From this page, the customer can launch the edit page. The recipe details page also includes a button to `! Delete Recipe !`

`/recipes/<recipeid>/edit` - formerly `/user/<userid>/recipe/<recipeid>/edit`, the page for editing an existing recipe. The customer is redirected to the recipe's viewing page upon successful validation of the customer's edits. 

`/recipes/<recipeid>/delete` - formerly `/user/<userid>/recipe/<recipeid>/delete`, the delete of a customer recipe. Deletion of a recipe is initiated from the recipe's detail page.

`search` - search Edamam for recipes that include the entered search text. Search is initiated from the navigation bar and is available to all site visitors.


## JavaScript
JavaScript was required to handled adding and deleting new ingredient and direction lines for a recipe. 
Clicking on `+` will insert a new ingredient / direction row below the row where the click occurred. 
Clicking on the trash can button deletes the row where the delete was clicked. Ingredient / direction rows are renumbered after the delete.


## Version 1.1.0
- BUG FIX: recipe ingredients and directions were not persisting when an error occurred while adding a new recipe.
- Password now have character criteria and code to check for it. Previous release only checked password length, not content.
- Field errors now appear by applicable input fields where possible.
- Confirm Password field added to registration form.
- `reset_token` field added to `User` model and added to `users` table via `ALTER TABLE users ADD COLUMN reset_token text;`.
- `the.recipe.ebox@gmail.com` email account created for application and for application-based emailing of password reset requests.
- Password resets are now possible. 
  1. The customer enters the email associated with the account.
  1. An email is sent with a reset token.
  1. User receives email and clicks on the link.
  1. User is presented with a page where they can enter a new password. 
  1. User is logged into Recipe eBox once password is successfully changed.
- Route improvements: `user` is no longer part of the recipe routes.
- Code module improvements. 
  - `module_recipe` added for all validations and edits of form data. Once all form information is validated, information is handed off to functions in `module_recipe_db` to carry out the necessary database changes.
  - `module_recipe_db` added for all of the logic for handling database changes for recipe, ingredients, and directions.


## Futures

- Better integration between Edamam search results and recipe storage.
- More button to load additional search results. Only the first 10 are currently displayed.
- Support for preferred unit of measure and conversions between measurement systems.
- User profile maintenance.
- Support for providing ingredient details for a new ingredient. Currently, ingredient category and is_liquid default to 'grocery' and 'false' respectively. 
  - ingredient category would group like ingredients together for a shopping list function. 
- Measurement table to hold imperial and metric measurements. The measurements displayed are determined by the customers selected measurement preference. 
- A custom 404 page.
- Proper sorting of steps before renumbering steps.
- User password reset capabilities were added in v1.1.0.
- Shopping list generation.
- 'I just made this' button to update last made and times prepared. 
- Page layout improvements, especially for small devices.

