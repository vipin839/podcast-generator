# Podcast Generator by Vipin

A GitHub Action that converts a readable YAML podcast description into a valid RSS podcast feed (`podcast.xml`).

## How It Works

The action reads `feed.yaml`, generates `podcast.xml`, and commits the generated feed back to your repository. Host your repository with GitHub Pages so podcast apps can access the feed and audio files.

## Setup

### 1. Enable GitHub Pages

In your repository, open **Settings > Pages** and select the `main` branch as the source. GitHub will provide a Pages URL for your repository.

### 2. Create `feed.yaml`

Add a `feed.yaml` file to the root of your repository:

```yaml
title: My Podcast
subtitle: A podcast about interesting topics
author: Your Name
description: A description of your podcast
link: https://your-username.github.io/your-repository/
image: /images/cover.jpg
language: en-us
category: Technology
format: audio/mpeg
item:
  - title: Episode 1
    description: Description of episode 1
    published: Thu, 12 Jan 2023 18:00:00 GMT
    file: /audio/episode-1.mp3
    duration: 00:00:36
    length: "576324"
```

Use the full GitHub Pages URL in `link`. The `image` and episode `file` values are resolved relative to that URL.

### 3. Add a workflow

Create `.github/workflows/generate-feed.yml`:

```yaml
name: Generate Podcast Feed

on:
  push:
    branches:
      - main

permissions:
  contents: write

jobs:
  generate-feed:
    runs-on: ubuntu-latest
    steps:
      - name: Check out repository
        uses: actions/checkout@v4

      - name: Generate podcast feed
        uses: vipin839/podcast-generator@v1.0.0
        with:
          name: github-actions[bot]
          email: github-actions[bot]@users.noreply.github.com
```

The workflow needs `contents: write` so the action can commit `podcast.xml`.

## YAML Fields

| Field | Description |
| --- | --- |
| `title` | Podcast title |
| `subtitle` | Podcast subtitle |
| `author` | Podcast author |
| `description` | Podcast description |
| `link` | GitHub Pages URL |
| `image` | Podcast artwork path |
| `language` | Language code, such as `en-us` |
| `category` | Apple Podcasts category |
| `format` | Audio format, such as `audio/mpeg` |
| `item` | List of podcast episodes |

Each episode requires `title`, `description`, `published`, `file`, `duration`, and `length`.

See the [Apple Podcasts category list](https://podcasters.apple.com/support/1691-apple-podcasts-categories) for valid category names.

## Output

The action creates `podcast.xml` in the repository root. Submit the GitHub Pages URL for this file to your podcast directory or podcast app.

## License

See [LICENSE](LICENSE).
