# GitHub Actions Status Light - AWS IoT

This project is a simple example of how to use AWS IoT to control a status light. The light is controlled by a GitHub Actions workflow that is triggered by a push to a GitHub repository. The workflow publishes a message to an AWS IoT topic that is subscribed to by a python script running on a Raspberry Pi. The python script then changes the color of a status light connected to the Raspberry Pi.

```bash
aws iot create-policy-version \
    --region ap-southeast-2 \
    --policy-name github-builder-Policy \
    --policy-document file://policy.json \
    --set-as-default

curl https://www.amazontrust.com/repository/AmazonRootCA1.pem > certs/root-CA.crt
```