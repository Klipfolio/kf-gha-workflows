# Slack Notify Action

Every GitHub Action Workflow job here at Klipfolio has final steps to Slack notify of the run results. We have two steps configured that notify to different channels:
  1. All job run results go to a build channel
  2. Failed run results go to a CI/CD alerts channel (action required)

This composite GitHub action combines these two steps into a single step and makes use of [8398a7/action-slack](https://github.com/8398a7/action-slack).

## Usage

<!-- start usage -->
```yml
- name: Checkout actions repo
uses: actions/checkout@v3
with:
    repository: Klipfolio/kf-github-actions
    path: kf-github-actions
    ref: slack-notify

- name: Slack Notify
uses: ./kf-github-actions/.github/actions/slack-notify
with: 
    job_name: Build and Test
    always_notify_channel_webhook_url: ${{ secrets.SLACK_WEBHOOK_ALL_BUILDS_NOTIFY_URL }}
    failure_notify_channel_webhook_url: ${{ secrets.SLACK_WEBHOOK_FAILURE_NOTIFY_URL }}
```
<!-- end usage -->
