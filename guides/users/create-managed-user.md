---
rank: 1
related_endpoints:
  - post_users
related_guides:
  - users/create-app-user
  - users/deprovision/user
  - users/deprovision/transfer-folders
related_pages: []
required_guides: []
related_resources: []
alias_paths: []
category_id: users
subcategory_id: null
id: users/create-managed-user
type: guide
is_index: false
total_steps: 3
sibling_id: users
parent_id: users
next_page_id: users/create-app-user
previous_page_id: ''
---

# Create Managed User

To generate a new managed user, the minimal information that will be required
will be a name and an email address for the managed user.

<Message type='notice'>

The email address supplied when creating a managed user must be unique. It
cannot already be associated with an existing Box user.

</Message>

<Samples id='post_users' >

</Samples>

To see all available optional parameters that may be set when creating an app
user, see the [create user endpoint](endpoint://post-users).

A user object will be returned from the create user request. Within the user
object is an ID for the managed user, which may be used to make API requests to
modify the user in the future.

Once a new managed user is created the email address used will receive an email
from Box asking them to create a password for the account. The account will be
in a `pending` state until that action has taken place.

<Message type='notice'>

For security reasons passwords cannot be supplied when creating a new managed
user

</Message>