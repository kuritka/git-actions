# runnig workflows

## release

You execute release by calling release action. Target for release package could be specified in `upload_url` property, 
but it is up to you where you want to upload releases. You can release to your custom repo by `push` or whatever of you want.
Extracted `upload_url` (returned by `release`)  in our case looks  like this `https://uploads.github.com/repos/kuritka/git-actions/releases/25416282/assets`

### How to run release manually ?

following is not mandatory, but you should create tag on your master / release branch

```bash
git tag -a v0.0.6 -m "sixth tag."
git push --tags
```

in your repo click on `releases > Draft new release > fill tag (ie. v0.0.1 ) title and comment > publish release` .


## push

following workflow runs push action when `git push` is executed and `tag` is set. Push is great 
when you need test or run linker but no simple release is possible because missing `upload_url` property in response.


```bash
git tag -a v0.0.6 -m "six tag."
git push --tags
```

```bash
on:
  push:
    # Sequence of patterns matched against refs/tags
    tags:
      - 'v*' # Push events to matching v*, i.e. v1.0, v20.15.10

name: Create Release

jobs:
  build:
    name: Create Release
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@master
      - name: Create Release
        id: create_release
        uses: actions/create-release@latest
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }} # This token is provided by Actions, you do not need to create your own token
        with:
          tag_name: ${{ github.ref }}
          release_name: Release ${{ github.ref }}
          body: |
            Changes in this Release
            - First Change
            - Second Change
          draft: false
          prerelease: false
```


## references

 - https://help.github.com/en/actions/reference/events-that-trigger-workflows
 - https://help.github.com/en/actions/configuring-and-managing-workflows/using-environment-variables
 - https://help.github.com/en/github/administering-a-repository/managing-releases-in-a-repository
 