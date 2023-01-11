************Step 1************

Enter email of test account B for password reset.

************Step 2************
Intercept the POST request

************Step 3************
Change the Referer header to interactsh-client 

************Step 4************

From test account B’s email click on the poisoned link

************Step 5************

Check the Interactsh-client logs and it will have GET request containing the account B’s token required to change the account B’s password.
