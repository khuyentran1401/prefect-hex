# prefect-hex

<p align="center">
    <a href="https://pypi.python.org/pypi/prefect-hex/" alt="PyPI version">
        <img alt="PyPI" src="https://img.shields.io/pypi/v/prefect-hex?color=0052FF&labelColor=090422"></a>
    <a href="https://github.com/PrefectHQ/prefect-hex/" alt="Stars">
        <img src="https://img.shields.io/github/stars/PrefectHQ/prefect-hex?color=0052FF&labelColor=090422" /></a>
    <a href="https://pepy.tech/badge/prefect-hex/" alt="Downloads">
        <img src="https://img.shields.io/pypi/dm/prefect-hex?color=0052FF&labelColor=090422" /></a>
    <a href="https://github.com/PrefectHQ/prefect-hex/pulse" alt="Activity">
        <img src="https://img.shields.io/github/commit-activity/m/PrefectHQ/prefect-hex?color=0052FF&labelColor=090422" /></a>
    <br>
    <a href="https://prefect-community.slack.com" alt="Slack">
        <img src="https://img.shields.io/badge/slack-join_community-red.svg?color=0052FF&labelColor=090422&logo=slack" /></a>
    <a href="https://discourse.prefect.io/" alt="Discourse">
        <img src="https://img.shields.io/badge/discourse-browse_forum-red.svg?color=0052FF&labelColor=090422&logo=discourse" /></a>
</p>

## Welcome!

Prefect integrations for interacting with Hex. 

Hex is a powerful platform for collaborative data science and analytics. For information getting started with Hex, check out Hex's [quickstart guide](https://learn.hex.tech/quickstart).

The tasks within this collection were created by a code generator using Hex's OpenAPI spec.

Hex's REST API documentation can be found [here](https://learn.hex.tech/docs/develop-logic/hex-api/api-reference).

## Getting Started

### Python setup

Requires an installation of Python 3.7+.

We recommend using a Python virtual environment manager such as pipenv, conda or virtualenv.

These tasks are designed to work with Prefect 2.0. For more information about how to use Prefect, please refer to the [Prefect documentation](https://orion-docs.prefect.io/).

### Installation

Install `prefect-hex` with `pip`:

```bash
pip install prefect-hex
```

Then, register to [view the block](https://orion-docs.prefect.io/ui/blocks/) on Prefect Cloud:

```bash
prefect block register -m prefect_hex
```

Note, to use the `load` method on Blocks, you must already have a block document [saved through code](https://orion-docs.prefect.io/concepts/blocks/#saving-blocks) or [saved through the UI](https://orion-docs.prefect.io/ui/blocks/).

### Gather and store authentication

1. Create new token on https://app.hex.tech/ Settings page:

![image](https://user-images.githubusercontent.com/15331990/201996947-07765380-50c4-4c61-9044-bd93e4b8efc7.png)

2. Store token on https://app.prefect.cloud/ Blocks page:

![image](https://user-images.githubusercontent.com/15331990/201997292-b3a18254-229f-4689-aaec-07a990cdaf87.png)

3. Copy project ID from browser URL (in red):

![image](https://user-images.githubusercontent.com/15331990/202002588-55a895b2-de89-438f-ac96-c86940946336.png)

### Write and run a flow

#### Trigger a Hex project run and wait for completion
```python
from prefect import flow
from prefect_hex import HexCredentials
from prefect_hex.project import trigger_project_run_and_wait_for_completion

@flow
def trigger_project_run_and_wait_for_completion_flow(project_id: str):
    hex_credentials = HexCredentials.load("hex-token")
    project_metadata = trigger_project_run_and_wait_for_completion(
        project_id=project_id,
        hex_credentials=hex_credentials
    )
    return project_metadata

trigger_project_run_and_wait_for_completion_flow(
    project_id="012345c6-b67c-1234-1b2c-66e4ad07b9f3"
)
```

#### Run project, get status, cancel run, and get list of projects
```python
from prefect import flow
from prefect_hex import HexCredentials
from prefect_hex.project import (
    get_project_runs,
    run_project,
    get_run_status,
    cancel_run,
)

@flow
def example_hex_flow():
    # load stored credentials
    hex_credentials = HexCredentials.load("hex-token")

    # run project
    project_id='5a8591dd-4039-49df-9202-96385ba3eff8',
    project_run = run_project(project_id=project_id, hex_credentials=hex_credentials)

    # get status
    run_id = project_run.run_id
    project_run_status = get_run_status(
        project_id=project_id, run_id=run_id, hex_credentials=hex_credentials
    )
    print(project_run_status.run_url)

    # cancel run if needed
    cancel_run(project_id=project_id, run_id=run_id, hex_credentials=hex_credentials)

    # get list of project runs
    project_runs = get_project_runs(
        project_id=project_id, hex_credentials=hex_credentials
    )

    return project_runs

example_hex_flow()
```

## Resources

If you encounter any bugs while using `prefect-hex`, feel free to open an issue in the [prefect-hex](https://github.com/PrefectHQ/prefect-hex) repository.

If you have any questions or issues while using `prefect-hex`, you can find help in either the [Prefect Discourse forum](https://discourse.prefect.io/) or the [Prefect Slack community](https://prefect.io/slack).

## Development

If you'd like to install a version of `prefect-hex` for development, clone the repository and perform an editable install with `pip`:

```bash
git clone https://github.com/PrefectHQ/prefect-hex.git

cd prefect-hex/

pip install -e ".[dev]"

# Install linting pre-commit hooks
pre-commit install
```
