# Flex Service Workspaces Example

This project uses [Create Kinvey Flex Service](https://github.com/dustintownsend/create-kinvey-flex-service) & [Yarn Workspaces](https://yarnpkg.com/lang/en/docs/workspaces/).

Checkout [Kinvey Flex Services Docs](https://devcenter.kinvey.com/nodejs/guides/flex-services) for more details on Flex services.

**You’ll need to have Node 8.10.0 or later on your local development machine** (but it’s not required on the server). You can use [nvm](https://github.com/creationix/nvm#installation) (macOS/Linux) or [nvm-windows](https://github.com/coreybutler/nvm-windows#node-version-manager-nvm-for-windows) to easily switch Node versions between different projects.

**You'll need to have YarnPkg on your local development machine**. [yarn](https://yarnpkg.com/en/docs/install).

## Development Machine Set-up

1. Install NVM [macOS/Linux](https://github.com/creationix/nvm#installation) or [Windows](https://github.com/coreybutler/nvm-windows#node-version-manager-nvm-for-windows) to easily switch Node versions between different projects. Have Node 8.10.0 or later installed.
2. Install [yarn](https://yarnpkg.com/en/docs/install).
3. Install [Kinvey CLI](https://github.com/Kinvey/kinvey-cli) - **See Kinvey-CLI Setup below**.
4. Create `.env` file. Use `.env.example` as a starting point. See **Environment Variables** below.
5. Run `yarn workspaces run` to install the dependencies and set-up the yarn workspaces.

## Kinvey-CLI Setup

- To install globally run: `npm install -g kinvey-cli`
- Run `kinvey init`
  - Authenticate with Kinvey credentials (username and password).
  - Set Instance ID if needed.
  - Set the profile name to `exampleprofile`.
- Run `kinvey profile use exampleprofile` to set exampleprofile as the active profile.
- When the auth token expires, run `kinvey profile login` to re-authenticate the profile.

## Environment Variables

Environment variables are used for debugging / testing Kinvey Flex services locally.
The environment variables are injected into the development server to allow a live connection
with the Kinvey Environment. This is useful for debugging and testing before deploying.

These variables are used when running the `yarn start` command.

| Variable name          | Required? | Notes     |
| :--------------------- | :-------- | :---------|
| `KINVEY_BAAS_URL`      | YES       | Required. |
| `KINVEY_APP_ID`        | YES       | Required. |
| `KINVEY_APP_SECRET`    | *YES*     | Required unless using `KINVEY_AUTH_HEADER` |
| `KINVEY_MASTER_SECRET` | *YES*     | Required unless using `KINVEY_AUTH_HEADER` |
| `KINVEY_AUTH_HEADER`   | *NO*      | Required if not using `KINVEY_APP_SECRET` and `KINVEY_MASTER_SECRET` |
| `KINVEY_USER_NAME`     | NO        | Required if code needs to execute in User Context. |
| `KINVEY_USER_ID`       | NO        | Required if code needs to execute in User Context. |
| `FLEX_SHARED_SECRET`   | NO        | Required if shared secret is set in the service options. |
| `KINVEY_API_VERSION`   | NO        | Defaults to 3. Only required if need to test against a different version of Kinvey API. |

## Commands

### `yarn workspaces run`

This command will run yarn in all the workspaces. This command should be run after pulling new changes from git.
You can add additional commands at the end if needed. This will run the command in all workspaces.
For example, running `yarn workspaces run deploy` will run the `yarn deploy` command in all workspaces (aka services).

### `yarn run start`

This command will start the development server. If environment variables are detected they are injected to make debugging / testing locally easier.
While this command is running, changes to source files will trigger a live reload. So, no need to restart this command after each change.

### `yarn run build`

This command will build a production ready bundled javascript file and package.json for deployment. These will be created in the build folder.

### `yarn run deploy`

This command will deploy the files in the build folder to Kinvey Flex. The version number is automatically bumped for you based on the current deployment version.

Some arugments are available, but optional.

- `--version / -v` will take a manual version input.
- `--major` will do a major version bump (instead of the default behavior of a patch)
- `--minor` will do a minor version bump (instead of the default behavior of a patch)
- `--skip-version-checks` will skip the version checks that happen before deploying. The deployment may fail when running this and should only be used if absolutly needed.

## Local Testing

See **Environment Variables** above and create a .env file for each service. Use .env.example for a starting point.
Replace port `10001` with port `9999` when calling the endpoints from postman.
Example: Instead of <http://localhost:10001/_flexFunctions/firstFunction> using <http://localhost:9999/_flexFunctions/firstFunction>

## Useful Kinvey-CLI Commands

### `kinvey flex init`

This will create a .kinvey file with details about the Flex Service. It will be used by other Flex related commands.
Each services directory should have a .kinvey file.

### `kinvey flex status`

Check the status of a Flex service. Provides information like deployment status and version.
Useful for checking the deployment status after running the deploy command.

## Create a new Service

1. Go to the /services directory from the terminal/command line. *Tip: inside [VSCode](https://code.visualstudio.com/) right click on the services folder in the File Explorer and click open in terminal*
2. Type `yarn create kinvey-flex-service [service-name]` replace [service-name] with the name of the new service. It should be lower case and have no spaces.
3. Type `cd [service-name]` replace [service-name] to match what was used in the previous command. This will also be shown in the instructions on the terminal.
4. Type `yarn start` to start the development server and make sure everything was set-up correctly. Press `control/cmd + c` to exit the command

If the service is not created inside Kinvey, you can either go to the Kinvey Console website and create it there or use the Kinvey CLI.
To use the Kinvey CLI:

1. Type `kinvey flex create [service-name] --app [app-name]`. The name doesn't have to match the name used above, but it doesn't hurt.
2. A service Id and secret should be returned if successful.

Finally after a service has been created you need to run the flex init command.

1. Type `kinvey flex init`
2. Choose the Kinvey App.
3. Select the service from the list. If it doesn't appear here then it may not have been created.
4. This will create a .kinvey file in the directory.

That's it. You now have a new Flex service ready for development and deployment.
