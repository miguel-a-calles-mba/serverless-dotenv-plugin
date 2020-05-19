# serverless-dotenv-plugin [![Financial Contributors on Open Collective](https://opencollective.com/serverless-dotenv-plugin/all/badge.svg?label=financial+contributors)](https://opencollective.com/serverless-dotenv-plugin) [![npm version](https://img.shields.io/npm/v/serverless-dotenv-plugin.svg?style=flat)](https://www.npmjs.com/package/serverless-dotenv-plugin)

Preload environment variables into serverless. Use this plugin if you have variables stored in a `.env` file that you want loaded into your serverless yaml config. This will allow you to reference them as `${env:VAR_NAME}` inside your config _and_ it will load them into your lambdas.

### Install and Setup

First, install the plugin:

```bash
> npm i -D serverless-dotenv-plugin
```

Next, add the plugin to your serverless config file:

```yaml
service: myService
plugins:
  - serverless-dotenv-plugin
...
```

Now, just like you would using [dotenv](https://www.npmjs.com/package/dotenv) in any other JS application, create your `.env` file in the root of your app:

```bash
DYANMODB_TABLE=myTable
AWS_REGION=us-west-1
AUTH0_CLIENT_ID=abc12345
AUTH0_CLIENT_SECRET=12345xyz
```

#### Automatic Env file name resolution

By default, the plugin looks for the file: `.env`. In most use cases this is all that is needed. However, there are times where you want different env files based on environment. For instance:

```bash
.env.development
.env.production
```

When you deploy with `NODE_ENV` set: `NODE_ENV=production sls deploy` the plugin will look for a file named `.env.production`. If it doesn't exist it will default to `.env`. If for some reason you can't set NODE_ENV, you could always just pass it in as an option: `sls deploy --env production` or `sls deploy --stage production`. If `NODE_ENV`, `--env` or `--stage` is not set, it will default to `development`.

The precedence between the options is the following:
`NODE_ENV` **>** `--env` **>** `--stage`

| Valid .env file names | Description                                                                                                    |
| --------------------- | -------------------------------------------------------------------------------------------------------------- |
| .env                  | Default file name when no other files are specified or found.                                                  |
| .env.development      | If NODE_ENV or --env or --stage **is not set**, will try to load `.env.development`. If not found, load `.env` |
| .env.{ENV}            | If NODE_ENV or --env or --stage **is set**, will try to load `.env.{env}`. If not found, load `.env`           |

### Plugin options

> path: path/to/my/.env

The plugin will look for your .env file in the same folder where you run the command using the file resolution rules as described above, but these rules can be overridden by setting the `path` option.

> basePath: path/to/my/

The problem with setting the `path` option is that you lose environment resolution on the file names. If you don't need environment resolution, the path option is just fine. If you do, then use the `basePath` option.

> include: ...

All env vars found in your file will be injected into your lambda functions. If you do not want all of them to be injected into your lambda functions, you can whitelist them with the `include` option.

> exclude: ...

(Added Feb 2, 2020 by @smcelhinney)

If you do not want all of them to be injected into your lambda functions, you can blacklist the unnecessary ones with the `exclude` option. Note, this is only available if the `include` option has not been set.

Example:

```yaml
custom:
  dotenv:
    exclude:
      - NODE_ENV # E.g for Google Cloud Functions, you cannot pass this env variable.
```

> logging: true|false (default true)

(Added Feb 2, 2020 by @kristopherchun)

By default, there's quite a bit that this plugin logs to the console. You can now quiet this with the new `logging` option. (This defaults to `true` since this was the original behavior)

Complete example:

```yaml
custom:
  dotenv:
    path: path/to/my/.env (default ./.env)
    basePath: path/to/ (default ./)
    logging: false
    include:
      - AUTH0_CLIENT_ID
      - AUTH0_CLIENT_SECRET
```

### Usage

Once loaded, you can now access the vars using the standard method for accessing ENV vars in serverless:

```yaml
...
provider:
  name: aws
  runtime: nodejs6.10
  stage: ${env:STAGE}
  region: ${env:AWS_REGION}
...
```

### Lambda Environment Variables

Again, remember that when you deploy your service, the plugin will inject these environment vars into any lambda functions you have and will therefore allow you to reference them as `process.env.AUTH0_CLIENT_ID` (Nodejs example).

You can avoid automatically injecting the variables by using the `processEnvOnly` option.

```yaml
custom:
  dotenv:
    processEnvOnly: true
```

### Examples

You can find example usage in the `examples` folder.

### Contributing

Because of the highly dependent nature of this plugin (i.e. thousands of developers depend on this to deploy their apps to production) I cannot introduce changes that are backwards incompatible. Any feature requests must first consider this as a blocker. If submitting a PR ensure that the change is developer opt-in only meaning it must guarantee that it will not affect existing workflows, it's only available with an opt-in setting. I appreciate your patience on this. Thanks.

### Roadmap

See https://colyn.dev/upcoming-changes-to-serverless-dotenv-plugin for upcoming changes.

## Contributors

### Code Contributors

This project exists thanks to all the people who contribute.
<a href="https://github.com/colynb/serverless-dotenv-plugin/graphs/contributors"><img src="https://opencollective.com/serverless-dotenv-plugin/contributors.svg?width=890&button=false" /></a>

### Financial Contributors

Become a financial contributor and help us sustain our community. [[Contribute](https://opencollective.com/serverless-dotenv-plugin/contribute)]

#### Individuals

<a href="https://opencollective.com/serverless-dotenv-plugin"><img src="https://opencollective.com/serverless-dotenv-plugin/individuals.svg?width=890"></a>

#### Organizations

Support this project with your organization. Your logo will show up here with a link to your website. [[Contribute](https://opencollective.com/serverless-dotenv-plugin/contribute)]

<a href="https://opencollective.com/serverless-dotenv-plugin/organization/0/website"><img src="https://opencollective.com/serverless-dotenv-plugin/organization/0/avatar.svg"></a>
<a href="https://opencollective.com/serverless-dotenv-plugin/organization/1/website"><img src="https://opencollective.com/serverless-dotenv-plugin/organization/1/avatar.svg"></a>
<a href="https://opencollective.com/serverless-dotenv-plugin/organization/2/website"><img src="https://opencollective.com/serverless-dotenv-plugin/organization/2/avatar.svg"></a>
<a href="https://opencollective.com/serverless-dotenv-plugin/organization/3/website"><img src="https://opencollective.com/serverless-dotenv-plugin/organization/3/avatar.svg"></a>
<a href="https://opencollective.com/serverless-dotenv-plugin/organization/4/website"><img src="https://opencollective.com/serverless-dotenv-plugin/organization/4/avatar.svg"></a>
<a href="https://opencollective.com/serverless-dotenv-plugin/organization/5/website"><img src="https://opencollective.com/serverless-dotenv-plugin/organization/5/avatar.svg"></a>
<a href="https://opencollective.com/serverless-dotenv-plugin/organization/6/website"><img src="https://opencollective.com/serverless-dotenv-plugin/organization/6/avatar.svg"></a>
<a href="https://opencollective.com/serverless-dotenv-plugin/organization/7/website"><img src="https://opencollective.com/serverless-dotenv-plugin/organization/7/avatar.svg"></a>
<a href="https://opencollective.com/serverless-dotenv-plugin/organization/8/website"><img src="https://opencollective.com/serverless-dotenv-plugin/organization/8/avatar.svg"></a>
<a href="https://opencollective.com/serverless-dotenv-plugin/organization/9/website"><img src="https://opencollective.com/serverless-dotenv-plugin/organization/9/avatar.svg"></a>
