# INT6181 Tutorial 7

Tutorial 7: Automate Tests with GitHub Actions

In this tutorial you will set up a GitHub Actions workflow so the provided tests run automatically on every pull request.

The modules under `tutorial_7` (`password_validator.py`, `triangle.py`, `parking_fee.py`) and all `test_*` test files are already provided in this repository on GitHub.

---

## Task: Set Up GitHub Actions

Create a GitHub Actions workflow that runs the provided tests on pull requests.

Inject some bugs to the implementations of the source files and open a pull request to observe what happens.

---

## Expected Results

![Expected Result](expected_results.png)

---

## Completed GitHub Actions setup

The workflow is [`.github/workflows/tests.yml`](.github/workflows/tests.yml). It uses Python 3.12 on `ubuntu-latest`, runs on every pull request, runs on pushes to `main`, and can be started manually from the Actions tab. The tests use Python's standard-library `unittest`; no extra packages are needed.

Run the same tests locally from the repository root:

```sh
python -m unittest discover -s . -p 'test_*.py' -v
```

## Verified tutorial experiment

| Revision | Result | Evidence |
| --- | --- | --- |
| Correct implementations on `main` | 14 tests passed | [Successful baseline run](https://github.com/wangnb002004-hue/int6181-tutorial-7/actions/runs/42027488761) |
| `demo/intentional-bugs` pull request | 14 tests run: 11 passed, 3 failed; exit code 1 | [Failed pull-request run](https://github.com/wangnb002004-hue/int6181-tutorial-7/actions/runs/42027630246) |

[Pull request #1](https://github.com/wangnb002004-hue/int6181-tutorial-7/pull/1) deliberately introduces three bugs without modifying any provided test file:

- Reject valid 8-character passwords by raising the minimum length to 9: `test_valid_passwords` fails.
- Classify scalene triangles as isosceles: `test_valid_triangles` fails.
- Round partial billable parking hours down instead of up: `test_valid_fee_calculation` fails for 1.5 hours ($0 instead of $10).

The failed PR check is the expected outcome: GitHub Actions detects the regressions automatically. Keep this demonstration PR unmerged so `main` retains the correct implementations. Reverting those three changes would make all 14 tests pass again.
