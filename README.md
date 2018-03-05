# zappa-contentful

Example application showing how to build an entirely serverless site with Python, Flask and Contentful. Then seamlessly deploy it to AWS Lambda/API Gateway using [Zappa](https://github.com/Miserlou/Zappa). A live version of this site lives at [henshin.ruparel.co](https://henshin.ruparel.co/).

## Contribution

This project is part of [contentful-userland](https://github.com/contentful-userland) which means that we’re always open to contributions and pull requests. You can learn more about how contentful userland is organized by visiting [our about repository](https://github.com/contentful-userland/about).

## Requirements

To use this project you have to have a Contentful and AWS account. If you don't have a Contentful account yet, you can register at [www.contentful.com/sign-up](https://www.contentful.com/sign-up/).

## Getting started

### Get the source code and install dependencies.

```
$ git clone git@github.com:Shy/zappa-contentful.git
$ cd zappa-contentful
$ virtualenv env
$ source env/bin/activate
$ pip install -r requirements.txt
```
You can use `python app.py` to run the application locally. 

#### Set up the content model and update the API Keys.

This project comes pre-connected to a live Contentful space. For you to be able to modify and evolve the project, you'll need to create your own Contentful space.

From the Contentful website click on the name of the space in the top left corner of the interface and select 'Add new Space'. Select the blank space option. Name your space, select its default locale (language) and the organization it should belong to. Then hit 'Create Space'.

To import the content model into your new space you'll need to install the [Contentful import tool](https://github.com/contentful/contentful-import).

```
npm install -g contentful-import
```

Once that's installed you'll be able to import the content model into your new space using the following command:

```
contentful-import \
  --space-id spaceID \
  --management-token managementToken \
  --content-file import_export/export.json
  ```

Make sure to update the command with your spaceID and mangementToken. You're able to find both of those keys via app.contentful.com -> Space Settings -> API keys.

Once that's taken care of update your [app.py](https://github.com/Shy/zappa-contentful/blob/master/app.py#L5-L6) file with your new SPACE_ID and DELIVERY_API_KEY.

#### Running Locally and deploying to AWS Lambda

To run the project locally, you can use `python app.py`.

[Zappa](https://github.com/Miserlou/Zappa) handles most of the legwork required to deploy on Lambda. Make sure that you've already [installed](https://docs.aws.amazon.com/cli/latest/userguide/installing.html) and [configured the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html#cli-quick-configuration).

To deploy to the cloud you can either use the existing Zappa configuration file or let Zappa automatically configure your deployment settings with `zappa init`.

If you use the configuration file in this repo you can deploy both a dev and production environment. For your dev environment use `zappa deploy dev` and for production `zappa deploy production`. The zappa deploy command will return a URL where you can access your website. 

Once you've deployed your dev and production environment if you make a code change you can use `zappa update dev` or `zappa update production` to push your code change to lambda without resulting in a chance to the URL that your function is deployed on. 

If you head over to API Gateway, you'll see a new API containing your function.

From this point, it's also possible to set up a [custom domain and SSL certificate](https://docs.aws.amazon.com/apigateway/latest/developerguide/how-to-custom-domains.html).
