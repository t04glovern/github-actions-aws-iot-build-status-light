# GitHub Actions Status Light - AWS IoT

This project is a simple example of how to use AWS IoT to control a status light. The light is controlled by a GitHub Actions workflow that is triggered by a push to a GitHub repository. The workflow publishes a message to an AWS IoT topic that is subscribed to by a python script running on a Raspberry Pi. The python script then changes the color of a status light connected to the Raspberry Pi.

```bash
# Copy certs to raspberry pi
scp ./connect_device_package.zip github-builder@github-builder.local:~/connect_device_package.zip

# Clone repo and unzip certs
sudo apt update && sudo apt install git -y
git clone git@github.com:t04glovern/github-actions-aws-iot-build-status-light.git
cd github-actions-aws-iot-build-status-light && mkdir certs
unzip ~/connect_device_package.zip -d certs
curl https://www.amazontrust.com/repository/AmazonRootCA1.pem > certs/root-CA.crt
```

Replace the policy that was setup with single-device provisioning with our own policy. Run this command from a system with the AWS CLI installed on it.

```bash
aws iot create-policy-version \
    --region ap-southeast-2 \
    --policy-name github-builder-Policy \
    --policy-document file://policy.json \
    --set-as-default
```
