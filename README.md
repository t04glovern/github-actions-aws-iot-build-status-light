# GitHub Actions Status Light - AWS IoT

This project is a simple example of how to use AWS IoT to control a status light. The light is controlled by a GitHub Actions workflow that is triggered by a push to a GitHub repository. The workflow publishes a message to an AWS IoT topic that is subscribed to by a python script running on a Raspberry Pi. The python script then changes the color of a status light connected to the Raspberry Pi.

```bash
# Copy certs to raspberry pi
scp ./connect_device_package.zip github-builder@github-builder.local:~/connect_device_package.zip

# Clone repo and unzip certs
sudo apt update && sudo apt install git
git clone git@github.com:t04glovern/github-actions-aws-iot-build-status-light.git
cd github-actions-aws-iot-build-status-light && mkdir certs
unzip ~/connect_device_package.zip -d certs
curl https://www.amazontrust.com/repository/AmazonRootCA1.pem > certs/root-CA.crt

# Setup python environment with awsiot package
sudo apt install python3-pip
pip3 install -r requirements.txt
```

Replace the policy that was set up with single-device provisioning with our policy. Run this command from a system with the AWS CLI installed on it.

```bash
aws iot create-policy-version \
    --region ap-southeast-2 \
    --policy-name github-builder-Policy \
    --policy-document file://policy.json \
    --set-as-default
```

Test the connection to AWS IoT

```bash
/usr/bin/python3 /home/github-builder/github-actions-aws-iot-build-status-light/main.py \
    --endpoint a3fwrozawgvob8-ats.iot.ap-southeast-2.amazonaws.com \
    --ca_file /home/github-builder/github-actions-aws-iot-build-status-light/certs/root-CA.crt \
    --cert /home/github-builder/github-actions-aws-iot-build-status-light/certs/github-builder.cert.pem \
    --key /home/github-builder/github-actions-aws-iot-build-status-light/certs/github-builder.private.key \
    --client_id github-builder \
    --topic t04glovern/github-builder
```

Install the systemd service

```bash
sudo cp github-builder /etc/systemd/system/github-builder
sudo systemctl daemon-reload
sudo systemctl enable github-builder
sudo systemctl start github-builder
```
