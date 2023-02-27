# Getting Captain working with pytest

Starting from a [simple workflow that runs pytest][workflow-before-captain], we want to

## 1. 🧪 Ensure pytest produces its reportlog output

The native pytest JUnit XML is insufficient for Captain's functionality. In place of it, we recommend installing [pytest-reportlog](https://github.com/pytest-dev/pytest-reportlog). Once installed, ensure pytest produces compatible output with:

```sh
pytest --report-log=tmp/log.json
```

## 2. 🔐 Create an Access Token

Create an Access Token for your organization within [Captain][captain] ([more documentation here][create-access-token]).

Add the new token as an action secret to your repository. Conventionally, we call this secret `RWX_ACCESS_TOKEN`.

## 3. 💌 Install the Captain CLI and call it when running tests

See the [full documentation on test suite integration][test-suite-integration].


```yaml
- uses: rwx-research/setup-captain@v1
- name: Run tests
  run: |
    captain run \
      --suite-id captian-examples-pytest \
      --test-results tmp/log.json \
      -- pytest --report-log=tmp/log.json
  env:
    RWX_ACCESS_TOKEN: ${{ secrets.RWX_ACCESS_TOKEN }}
```

## 4. 🎉 See your test results in Captain!

Take a look at the [final workflow!][workflow-with-captain]

[workflow-before-captain]: https://github.com/captain-examples/pytest/blob/basic-workflow/.github/workflows/ci.yml
[captain]:  https://account.rwx.com/deep_link/manage/access_tokens
[create-access-token]:  https://www.rwx.com/docs/access-tokens
[workflow-with-captain]: https://github.com/captain-examples/pytest/blob/main/.github/workflows/ci.yml
[test-suite-integration]: https://www.rwx.com/captain/docs/test-suite-integration
