## Security [Optional]

### Prerequisites:

* SSO Tile installed
* SSO plan called sso-dev is create and has IDP called "gcp" configured against google. See https://docs.pivotal.io/p-identity/1-5/#gcp
* You can access SSO admin portal to edit user's permissions

### Steps

1. Create an instance of SSO from marketplace (call service `sso`)
1. Bind it to FunnyQuotesUICore & FunnyQuotesServicesOwin
1. Edit application.yml in config repo and enable security. Commit + push
1. Restart both apps
1. Check manifest.yml for SSO settings to use for FunnyQuotesUICore.
	1. Adjust SSO_REDIRECT_URIS to be correct based on environment
	1. Push the app using the manifest (delete other apps from it)
1. Access the FunnyQuotesUICore and get a cookie. You should be redirected to authenticate with google
1. Access the sso plan from apps manager. Explain:
	1. How adding apps works, their types and how they map on to OAuth2 flows
	1. How managing resources declared by app
1. Click Kill button and highlight that different set of permissions is required
1. Go to SSO admin portal. Edit plan IDP, manage users. Add resource `funnyquote.kill` to the previously logged in user.
1. Get a new token by logging out and relogging. 
1. Kill command should now work
