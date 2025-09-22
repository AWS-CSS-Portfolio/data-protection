# 1. Create a test file
echo "Testing encryption and TLS enforcement" > test-deny.txt

# 2. Secure upload (default HTTPS, should succeed)
aws s3 cp .\test-deny.txt s3://dp-lab-secure-bucket/

# 3. Insecure upload (HTTP endpoint, should fail with AccessDenied)
aws s3 cp .\test-deny.txt s3://dp-lab-secure-bucket/ --endpoint-url http://s3.us-east-1.amazonaws.com

# 4. Wrong encryption type (AES256, bucket should override and enforce CMK)
aws s3 cp .\test-deny.txt s3://dp-lab-secure-bucket/ --sse AES256

# 5. Correct CMK encryption (explicitly use your CMK ARN, should succeed)
aws s3 cp .\test-deny.txt s3://dp-lab-secure-bucket/ --sse aws:kms --sse-kms-key-id arn:aws:kms:us-east-1:xxxxxxxxxxxx:key/46d4dd46-xxxx-xxxx-xxxx-xxxxxxxxxxxx
