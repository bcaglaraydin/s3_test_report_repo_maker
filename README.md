# Convert and Push Test Report to S3

This GitHub Action automates the process of converting JUnit XML test reports to HTML format, renaming the file, and pushing it to an Amazon S3 bucket. This can be useful for maintaining organized and easily accessible test reports in your S3 storage.

## Inputs

- **s3-bucket-base-path** (required): Base path of the S3 bucket.
- **s3-path** (required): Path within the S3 bucket where the converted test report will be stored.
- **version** (optional): Version identifier. Defaults to the GitHub reference name (`github.ref_name`).
- **test-report-filename** (optional): Filename of the JUnit XML test report. Defaults to `test_report.xml`.

## Example Usage

```yaml
name: Convert and upload unit test report

on:
  push:
    tags:
      - "*"

jobs:
  build-and-run-docker:
    runs-on: ubuntu-latest

    env:
      TEST_REPORT_FILE: test_report.xml
      VERSION: ${{ github.ref_name }}
      S3_BUCKET: s3://your-s3-bucket-name
      S3_PATH: your-s3-path
      AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
      AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
      AWS_DEFAULT_REGION: us-east-1

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Convert and Push Test Report to S3
        uses: bcaglaraydin/s3_test_report_repo_maker@main
        with:
          s3-bucket-base-path: ${{ env.S3_BUCKET }}
          s3-path:  ${{ env.S3_PATH }}
          version: ${{ env.VERSION }}
          test-report-filename: ${{ env.TEST_REPORT_FILE }}
        if: always()
