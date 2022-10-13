# Getting Captain working with pytest

Starting from a [simple workflow that runs pytest][workflow-before-captain], we want to

1. 🧪 Ensure pytest produces junit output

```sh
pytest --junit-xml tmp/pytest.xml
```

will produce captain-compatible xml output in `tmp/pytest.xml`.

2. 🔐 Create an API token

Create an API token for your organization within [captain][captain], ([more documentation here][create-api-token]).
The token needs write access.

Add the new token as an action secret to your repository. Conventionally, we call this secret `CAPTAIN_API_TOKEN`.

3. 💌 Add a step to upload to captain

```yaml
- name: Upload test results to Captain # optional, shows in github action results
  uses: rwx-research/upload-captain-artifact@v1
  if: always() # run even if build fails
  continue-on-error: true # if upload to captain fails, don't fail the build
  with:
    artifacts: |
      [
        {
          "name": "pytest",
          "path": "tmp/pytest.xml",
          "kind": "test_results",
          "parser": "junit_xml"
        }
      ]
    captain-token: '${{ secrets.CAPTAIN_API_TOKEN }}'
```

4. 🎉 See your test results in Captain!

Take a look at the [final workflow!][workflow-with-captain]

For more information on the github action, see [its readme][action-readme].

[workflow-before-captain]: https://github.com/captain-examples/pytest/blob/basic-workflow/.github/workflows/ci.yml
[captain]: https://captain.build/_/organizations
[create-api-token]: https://www.rwx.com/captain/docs/api-tokens#generating-an-api-token
[workflow-with-captain]: https://github.com/captain-examples/pytest/blob/main/.github/workflows/ci.yml
[action-readme]: https://github.com/rwx-research/upload-captain-artifact/#readme
