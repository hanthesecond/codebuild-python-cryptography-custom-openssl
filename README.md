# codebuild-python-cryptography-custom-openssl
As of this writing, the default [AWS Lambda][2] runtime includes OpenSSL 1.0.1k-fips.  There
are many use cases where a newer version of OpenSSL is desirable or required, but this is
complicated for Python developers both by the build required as well as the integration of
that specific build with the [cryptography library][1].

This repository contains example resources to use the [AWS CodeBuild][3] service to build a
Lambda-deployable zip file which contains the cryptography library built against a specific
desired version of OpenSSL.

Built libraries are built into a "site-packages" directory, which must be added to `sys.path`
in the Lambda Python module referencing those libraries.

The `OPENSSL_VERSION` environment variable can be set to the desired OpenSSL version.  In the
supplied build script, it defaults to `1.1.0d`.

This example build can be extended with your project's build requirements or tiered with [AWS
CodePipeline][4] to feed into your other builds.

## Contents

### codebuild.yml
Cloudformation template to set up CodeBuild resources to build this repo.

The __GithubSourceLocation__ parameter should be in the form of `https://<username>:<personal_token>@github.com/<path_to_repo.git>`

### buildspec.yml
CodeBuild build specification file.

### build_cryptography_custom_openssl.sh
Shell script to handle custom OpenSSL compilation and cryptography wheel build.

### lambda_entry.py
Lambda Function code.  Prints out the version of OpenSSL used.

[1]: https://cryptography.io/
[2]: https://aws.amazon.com/lambda/
[3]: https://aws.amazon.com/codebuild/
[4]: https://aws.amazon.com/codepipeline/