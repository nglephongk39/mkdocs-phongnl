# Deploying an MkDocs Site from a GitHub Repository to Vercel

This guide shows how to deploy an MkDocs project from GitHub to Vercel.

## 1. Prepare the repository

Make sure your project includes these files:

```text
mkdocs.yml
docs/
pyproject.toml
uv.lock
```

The `mkdocs.yml` file controls the site configuration, and the `docs/` folder contains the Markdown pages.

## 2. Add a Vercel configuration

Create a `vercel.json` file in the root of the repository:

```json
{
  "buildCommand": "uv run mkdocs build",
  "outputDirectory": "site",
  "installCommand": "uv sync --frozen"
}
```

## 3. Push the project to GitHub

Commit and push the documentation project:

```bash
git status
git add .
git commit -m "Add MkDocs Vercel deployment"
git push
```

## 4. Import the repository in Vercel

1. Open [Vercel](https://vercel.com).
2. Click **Add New** > **Project**.
3. Select the GitHub repository that contains your MkDocs site.
4. Keep the framework preset as **Other**.
5. Confirm these project settings:

```text
Build Command: uv run mkdocs build
Output Directory: site
Install Command: uv sync --frozen
```

## 5. Deploy

Click **Deploy**. Vercel will install dependencies, build the MkDocs site, and publish the generated `site/` directory.

## 6. Update the site

After the first deployment, every push to the connected branch will trigger a new Vercel deployment.

```bash
git add .
git commit -m "Update docs"
git push
```

## Troubleshooting

### Build command fails

Run the same command locally:

```bash
uv run mkdocs build
```

If it fails locally, fix the MkDocs error before redeploying.

### Vercel cannot find `uv`

If Vercel does not provide `uv` in the build environment, replace the Vercel commands with pip-based commands:

```json
{
  "buildCommand": "mkdocs build",
  "outputDirectory": "site",
  "installCommand": "pip install -r requirements.txt"
}
```

Then add a `requirements.txt` file:

```text
mkdocs>=1.6.1
mkdocs-material>=9.7.6
```

### The deployed site is blank or missing styles

Check that `outputDirectory` is set to `site`, because MkDocs writes the generated static files there by default.
