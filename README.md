# Example app: github-issue-autolabel

This app fetches Github open issues for a given repo (via `config.edn`) and
updates triage labels periodically.

## Usage

You need a Unix-compatible (macOS, Linux or Windows [WSL](https://learn.microsoft.com/en-us/windows/wsl/install))
system to run this app. Open a terminal and execute the steps below:

To run this app you need to define these env vars:

```shell
export OPENAI_API_KEY="FIXME" # your OpenAI API key
export GITHUB_OWNER="FIXME"   # Github owner/org of the target Github repo
export GITHUB_REPO="FIXME"    # Github repo name of the target Github repo
export GITHUB_TOKEN="FIXME"   # Github access token for the target Github repo
```

Note:
- To create a Github token visit https://github.com/settings/tokens?type=beta
  and create a token with permissions to access repository issues.

You may run this app without making a local copy first (i.e. clone the repo)
using Docker as follows:


```shell
docker run --rm \
  -p "0.0.0.0:8080:8080" \
  -v $PWD:/agentlang \
  -e OPENAI_API_KEY="$OPENAI_API_KEY" \
  -e GITHUB_OWNER="$GITHUB_OWNER" \
  -e GITHUB_REPO="$GITHUB_REPO" \
  -e GITHUB_TOKEN="$GITHUB_TOKEN" \
  -it agentlang/agentlang.cli:latest \
  agent clonerun https://github.com/agentlang-hub/github-issue-autolabel.git
```

### Running a local copy of the app

If you have cloned the app and made a local copy on your computer,
you may run it using Docker as follows:

```shell
docker run --rm \
  -p "0.0.0.0:8080:8080" \
  -v $PWD:/agentlang \
  -e OPENAI_API_KEY="$OPENAI_API_KEY" \
  -e GITHUB_OWNER="$GITHUB_OWNER" \
  -e GITHUB_REPO="$GITHUB_REPO" \
  -e GITHUB_TOKEN="$GITHUB_TOKEN" \
  -it agentlang/agentlang.cli:latest \
  agent run
```

Alternatively, instead of Docker you may use the locally installed
[Agentlang CLI](https://github.com/agentlang-ai/agentlang.cli):

```shell
agent run
```

### Getting the app to autolabel the issues

Once the app is running, it automatically fetches all open issues and
assigns triage labels. Also, it keeps repeating the process once every
hour (check the source code for the interval.)

Should you want to inspect what is happening under the hood, you may
check the logs as follows:

```shell
tail -f logs/agentlang.log
```

### Example Github issues - Input/Output data

The environment variables `GITHUB_OWNER` and `GITHUB_REPO` in the file
`config.edn` point to the Github repository where the issues need to be
analysed. The AI-Agent reads the open issues and generates labels for
them periodically. Below is a sample of issues and the corresponding
labels generated by the AI-agent.

**Legend:** The labels are generated for
- *Severity:* `Critical`, `Major`, `Minor` or `Low`
- *Priority:* `Urgent`, `High`, `Moderate`, `Low` or `Negligible`

| Issue column  | Details |
|---------------|---------|
| Issue 1       |---      |
| Issue Title   | Minor issue retrieving order information |
| Issue Body    | Customers do not see the latest image of the product. Need to fix this when possible. |
| AI-gen Labels | `Moderate` `Minor` |
| Issue 2       |---      |
| Issue Title   | Production broken with latest commit |
| Issue Body    | We are seeing a production issue with customers not being able to place orders. This impacts business. |
| AI-gen Labels | `Critical` `Urgent` |
| Issue 3       |---      |
| Issue Title   | Test issue |
| Issue Body    | This is a test issue |
| AI-gen Labels | `Low` `Negligible` |


## License

Copyright 2024-2025 Fractl Inc.

Licensed under the Apache License, Version 2.0:
http://www.apache.org/licenses/LICENSE-2.0

