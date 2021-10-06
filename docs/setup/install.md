---
id: install
title: Install Rowy
slug: /setup/install
---

## Option 1: Quick deploy – ✅ Recommended

You can easily set up Rowy on your Google Cloud or Firebase project using the
following one-click deploy button, which will guide you with a step-by-step
process.

:::caution Required

Before starting, make sure you have a Google Cloud or Firebase project with
**Firestore** and **Firebase Authentication** enabled.  
Don’t have a project? [Learn how to create one&nbsp;→](./firebase-project.md)

:::

Click the button 👇

[![Run on Google Cloud](https://deploy.cloud.run/button.svg)](https://deploy.cloud.run/?git_repo=https://github.com/rowyio/rowyRun.git)

This runs Rowy Run, an open source Cloud Run instance that operates exclusively
on your GCP project, so we never have access to any of your data. Like Rowy,
it’s completely open source and free to use.
[Learn more about Rowy Run](../rowy-run/overview.mdx)

Once installed, you’ll be given an URL to the Rowy app to continue installation:

![Cloud Run Output](./assets/cloud-run.png)

## Option 2: Manual install

If you don’t want to set up Rowy as described above, you can follow these steps
to install it manually for development. You should be familiar with

### Requirements

- [Git](https://git-scm.com/downloads)
- [Node](https://nodejs.org/en/download/) 10+
- [Yarn](https://classic.yarnpkg.com/en/docs/install/) 1
- [Firebase CLI](https://firebase.google.com/docs/cli) 8+

1. Make sure you’e logged in to your Firebase account in the Firebase CLI:

   ```bash
   firebase login
   ```

2. Clone this repo.

   ```bash
   git clone https://github.com/rowyio/rowy.git
   ```

3. Set environment variables.

   - Create a `.env` file.
   - Get the **Project ID** and **Web API key** in the
     [Firebase Console&nbsp;&UpperRightArrow;](https://console.firebase.google.com/project/_/settings/general)
   - Paste them in the `.env` file:
     ```bash
     REACT_APP_FIREBASE_PROJECT_ID =
     REACT_APP_FIREBASE_PROJECT_WEB_API_KEY =
     ```

4. Install frontend dependencies using Yarn.

   ```bash
   yarn
   ```

5. Run the app locally.

   ```bash
   yarn start
   ```
