# GOV.UK Synthetic test app canary

This is a simple synthetic test app used to test that the GOV.UK CI/CD pipeline is working.

## Run the app locally

The app is intended to run on the GOV.UK Kubernetes clusters, but it is also possible to run it locally.

```sh
docker build -t govuk-synthetic-test-app .
```

```sh
docker run --rm -p3000:3000 govuk-synthetic-test-app
```

You should then be able to browse the web app on http://localhost:3000/.

## Usage in synthetic tests

The repo is used as a sort of fixture, an example app to test deployment against. The actual synthetic tests live in [govuk-synthetic-test-app](https://github.com/alphagov/govuk-synthetic-test-app) and use ginkgo to push a commit to the canary repo, which then triggers the deployment pipeline starting with the [Github Action Release](./.github/workflows/release.yml), followed by [Deploy](./.github/workflows/deploy.yml). The ginkgo tests will then trace the deployment and relevant tags and shas through the argo and k8s environments. The canary app itself is very simple and is only responsible for serving the value found in `.version` at the `/` endpoint, the gingko tests will lastly assert that the correct value (.version matches the released github tag) is being served.
