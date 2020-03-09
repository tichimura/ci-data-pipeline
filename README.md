# Pipeline your data with CI tools

data processing with ci pipeline for mapping your data sources

## Objective

This is a sample CI project for building data process pipeline.  
There are lots of data format and many tools can be available for data processing, like changing data format, picking up some features, or put your data to your repository or your site.  
However, you need to prepare these tools with proper way in multiple runtime, like java, python, go, ...
Also, you need to secure your compute resource to run these process, and automation will be needed.

This pipeline will reduce your worklaod for such preparation and improve your productivity around data processing.  


## Requirement

- CI tools access
  - currently, only for Circle CI

- VCS(github, bitbucket,...)

- git cli


## Installation

1. [CI] Confirm or Create your account in CI Tools. (option)

1. [CI] Integrate your VCS to your CI tools. (option)
   - for circle CI: integrate it at https://circleci.com/account  
    User Settings -> Account Integration  

    <img src="./assets/Screen Shot 2020-03-06 at 9.20.05 AM.png" width=180px> <img src="./assets/Screen Shot 2020-03-06 at 9.34.23 AM.png" width=240px>

1. [VCS] Create new repository at your repositories.

1. [CLI] Clone this repository to your work environment (laptop/server)

  ```
  git clone https://github.com/tichimura/ci-data-pipeline.git
  ```

1. [CLI] Push the cloned code to your repository which is created in previous steps.

  ```
  git remote remove origin
  git remote add origin URL_TO_YOUR_REPO
  git add .
  git commit -m "first commit"
  git push -u origin master
  ```

1. [CI] Add your repository or project to CI
   - in below case, please click `Set Up Project`

   <img src="./assets/Screen Shot 2020-03-06 at 9.37.47 AM.png" width=240px>

1. [CI] Setup pipeline

  It will create `.circleci/config.yml` file in a new branch called `circleci-project-setup`. ( we will change it later steps. )

  - Push `Start Build` to setup project  
  <img src="./assets/Screen Shot 2020-03-09 at 5.34.21 PM.png" width=240px>  

 - To create new branch for this pipeline, click `Add Config`
  <img src="./assets/Screen Shot 2020-03-09 at 5.37.51 PM.png" width=200px>

1. [CI] Setup will be finished  
  - please confirm the status is `Success`.  
  <img src="./assets/Screen Shot 2020-03-09 at 5.42.27 PM.png" width=200px>

  - You can see new branch is created in your repo  
  <img src="./assets/Screen Shot 2020-03-09 at 5.41.02 PM.png" width=200px>

1. [GIT] Go to `Project Settings` then edit `Environment Variables`
<img src="./assets/Screen Shot 2020-03-10 at 5.59.34 AM.png" width=200px>

1. [Mapbox] Go to account.mapbox.com and create new token.
   - select tileset-related scopes(LIST,READ,WRITE) in Secret scopes

   <img src="./assets/Screen Shot 2020-03-10 at 6.17.44 AM.png" width=200px>

1. [GIT] Add `MAPBOX_ACCESS_TOKEN` with the token in previous steps and `MAPBOX_ACCOUNT_NAME` from your mapbox account.

1. [GIT] change the branch to circleci-project-setup and check `.circleci/config.yml` is there.

1. [GIT] copy `.circleci/config.yml` from your `master` branch, then paste it to `circleci-project-setup` branch.

1. [CI] New Job will start
<img src="./assets/Screen Shot 2020-03-10 at 6.34.33 AM.png" width=200px>


## Setup

1. Edit `input.json` to setup SOURCE_URL and FILENAME, and push it to pipeline repository ( will be `circleci-project-setup`)

1. Edit `sample-recipe.json` to change it to your layer name(`"hello_world"`) and source path(`"data-pipeline-demo"`) for tileset cli configuration  

    ```
    {
      "version": 1,
      "layers": {
        "hello_world": {
          "source": "mapbox://tileset-source/${MAPBOX_ACCOUNT_NAME}/data-pipeline-demo",
          "minzoom": 0,
          "maxzoom": 5
        }
      }
    }
    ```

## Steps in Pipeline

1. By default, it assumes that source file(SOURCE_URL) is in zip file(.zip) which includes shape file(.shp)

1. Retrieve zip file from SOURCE_URL, and unzip it.

1. Change shapefile to geojson with `ogr2ogr`. May modify geojson with tippecanoe.

1. upload data source(add-source) and create, then publish with tileset cli.


## Reference

- [ogr2ogr](https://gdal.org/programs/ogr2ogr.html)
- [tippecanoe](https://github.com/mapbox/tippecanoe)
- [tileset cli](https://github.com/mapbox/tilesets-cli/)
- [tileset api](https://docs.mapbox.com/help/tutorials/get-started-tilesets-api-and-cli/)
