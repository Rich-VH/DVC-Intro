
# Working with DVC

This guide will help you set up a DVC environment, and get your hands dirty with it's usage.

## Setting Up the Virtual Environment

1. **Create a virtual environment:**

```bash
python -m venv practice1
```

2. **Activate the virtual environment:**

On Mac:
```bash
source practice1/bin/activate
```

On Windows:
```bash
practice1\Scripts\activate
```

3. **Initialize a git repository:**

```bash
git init
```

4. **Upgrade pip:**

```bash
pip install --upgrade pip
```

5. **Install necessary packages:**

```bash
pip install numpy pandas ipykernel matplotlib seaborn dvc
```

## Creating and Working with Notebooks

To create a new Jupyter notebook:

On Mac:
```bash
touch EDA.ipynb
```

On Windows:
```bash
type nul > EDA.ipynb
```

## Handling SSL Certificate Errors

In case you encounter SSL certificate errors while downloading datasets, you can bypass them using the following code:

```python
import ssl
ssl._create_default_https_context = ssl._create_unverified_context
```

## Initializing DVC and Adding Data

1. **Initialize DVC:**

```bash
dvc init
```

2. **Load and save the Titanic dataset:**

```python
import seaborn as sns
titanic = sns.load_dataset('titanic')
titanic.to_csv('titanic_dataset.csv', index=False)
```

3. **Add the Titanic dataset to DVC tracking:**

```bash
dvc add titanic_dataset.csv
```

4. **Stage the changes for git:**

```bash
git add titanic_dataset.csv.dvc .gitignore
```

5. **Commit the changes:**

```bash
git commit -m "Adding raw data"
```

## Setting Up Remote Storage

Before setting up remote storage, ensure you have the necessary dependencies installed for your chosen storage provider:

```bash
pip install 'dvc[gdrive]'
pip install 'dvc[s3]'
pip install 'dvc[gcs]'
pip install 'dvc[azure]'
```

1. **Add Google Drive as remote storage:**

```bash
dvc remote add -d remote-storage gdrive://1LrDSmA8mJZ9_mFbD-bkaGREFWE64EatN
```

2. **Commit the remote storage configuration:**

```bash
git add .dvc/config
git commit -m "Setting up remote"
```

3. **Push data to remote storage:**

```bash
dvc push
```

If authentication with Google Drive fails, you can set up a local storage instead:

1. **Add local storage:**

```bash
dvc remote add -d local_remote /Users/ricardo.valdez/Documents/Demo1/local_storage
```

2. **Push data to local storage:**

```bash
dvc push
```

## Retrieving Data from Remote Storage

To simulate retrieving data from remote storage:

1. **Remove local data and cache:**

```bash
rm -rf titanic_dataset.csv
rm -rf .dvc/cache
```

2. **Pull data from remote storage:**

```bash
dvc pull
```

## Making and Tracking Changes

After modifying the dataset (e.g., removing lines), follow these steps:

1. **Add changes to DVC:**

```bash
dvc add titanic_dataset.csv
```

2. **Stage the changes for git:**

```bash
git add titanic_dataset.csv.dvc
```

3. **Commit the changes:**

```bash
git commit -m "Removing lines"
```

4. **Push the changes to remote storage:**

```bash
dvc push
```

## Retrieving Previous Versions

To revert to a previous version of the dataset:

1. **Checkout the previous version of the `.dvc` file:**

```bash
git checkout HEAD^1 titanic_dataset.csv.dvc
```

2. **Update the local data to match the checked out version:**

```bash
dvc checkout
```

3. **Commit the reversion:**

```bash
git commit -m "Reverting changes"
```