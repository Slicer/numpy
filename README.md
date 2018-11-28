What is this project ?
----------------------

This project is **NOT** the official numpy repository.

It is a fork of numpy sources hosted at https://github.com/numpy/numpy.

The official upstream repository is https://github.com/numpy/numpy.

It is used as staging area to maintain and test patches that will be contributed back to the
official repository.


What is the branch naming convention ?
--------------------------------------

Each branch is named following the pattern `slicer-vY.Y.Z-YYYY-MM-DD-SHA{N}`

where:

* `vX.Y.Z` is the version of the forked project
* `YYYY-MM-DD` is the date of the last official commit associated with the branch.
* `SHA{N}` are the first N characters of the last official commit associated with the branch.

For more details, see https://www.slicer.org/wiki/Documentation/Nightly/Developers/ProjectForks


How to update the version of numpy ?
----------------------------------

1. Clone this repository and add a remote to the official project

```
git clone git://github.com/Slicer/numpy
cd numpy
git remote add upstream git://github.com/numpy/numpy
git fetch upstream
```

2. Create a new branch following the convention

```
# Extract version from https://github.com/numpy/numpy/blob/master/setup.py
X=$(cat setup.py | grep "^MAJOR" | cut -d= -f2 | tr -d " ")
Y=$(cat setup.py | grep "^MINOR" | cut -d= -f2 | tr -d " ")
Z=$(cat setup.py | grep "^MICRO" | cut -d= -f2 | tr -d " ")
numpy_XYZ=$X.$Y.$Z
echo "numpy_XYZ [${numpy_XYZ}]"

numpy_DATE=$(git show -s --format=%ci upstream/master | cut -d" " -f1)
echo "numpy_DATE [${numpy_DATE}]"

numpy_SHA=$(git show -s --format=%h upstream/master)
echo "numpy_SHA: [${numpy_SHA}]"

BRANCH_NAME=slicer-v${numpy_XYZ}-${numpy_DATE}-${numpy_SHA}
echo "BRANCH_NAME [${BRANCH_NAME}]"

git checkout -b ${BRANCH_NAME} ${numpy_SHA}
```

3. Cherry-pick the Slicer specific commits from last branch. Resolve conflict as needed.

   - the commit hash included in each branch name is the numpy *upstream* hash, so
     Slicer specific commits may be viewed with the git range (`..`) operation:
```
          git log --pretty=format:"%h" {numpy_SHA}..slicer-xyz-{numpy_SHA}
```

4. To **test the changes**, locally rebuild python-numpy and Slicer.

5. Publish the branch. (directly in this repo if you have push rights, or on a fork)

6. Update Slicer numpy external project and submit a pull request.


How to be granted push rights ?
-------------------------------

Ask on https://discourse.slicer.org/


Questions
---------

If you have questions, see https://discourse.slicer.org/

