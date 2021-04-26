---
layout: datacollection
title: Overview
redirect_from: /datacollection/1.html
section: datacollection
---

# {{ page.title }}

There are several ways in which experiments can be deployed.

## Deploying an experiment online

### Frontend configuration

By default, _magpie relies on [GitHub Pages](https://pages.github.com/) to make experiments accessible online. However, you may choose to use any static website host, such as [Netlify](https://www.netlify.com/) or [GitLab Pages](https://about.gitlab.com/features/pages/) or even your own server, since in essence the experiments are composed of plain HTML/JS/CSS files.

To deploy an experiment via GitHub Pages:
1. Make sure that the experiment repo is already on GitHub, e.g. https://github.com/magpie-ea/MinimalTemplate
2. Go to the "Settings" tab of the repo, e.g. https://github.com/magpie-ea/MinimalTemplate/settings
3. Scroll down to find the section titled "GitHub Pages". Under "Source", choose "master branch" and click on save.
4. The experiment should now be available at your-account.github.io/ExperimentName, e.g. https://magpie-ea.github.io/MinimalTemplate/

<!--- Make sure that the entry point of the experiment is named `index.html`. Otherwise GitHub Pages will not be able to serve the experiment correctly. -->

#### Crowdsourcing (MTurk, Prolific) or direct link?
You might recruit participants via [Amazon Mechanical Turk](https://www.mturk.com/), [Prolific](https://www.prolific.ac/), or by directly sending the link to them. Please change the `config/config_deploy.js` in the experiment directory accordingly, and then push the changes via git:

```javascript
var config_deploy = {
	...
	"deployMethod" : 'directLink', //  use one of 'MTurk', 'Prolific', 'directLink'
	...
}
```

### Backend configuration
After an experiment is finished online, the results need to be stored and retrieved later. A server is needed for that purpose. The default server implementation for _magpie is available at https://github.com/magpie-ea/magpie-backend. You may refer to its documentation for details. It is based on [Phoenix Framework](http://phoenixframework.org/) and written in [Elixir](https://elixir-lang.org/). A demo app is deployed on Heroku at [https://magpie-demo.herokuapp.com/](https://magpie-demo.herokuapp.com/).

In the current version of the server app, each experiment is explicitly created and managed with a user interface. After creation, each experiment gets assigned a unique ID. Experiment results need to be submitted with [HTTP POST](https://en.wikipedia.org/wiki/HTTP_POST) in [JSON format](https://en.wikipedia.org/wiki/JSON) to the endpoint displayed in the UI:

![Submission UI](../images/submission_ui.png)

Don't forget to set the unique ID in `config/config_deploy.js` of your frontend configuration:

```javascript
var config_deploy = {
	...
  "experiment_id": 1,
	...
}
```

You are recommended to deploy your own server instance, either with Heroku or with other hosting services you see fit. The detailed deployment instructions for Heroku can be found [here](../serverapp/02serverinstall.html).

## Deploying an experiment locally
Sometimes you may want to let the participants complete an experiment directly on a local machine without requiring internet connection. This is particularly useful for doing fieldwork or working in labs.

### Frontend configuration
The only change needed should be in `config/config_deploy.js`.

```javascript
var config_deploy = {
	...
	"deployMethod" : 'localServer',
	...
}
```

You can then run the experiment by opening `index.html` in the browser in your local machine, and invite participants to finish the experiment.

(Of course, if the machine has internet connection, you can still specify `directLink` as the `deployMethod`, and the results will be submitted to the online server instead, even if the experiment is run locally. This way you won't need to additionally run a local server.)

### Backend configuration
This time, the server needs to be deployed on the local machine instead of online. To simplify the deployment, [Docker](https://www.docker.com/) is used. Please refer to [the documentation](../serverapp/03localinstall.html) for detailed deployment instructions.

Everything else, including the usage of the user interface, should remain the same just like in online deployment.

## Using the "debug" deployment
With "Debug" deployment, no backend server will be used and no experiment results will be actually recorded. Instead, the results will be displayed directly on the webpage when the experiment finishes. This helps you debug the experiment and ensure that the results are recorded as intended.

To set deployment mode to "Debug", simply set

```javascript
var config_deploy = {
	...
	"deployMethod" : 'debug',
	...
}
```

in the `config/config_deploy.js` file in your experiment directory.

You can then test the experiment by opening `index.html` in the browser in your local machine.
