# semantic-release-slack-bot

[**semantic-release**](https://github.com/semantic-release/semantic-release) plugin to get release notifications on [slack](https://slack.com/) from a slack bot

[![npm](https://img.shields.io/npm/v/semantic-release-slack-bot.svg?style=flat-square)](https://www.npmjs.com/package/semantic-release-slack-bot)
[![npm](https://img.shields.io/npm/dm/semantic-release-slack-bot.svg?style=flat-square)](https://www.npmjs.com/package/semantic-release-slack-bot)
[![CircleCI branch](https://img.shields.io/circleci/project/github/juliuscc/semantic-release-slack-bot/master.svg?style=flat-square)](https://circleci.com/gh/juliuscc/semantic-release-slack-bot)
[![license](https://img.shields.io/github/license/juliuscc/semantic-release-slack-bot.svg?style=flat-square)](https://github.com/juliuscc/semantic-release-slack-bot/blob/master/LICENSE)
[![add to slack](https://img.shields.io/badge/Add%20to%20Slack-Semantic%20Release-%234A154B.svg?style=flat-square&logo=slack)](https://slack.com/oauth/authorize?client_id=605439709265.611687593109&scope=incoming-webhook)

| Step      | Description                                         |
| --------- | --------------------------------------------------- |
| `success` | Send a slack message to notify of a new release.    |
| `fail`    | Send a slack message to notify of a failed release. |

## Install

Add the plugin to your npm-project:

```bash
$ npm install semantic-release-slack-bot -D
```

The corresponding slack app has be installed in you slack workspace as well. Follow the instructions under [configuration](#configuration) for more information.

## Usage

The plugin can be configured in the [**semantic-release** configuration file](https://github.com/semantic-release/semantic-release/blob/master/docs/usage/configuration.md#configuration):

```json
{
	"plugins": [
		"@semantic-release/commit-analyzer",
		"@semantic-release/release-notes-generator",
		[
			"semantic-release-slack-bot",
			{
				"notifyOnSuccess": false,
				"notifyOnFail": true
			}
		]
	]
}
```

With this example:

-   Slack notifications are skipped on a succesfull release
-   Slack notifications are sent on a failed release

## Screenshots

![Screenshot of success](images/screenshot-success.png)
![Screenshot of fail](images/screenshot-fail.png)

## Configuration

### Slack app installation

The plugin uses a slack webhook which you get by adding the slack app to your slack workspace. Register the app using the button below or [this link](https://slack.com/oauth/authorize?client_id=605439709265.611687593109&scope=incoming-webhook).

<a href="https://slack.com/oauth/authorize?client_id=605439709265.611687593109&scope=incoming-webhook"><img alt="Add to Slack" height="40" width="139" src="https://platform.slack-edge.com/img/add_to_slack.png" srcset="https://platform.slack-edge.com/img/add_to_slack.png 1x, https://platform.slack-edge.com/img/add_to_slack@2x.png 2x"></a>

### Slack app authentication

Installing the app will yield you with a webhook that the app uses to publish updates to your selected chanel. The Slack webhook authentication link is **required and needs to be kept a secret**. It should be defined in the [environment variables](#environment-variables).

### Environment variables

The `SLACK_WEBHOOK` variable has to be defined in the environment where you will be running semantic release. This can be done by exporting it in bash or in the user interface of you CI provider. Obtain this token by installing the slack app according to [slack app installation](#slack-app-installation).

| Variable        | Description                                         |
| --------------- | --------------------------------------------------- |
| `SLACK_WEBHOOK` | Slack webhook created when adding app to workspace. |

### Options

| Option              | Description                                                                                                                   | Default   |
| ------------------- | ----------------------------------------------------------------------------------------------------------------------------- | --------- |
| `notifyOnSuccess`   | Determines if a succesfull release should trigger a slack message to be sent. If `false` this plugin does nothing on success. | false     |
| `notifyOnFail`      | Determines if a failed release should trigger a slack message to be sent. If `false` this plugin does nothing on fail.        | false     |
| `onSuccessTemplate` | Provides a template for the slack message object on success when `notifyOnSuccess` is `true`. See [templating](#templating).  | undefined |
| `onFailTemplate`    | Provides a template for the slack message object on fail when `notifyOnFail` is `true`. See [templating](#templating).        | undefined |

### Templating

If a template is provided via either the `onSuccessTemplate` or `onFailTemplate` options, it will be used for the respective slack message. The template should be an object that follows the [Slack API message structure](https://api.slack.com/docs/message-formatting). Strings within the template will have keywords replaced:

| Keyword                | Description                 | Example                                           | Template          |
| ---------------------- | --------------------------- | ------------------------------------------------- | ----------------- |
| `$npm_package_name`    | The name of the package.    | semantic-release-test                             | Both              |
| `$npm_package_version` | The version of the release. | 1.0.93                                            | onSuccessTemplate |
| `$repo_path`           | The repository path.        | juliuscc/semantic-release-test                    | Both              |
| `$repo_url`            | The repository URL.         | https://github.com/juliuscc/semantic-release-test | Both              |
| `$release_notes`       | The notes of the release.   |                                                   | onSuccessTemplate |
