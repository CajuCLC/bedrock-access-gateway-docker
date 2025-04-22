# Bedrock Access Gateway Docker

This repository provides a Dockerized version of the AWS Bedrock Access Gateway, allowing seamless access to Amazon Bedrock models using OpenAI-compatible APIs and SDKs. This setup enables you to experiment with Amazon Bedrock without modifying your codebase, making it easy to evaluate the capabilities of various foundation models.

## Docker Image

The Docker image is available at `ghcr.io/cajuclc/bedrock-access-gateway-docker` and can be pulled using the following command:

```bash
docker pull ghcr.io/cajuclc/bedrock-access-gateway-docker:v0.0.1
```

## Running the Docker Container

To run the Docker container:

```bash
docker run -d --name bedrock_gateway \
  -e AWS_ACCESS_KEY_ID=your_access_key \
  -e AWS_SECRET_ACCESS_KEY=your_secret \
  -e AWS_REGION=aws_region \
  -p 8081:80 \
  ghcr.io/cajuclc/bedrock-access-gateway-docker
```

### Environment Variables

- `AWS_ACCESS_KEY_ID`: Your AWS access key ID.
- `AWS_SECRET_ACCESS_KEY`: Your AWS secret access key.
- `AWS_REGION`: The AWS region to use.
- `AWS_SESSION_TOKEN`: (Optional) Your AWS session token, required only if you are using temporary security credentials.

## Integration with Open-WebUI

To configure the model in Open-WebUI or similar interfaces:

- Under the Admin Panel, go to **Settings -> Connections**.
- Use the "+" (plus) button to add a new connection under the **OpenAI**.
- For the URL, use `http://YOUR_IP:8081/api/v1` (Replace `YOUR_IP` with the appropriate IP address or hostname of your server).
- For the password, the default password defined in BAG is `bedrock`. You can always change this password in the BAG settings (see `DEFAULT_API_KEYS`).
- Click the "Verify Connection" button and you should see a "Server connection verified" alert in the top-right.

### Authentication

Use the API key provided in `DEFAULT_API_KEYS`. By default, it is set to `bedrock`. 

If you wish to use a different password, you can set it by passing the `DEFAULT_API_KEYS` environment variable when running the Docker container:

```bash
docker run -d --name bedrock_gateway \
  -e AWS_ACCESS_KEY_ID=your_access_key \
  -e AWS_SECRET_ACCESS_KEY=your_secret \
  -e AWS_REGION=aws_region \
  -e DEFAULT_API_KEYS=your_custom_api_key \
  -p 8081:80 \
  ghcr.io/cajuclc/bedrock-access-gateway-docker
```

This allows you to customize the API key required for accessing the service.

## Configuration in `api/setting.py`

If you need to change the default API key from `bedrock` to another value, ensure the `api/setting.py` file is appropriately configured to read from environment variables. Adjust the following line as shown below:

```python
import os

DEFAULT_API_KEYS = os.environ.get("DEFAULT_API_KEYS", "bedrock")
```

This change allows you to pass a different API key using an environment variable when running the Docker container, providing flexibility in setting authentication credentials.
```
