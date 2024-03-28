# Overview

This project is a fork from the original [Textualize/Rich PyPI library](https://github.com/Textualize/rich). Group 1 of CS489 (Winter 2024) at the University of Waterloo has selected this open source project to improve its software delivery practices. The details of the improvements are submitted as a report to the grading team. This README serves the purpose to outline the commands that need to be executed for the project setup and experiments.

## Initial setup
1. Make sure you have Python (>= 3.7.0) and pip installed on your machine.
2. Make sure to have Poetry installed. If not, run `pip install poetry` or follow any other recommended methods.
3. Install dependencies using poetry
```
poetry install
```
4. Boot up a poetry shell (Use the poetry virtual env to execute all the successive commands)
```
poetry shell
```
5. Run pre-commit setup script to configure the hook
```
make setup-pre-commit
```

## Experiment - Automated build and release pipeline

Make sure to complete the **Common pre-build steps** prior to proceeding to test each of the experiment groups.

### Control (Manual process)
1. Check out to `master` and pull latest `master` from remote repository
```
git checkout master
```
```
git pull
```
2. Run the build
```
poetry build
```
3. Configure devpi-client
```
devpi use http://3.140.87.9:80
```
```
devpi login testuser --password=testusercs489
```
```
devpi use testuser/cs489-test-index
```
4. Upload to devpi-server
```
devpi upload --from-dir dist/*
```
5. Follow the **Creating release on Github** steps to create a new release

### Experimental (Automated CI/CD workflow process)
1. Go to **Actions** -> **Build, Publish, and Release Rich** workflow tab on the repository
2. Select the workflow triggered by your merge/push to `master`
3. Click on `Review deployments` and select `deployment` environment from options
4. Click `Approve and deploy` to approve and run the workflow
5. Wait for the job to finish and then check the Releases section to verify the release.


## Experiment - Benchmarking

Details about Benchmarking can be found in [this README.md](https://github.com/Taseen08/rich/tree/master/benchmarks#readme) file. A more in-depth process of replicating the 2 workflows can be seen in the Replicability section of Improvement 2 (Monitoring - Improve Benchmarks) under the Evaluation Report.

Manual Process:

1. Ensure any tags you wish to benchmark are included in the file `asvhashfile` at the root of the repo.
2. Run the benchmarks for those tags by running `asv run HASHFILE:asvhashfile`. This will take several minutes.
3. Create the HTML locally for those benchmarks by running `asv publish`.
4. Run `asv preview` to launch a local webserver that will let you preview the benchmarks dashboard. Navigate to the URL this command gives you and check everything looks fine.
5. Checkout the `rich-benchmarks` repo from [here](https://github.com/Textualize/rich-benchmarks) and `cd` into it.
6. Copy the HTML you generated earlier into the root of this repo, e.g. `cp -r ../rich/benchmarks/html/* .` (assuming you checked out `rich-benchmarks` alongside `rich` in your filesystem)
7. When the HTML is merged into `main`, the [benchmark dashboard](https://textualize.github.io/rich-benchmarks/) will be updated via a GitHub Action.

Automated Process:

1. Run `./run_benchmarks.sh <tag_name>` where `tag_name` is the new tag you want to monitor using asv.
2. When your changes are pushed to the master branch, the benchmarking dashboard will be updated automatically via a GitHub Action.

<!-- This is a test, no need to translate -->
