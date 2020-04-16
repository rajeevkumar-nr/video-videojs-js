# New Relic VideoJS Tracker

The New Relic VideoJS tracker instruments the VideoJS player and Brightcove player. It also instruments Ads for FreeWheel and IMA. Makes use of [newrelic-video-core](https://github.com/newrelic/video-core-js), extending the `VideoTracker` class.

## Usage
Add **scripts** inside `dist` folder to your page.

> If `dist` folder is not included, run `npm i && npm run build` to build it.

### Standard way
```javascript
// var player = videojs('my-player')
nrvideo.Core.addTracker(new nrvideo.VideojsTracker(player))
```

### VideoJS Plugin Ecosystem
You can use built-in Videojs plugin system.

```javascript
// var player = videojs('my-player')
player.newrelic()
```

### Examples

Check out the `samples` folder for complete usage examples.

# Open source license

This project is distributed under the [Apache 2 license](LICENSE).

# Support

New Relic has open-sourced this project. This project is provided AS-IS WITHOUT WARRANTY OR DEDICATED SUPPORT. Issues and contributions should be reported to the project here on GitHub.

We encourage you to bring your experiences and questions to the [Explorers Hub](https://discuss.newrelic.com) where our community members collaborate on solutions and new ideas.

## Community

> TODO: Work with the Explorer's Hub team to create a tag for your app, then update the link below.

New Relic hosts and moderates an online forum where customers can interact with New Relic employees as well as other customers to get help and share best practices. Like all official New Relic open source projects, there's a related Community topic in the New Relic Explorers Hub. You can find this project's topic/threads here:

https://discuss.newrelic.com/t/{{ APP_NAME }}
*(Note: This URL is subject to change before GA)*

## Issues / enhancement requests

> TODO: Update path

Issues and enhancement requests can be submitted in the [Issues tab of this repository](../../issues). Please search for and review the existing open issues before submitting a new issue.

# Contributing

> TODO: Work with the Open Source Office to update the email alias below.

Contributions are encouraged! If you submit an enhancement request, we'll invite you to contribute the change yourself. Please review our [Contributors Guide](CONTRIBUTING.md).

Keep in mind that when you submit your pull request, you'll need to sign the CLA via the click-through using CLA-Assistant. If you'd like to execute our corporate CLA, or if you have any questions, please drop us an email at opensource+{{ APP_NAME }}@newrelic.com.
