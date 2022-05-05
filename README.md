# Lambda Rust Sample

## Build

```bash
cargo lambda build --release --target x86_64-unknown-linux-gnu
cd ./target/lambda/lambda_rust_sample
chmod 755 bootstrap
zip bootstrap.zip bootstrap
cd -
```

## Run

```bash
docker-compose up -d

aws lambda create-function --function-name LambdaRustSample \
  --handler bootstrap \
  --zip-file fileb://./target/lambda/lambda_rust_sample/tmp/bootstrap.zip \
  --runtime provided.al2 \
  --role arn:aws:iam::XXXXXXXXXXXXX:role/your_lambda_execution_role \
  --endpoint-url=http://localhost:4566

aws lambda invoke \
  --cli-binary-format raw-in-base64-out \
  --function-name LambdaRustSample \
  --payload '{"firstName": "yutaka0m"}' \
  --endpoint-url=http://localhost:4566 \
  output.json
  
cat output.json | jq
```

## References

- [(GitHub)aws-lambda-rust-runtime](https://github.com/awslabs/aws-lambda-rust-runtime)
- [(AWS)AWS Lambda のカスタムランタイム](https://docs.aws.amazon.com/ja_jp/lambda/latest/dg/runtimes-custom.html)