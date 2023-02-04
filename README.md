# Intro to CI for ML

## Step1:
*   git the dataset and train the model
*   create readme.md file
*   create the requirements.txt and specify the required libraries+onstall it
*   after all push the entire code to github


## Step2:
*   Open the repository in github and create a file => .github/workflows/cml.yaml
*   Got to the link  [CML](https://github.com/iterative/cml) and copy the given code and finally past it to cml.yaml file
```bash
name: your-workflow-name
on: [push]
jobs:
  run:
    runs-on: ubuntu-latest
    # optionally use a convenient Ubuntu LTS + DVC + CML image
    # container: ghcr.io/iterative/cml:0-dvc2-base1
    steps:
      - uses: actions/checkout@v3
      # may need to setup NodeJS & Python3 on e.g. self-hosted
      # - uses: actions/setup-node@v3
      #   with:
      #     node-version: '16'
      # - uses: actions/setup-python@v4
      #   with:
      #     python-version: '3.x'
      - uses: iterative/setup-cml@v1
      - name: Train model
        run: |
          # Your ML workflow goes here
          pip install -r requirements.txt
          python train.py
      - name: Write CML report
        env:
          REPO_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        run: |
          # Post reports as comments in GitHub PRs
          cat results.txt >> report.md
          cml comment create report.md
```

