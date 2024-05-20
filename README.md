# SubscribeToMailchimp
A lightweight module for the [ProcessWire CMS/CMF](https://processwire.com/) that lets you subscribe a user to a [Constant Contact](https://www.constantcontact.com/) list.

## The basic idea
```php
// Easily subscribe a user with SubscribeToConstantContact
$mc = $modules->get("SubscribeToConstantContact");
$mc->subscribe('email@example.com');
```

## How To Install
1. Download the [zip file](https://github.com/BrendonKoz/SubscribeToConstantContact/archive/master.zip) at Github or clone the repo into your `site/modules`
2. If you downloaded the `zip file`, extract it in your `sites/modules` directory.
3. You might have to change the folder's name to 'SubscribeToConstantContact'.
4. Go to the modules admin page, click on refresh and install it. This will also install the associated SubscribeToConstantContactKeepAlive module which is necessary for keeping the API active.

## Setup at Constant Contact
1. Log into your Constant Contact account and go to the [Developer Portal](https://developer.constantcontact.com/) > My Applications (you may have to click a "log in" button again) and edit an existing, or create a new application using Authorization Code Flow and Implicit Flow, and rotating refresh tokens. Retrieve the API Key and Client Secret.

_**NOTE:** The client secret may need to be recreated to retrieve it._

## Module Setup
1. Put the API Key and Client Secret into the module settings (`Processwire > Modules > Site > SubscribeToConstantContact`), and use the "Redirect URL" as printed on the module config screen for your Constant Contact API application's _Redirect URI_ field value. Submit the form to save the values from this step.
2. Click on the generated URL, "Authorize and connect this module to your Constant Contact Application," on the module configuration screen.
3. Values have been retrieved from the Constant Contact API. Click "Submit" to save them to the module configuration.
4. **OPTIONAL:** Choose a default contact list for the module to subscribe contacts to.

![module settings](https://i.imgur.com/Gps1dWi.png)

## Usage
```php
// load module into template
$mc = $modules->get("SubscribeToConstantContact");

 // subscribe / update a user in your default audience
$mc->subscribe('email@example.com');

// add additional fields to fill out user data
// subscribe($email, $list_id, $parameters)
// $list_id will default to the module's saved configuration value, if set
// NOTE: Parameter values are not validated by the module, see the documentation for further info
$mc->subscribe('email@example.com', null, ['first_name' => 'John', 'last_name' => 'Doe']);

// Subscribe a user to a specific list (other than default)
$mc->subscribe('email@example.com', 'adcdef12345', ['first_name' => 'John', 'last_name' => 'Doe']);
```

Additional methods
```php
// Unsubscribe a user
$mc->unsubscribe('email@example.com');

// Delete a user. Deleted users still exist in Constant Contact, but cannot be seen (in Constant Contact) or retrieved (via API)
$mc->delete('email@example.com');

// Unsubscribe a user from a contact list (or array of lists)
$mc->removeFromList('email@example.com');
$mc->removeFromList('email@example.com', 'abcdef1356');
$mc->removeFromList('email@example.com', ['abcdef1356']);

```

## Example
Example usage after a form is submitted on your page:
```php
// ... validation of form data

$mc = $modules->get("SubscribeToConstantContact");
$user_email = $sanitizer->email($input->post->email);
$mc->subscribe($user_email);

```

## Troubleshooting 
In case of trouble check your ProcessWire warning logs.

### FAQ
#### I can't see the subscriber in the contact list
If you have enabled double opt-in in your Constant Contact settings, you will not see the subscriber until the confirmation link in the email sent by Constant Contact has been used; the user may have also been deleted

#### I get an error in my ProcessWire warning logs
* Check if you have the right contact list ID and API Key.
* Check if you pass a valid email address.
* Make sure LazyCron is installed

Go to the [Constant Contact Developer Documentation](https://developer.constantcontact.com/api_guide/index.html) for more information.

\*I have only done minimal testing on this module, so use with caution

## Contribution
Pull requests welcome. (Especially for the awkward module setup/configuration flow.)