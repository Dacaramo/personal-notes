# Deploy Web App to GitHub Pages

Go to the GitHub Actions marketplace and search the [Deploy to GitHub Pages Action](https://github.com/marketplace/actions/deploy-to-github-pages) and copy the code sample of the Getting Started section.

At the root of your project create the folder `/.github`
- Inside of it create the folder `/workflows`
	- Inside of it create a file named like `deployToGitHubPages.yml`

Paste the copied code sample inside of the YAML file and edit it so it looks like as follows:

```YAML
name: Build and Deploy
on: [push]
permissions:
  contents: write
jobs:
  build-and-deploy:
    concurrency: ci-${{ github.ref }} # Recommended if you intend to make multiple deployments in quick succession.
    runs-on: ubuntu-latest
    steps:
      - name: Checkout 🛎️
        uses: actions/checkout@v3

      - name: Install and Build 🔧 # This example project is built using npm and outputs the result to the 'build' folder. Replace with the commands required to build your project, or remove this step entirely if your site is pre-built.
        run: |
          npm ci
          npm run build

      - name: Deploy 🚀
        uses: JamesIves/github-pages-deploy-action@v4
        with:
          folder: build # The folder the action should deploy.
```