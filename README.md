# Continuous deployment with Mantle and roblox-ts template

Welcome to a quick guide on how to set up your workflow for CD (continuous deployment) with [Mantle](https://mantledeploy.vercel.app) and [roblox-ts](https://roblox-ts.com).

Mantle is a roblox infrastructure-as-code deployment tool (words of a friend, very wise). Letting you deploy a ROBLOX experience (game) from a CLI in no time by just writing a few lines. Letting you focus on writing and not caring about deployment, as once your code is bug-free, it will be merged to a deployable branch and Mantle can take care of everything else.

Imagine having this type of workflow:

-   Feature being worked on `dev/<feature>` branch.
-   Branch passes initial checks, reviewed and approved.
-   Merged into `main` (or `master`).

And now you need to `git checkout main`, pull changes, `rojo serve`, sync the latest code and publish those changes to Roblox.

This isn't the best way to go, and by far it would be with already existing technology that can automatize this for you.

## Getting the template

First of all, everything you need to do is run this command in your terminal:

```
npx degit siriuslatte/mantle-roblox-ts-template
```

## AWS set up + credentials for authentication

This kind of setup uses AWS (Amazon Web Services) S3 (Simple Storage Service) buckets to read/write the mantle-state files automatically generated when deploying the place. Using a remote state enables the CD workflow we want to achieve, and so on, you need to do some setup on this for it to work correctly as it needs some kind of authentication.

The first step, go to their [page](https://aws.amazon.com/es/) and log in to the Console. To not overcomplicate things (but this is unsafe according to AWS itself) we will use the root user to create the credentials.

Once logged in, go to the search bar and type "_S3_", and click on the first one that appears in the _Services_ category. It will redirect you to their S3 management console. On there, click on "Create Bucket", give it a name, and a region (near your living), and don't worry about anything else. You have just created your amazing bucket!

Now you may go and create an API access key for programmatic calls to AWS as if it was you, be careful, anyone owning these credentials can take full control over your account.

> See [more information](https://docs.aws.amazon.com/accounts/latest/reference/credentials-access-keys-best-practices.html) about how to safely make programmatic calls to AWS without them being on the root user.

To create a new API access key, we will go ahead and click our username on the top right corner, then click **My Security Credentials**, and scroll down until you see the **Access keys** section. Click on **Create new access key** and follow the steps. In the end, you should end up with a `.csv` file, this one is VERY important for you to keep as otherwise, you won't be able to access your Secret Key once again.

We just got what we need from AWS. Though we aren't finished _just yet_. For this last step, we will be modifying the `mantle.yml` file on our root folder (the project folder). Open the file, and scroll down till **state** configuration.

## Setting up the GitHub Actions Secrets

If you have already made your remote repository at [GitHub](https://github.com), then the next step is to go to the configuration of that repository, scroll down to **Secrets**, expand, and then select **Actions**. You'll need to add a couple of secrets so no one else can see your credentials and use them maliciously.

### ROBLOX Cookie

The first one will be your ROBLOX `.ROBLOSECURITY` cookie. This is obtained from the website, by opening the dev tools, navigating to the **Application** panel > **Cookies** and searching for the `.ROBLOSECURITY` one.

The name for the secret will be **ROBLOSECURITY** and the value would be what you just copied from the previous steps.

### AWS Credentials

Now time for the AWS credentials. The name for the AWS Key will be **AWS_ACCESS_KEY_ID** and the value will be the one on the `.csv` file.

And the name for the Secret Key will be **AWS_SECRET_ACCESS_KEY** and the value will be the one on the `.csv` file as well.

## Done!

You made an excellent job, now you can push a change (to the main branch), see that it compiles correctly (if you didn't shoot yourself in the foot), build the place, and deploy it to ROBLOX, all in one place!

Feel free to check out the dev workflow (.github > workflows) so you can understand the type of workflow the development environment has.

Some extra features included:

-   It uses remodel, so you can script some of your logic for a fully Rojo-managed place, under the `scripts` folder, including special behavior for the development environment, as you may want to add tests.
