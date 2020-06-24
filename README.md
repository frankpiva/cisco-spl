# cisco-spl

## Information
The following commands provision an ELB in front of an EC2 with a security group allowing traffic on port 80.

This solution relies on the Amazon default VPC existing. If no default VPC exists, you may receive the following error:
```
No default VPC for this user (Service: AmazonEC2; Status Code: 400; Error Code: VPCIdNotSpecified; Request ID: 6d0d8b96-31ed-4c1a-a125-fc935da365aa)
```

A live example can be viewed [here](http://cisco-spl-1259915956.us-east-1.elb.amazonaws.com/).

## Deploy
1. Configure your command line credentials.

    AWS CLI: `aws configure`

    Windows: `SET AWS_PROFILE=yourProfile`

    Unix: `export AWS_PROFILE=yourProfile`

2. Verify your credentials.
    ```
    aws sts get-caller-identity

    {
        "UserId": "ABCDEFGHIJK1234567890:user@domain.com",
        "Account": "12345678901234",
        "Arn": "arn:aws:sts::12345678901234:assumed-role/role/user@domain.com"
    }
    ```


3. Clone the repository, execute the deploy command, and wait for the stack to provision.

    ```
    git clone https://github.com/frankpiva/cisco-spl.git; cd cisco-spl/
    aws cloudformation deploy --region us-east-1 --stack-name cisco-spl --template-file cisco-spl.json

    Waiting for changeset to be created..
    Waiting for stack create/update to complete
    Successfully created/updated stack - cisco-spl
    ```

4. Attempt to redeploy using the same command.

    ```
    aws cloudformation deploy --region us-east-1 --stack-name cisco-spl --template-file cisco-spl.json

    Waiting for changeset to be created..

    No changes to deploy. Stack cisco-spl is up to date
    ```

5. Test the stack end-to-end.

    **Note**:  It may take a few minutes for the load balancer to rotate the instance into service after passing the health check.

    ```
    aws elb describe-load-balancers --load-balancer-names cisco-spl --region us-east-1

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
