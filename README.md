# cisco-spl

## Deploy
1. Configure your command line credentials.

    AWS CLI: `aws configure`

    Windows: `SET AWS_PROFILE=yourProfile`

    Unix: `export AWS_PROFILE=yourProfile`

2. Execute the deploy command, wait for the stack to provision.

    ```
    aws cloudformation deploy --region us-east-1 --stack-name cisco-spl --template-file cisco-spl.json

    Waiting for changeset to be created..
    Waiting for stack create/update to complete
    Successfully created/updated stack - cisco-spl
    ```

3. Attempt to redeploy using the same command.

    ```
    aws cloudformation deploy --region us-east-1 --stack-name cisco-spl --template-file cisco-spl.json

    Waiting for changeset to be created..

    No changes to deploy. Stack cisco-spl is up to date
    ```

4. Test the stack end-to-end.

    **Note**:  It may take a few minutes for the load balancer to rotate the instance into service after passing the health check.

    ```
    aws --region us-east-1 elb describe-load-balancers --load-balancer-names cisco-spl

    {
    "LoadBalancerDescriptions": [
        {
            "LoadBalancerName": "cisco-spl",
            "DNSName": "cisco-spl-1259915956.us-east-1.elb.amazonaws.com",
            . . .

    curl cisco-spl-1259915956.us-east-1.elb.amazonaws.com
    curl: (6) Could not resolve host: cisco-spl-1259915956.us-east-1.elb.amazonaws.com
    curl cisco-spl-1259915956.us-east-1.elb.amazonaws.com
    <html><head><title>Cisco SPL</title></head><body><h1><center>Cisco SPL</center></h1></body></html>
    ```
