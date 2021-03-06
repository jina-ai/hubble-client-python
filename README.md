<p align="center">
    <a href="https://jina.ai/">
        <img src="https://github.com/jina-ai/hubble-client-python/blob/main/.github/logos/hubble-colorful.png?raw=true" alt="Hubble python SDK logo" width="200px">
    </a>
</p>

<p align="center">
    <b>Hubble Python SDK: Talk with Hubble in a Pythonic Way</b>
</p>

<p align="center">
    <a href="https://codecov.io/gh/jina-ai/hubble-client-python">
        <img src="https://codecov.io/gh/jina-ai/hubble-client-python/branch/main/graph/badge.svg?token=Sttz9HTmDq"/>
    </a>
</p>



## Install

```shell
pip install jina-hubble-sdk
```

## Core functionality

* Authentication and token management.
* Artifact management.

## Usage

### Login to Hubble

```python
import hubble

# Open browser automatically and login via 3rd party.
# Token will be saved locally.
hubble.login()
```

### Logout

```python
import hubble

# If there is a valid token locally, 
# this will disable that token and remove it from local config.
hubble.logout()
```

### Authentication and Token Management

After calling `hubble.login()`, you can use the client with:

```python
import hubble

client = hubble.Client(
    max_retries=None,
    jsonify=True
)
# Get current user information.
response = client.get_user_info()
# Create a new personally access token for longer expiration period.
response = client.create_personal_access_token(
    name='my-pat',
    expiration_days=30
)
# Query all personal access tokens.
response = client.list_personal_access_tokens()
```

### Artifact Management
```python
import hubble
import io

client = hubble.Client(
    max_retries=None,
    jsonify=True
)

# Upload artifact to Hubble Artifact Storage by providing path.
response = client.upload_artifact(
    f='~/Documents/my-model.onnx',
    is_public=False
)

# Upload artifact to Hubble Artifact Storage by providing `io.BytesIO`
response = client.upload_artifact(
    f=io.BytesIO(b"some initial binary data: \x00\x01"),
    is_public=False
)

# Get current artifact information.
response = client.get_artifact_info(id='my-artifact-id')

# Download artifact to local directory.
response = client.download_artifact(
    id='my-artifact-id',
    f='my-local-filepath'
)
# Download artifact as an io.BytesIO object
response = client.download_artifact(
    id='my-artifact-id',
    f=io.BytesIO()
)

# Get list of artifacts.
response = client.list_artifacts(filter={'metaData.foo': 'bar'}, sort={'type': -1})

# Delete the artifact.
response = client.delete_artifact(id='my-artifact-id')
```

### Error Handling
```python
import hubble

client = hubble.Client()

try:
    client.get_user_info()
except hubble.excepts.AuthenticationRequiredError:
    print('Please login first.')
except Exception:
    print('Unknown error')
```

## Development

### Local test

- Make a new virtual env. `make env`
- Install dependencies. `make init`
- **The test should be run in a logged in environment**. So need to login to Jina. `jina auth login`
- Test locally. `make test`

### Release cycle

- Each time new commits come into `main` branch, CD workflow will generate a new release both on GitHub and Pypi.
- Each time new commits come into `alpha` branch, CD workflow will generate a new pre-release both on GitHub and Pypi.


<!-- start support-pitch -->
## Support

- Use [Discussions](https://github.com/jina-ai/finetuner/discussions) to talk about your use cases, questions, and
  support queries.
- Join our [Slack community](https://slack.jina.ai) and chat with other Jina community members about ideas.
- Join our [Engineering All Hands](https://youtube.com/playlist?list=PL3UBBWOUVhFYRUa_gpYYKBqEAkO4sxmne) meet-up to discuss your use case and learn Jina's new features.
    - **When?** The second Tuesday of every month
    - **Where?**
      Zoom ([see our public events calendar](https://calendar.google.com/calendar/embed?src=c_1t5ogfp2d45v8fit981j08mcm4%40group.calendar.google.com&ctz=Europe%2FBerlin)/[.ical](https://calendar.google.com/calendar/ical/c_1t5ogfp2d45v8fit981j08mcm4%40group.calendar.google.com/public/basic.ics))
      and [live stream on YouTube](https://youtube.com/c/jina-ai)
- Subscribe to the latest video tutorials on our [YouTube channel](https://youtube.com/c/jina-ai)

## Join Us

Hubble Python SDK is backed by [Jina AI](https://jina.ai) and licensed under [Apache-2.0](./LICENSE). [We are actively hiring](https://jobs.jina.ai) AI engineers, solution engineers to build the next neural search ecosystem in opensource.

<!-- end support-pitch -->
