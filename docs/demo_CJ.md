# Create a HackMD GitHub App

## 1. Register a new App
- For personal App, go to `https://github.com/settings/apps`

    ![](https://i.imgur.com/SlJmoFj.png)

- For Organizational App, go to `https://github.com/organizations/<org_name>/settings/apps`
    - (Replace <org_name> with the name of your GitHub Org.)

    ![](https://i.imgur.com/6ah6aC0.png)

- Hit the button: "New GitHub App"


## 2. Setup the App in the form:

![](https://i.imgur.com/6ggGO3R.png)

Follow below to fill in the form:

- GitHub App name: *Fill-in anything*
- GitHub App description: *Fill-in anything*
- Homepage URL: *Fill-in anything*
- User authorization callback URL: 
==`https://<instance url>/api/github/sync/callback`==
    Where`<instance url>` is your HackMD instance
- Request user authorization (OAuth) during installation: ==Check== :white_check_mark: 
- Setup URL: ==Leave blank==
- Redirect on update: ==Check== :white_check_mark: 
- Webhook URL:
==`https://<instance url>/api/github/sync/webhook`==
    Where`<instance url>` is your HackMD instance
- Webhook secret: ==Any string. We will need this later.==
- Repository permissions: ==Give access to below.== :white_check_mark:
    - Checks: Read & Write
    - Contents: Read & Write
    - Metadata: Read-only
    - Pull requests: Read & Write
    - Webhooks: Read & Write
    - Commit statuses: Read & Write
- Organization permissions: *Leave it*
- User permissions: *Leave it*
- Subscribe to events: ==Give access to below.==  :white_check_mark: 
    - Check run
    - Check suite
    - Create
    - Public
    - Pull request
    - Push
    - Repository
    - Repository dispatch

## 3. Generate a private key:

- After setting up the app, go to the App page and scroll to the bottom. 

![](https://i.imgur.com/WxRnMFQ.png)

- Generate a Private Key and download the .pem file.

![](https://i.imgur.com/zE8jc6y.png)

## 4. Share the below with the HackMD Team:

On the GitHub App page
- App ID: on the GitHub App page
- Client ID: on the GitHub App page
- Client secret: on the GitHub App page
- GitHub App name: find on GitHub App page URL last part (as below)
    - ![](https://hackmd.io/_uploads/H1z-Ae_6t.png)
- Webhook secret: the string generated in step 2.
- GitHub App key: the .pem file generated in step 3.