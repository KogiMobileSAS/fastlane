# Fastlane

## Installation

To install `fastlane` use the following command:

```
sudo gem install fastlane
```

For more information check the official [instructions](https://github.com/fastlane/fastlane#installation)

You will also need to install the `badge` gem and `graphicsmagick` which are used to add the version to the app icon, to install them use the following commands:

```
sudo gem install badge
sudo gem install watchbuild
brew install graphicsmagick
```

**NOTE:** In case you don't have `Homebrew` installed you can find the instructions in their [page](http://brew.sh/)

## Setup

### Fastlane

To start using `fastlane` run `fastlane init` in your project folder which will start an assistant and set up `fastlane`. For more information check the official [instructions](https://github.com/fastlane/fastlane#quick-start)

After you have `fastlane` configured in your project add to the beginning of the `Fastfile` in the `fastlane` folder the following line:

```ruby
import_from_git(
            url: "https://github.com/KogiMobileSAS/fastlane.git", # The url of the repository to import the Fastfile from.
            branch: "feature/export_method", # The branch to checkout on the repository. Defaults to `HEAD`.
            path: "fastlane/Fastfile_Multitarget" # The path of the Fastfile in the repository. Defaults to `fastlane/Fastfile`.
          )
```
And the following under your platform:

```ruby
  desc "Create a new build in test environment, upload it to Fabric and send it to Kogi group"
    lane :development do
    fabric(configuration: "Debug", 
         environment: "Test", 
         groups: ['Kogi'], 
         scheme: "projectScheme",
         export_method: "development")
  end

  desc "Create a new build in test environment, upload it to Fabric and send it to the client and Kogi groups"
  lane :testing do
    fabric(configuration: "AdHoc", 
         environment: "Test", 
         groups: ['Kogi'], 
         scheme: "projectScheme",
         export_method: "ad-hoc")
  end

  desc "Create a new build in test environment, upload it to Fabric and send it to the client and Kogi groups 
    with an enterprise account"
    lane :enterprise do
    fabric(configuration: "AdHoc", 
         environment: "Test", 
         groups: ['Kogi'], 
         scheme: "projectScheme",
         export_method: "enterprise")
  end

  desc "Create a new build in the production environment, upload it to Fabric and send it to the client and 
    Kogi groups"
    lane :production do
    fabric(configuration: "Release", 
         environment: "Production", 
         groups: ['Kogi'], 
         scheme: "projectScheme",
         export_method: "ad-hoc")
  end

  desc "Create a new build in the production environment, upload it to iTunes Connect (Only upload the build, 
    THIS LANE DON'T DO THE SUBMISSION)"
    lane :uploadtostoreAvail do
    itunesconnect(configuration: "Release", 
            scheme: "projectScheme",
            environment: "Production", 
            export_method: "app-store")
  end

########## INCREMENT LANES ##########
  desc "Build increment scheme"
    lane :buildIncrementProjectscheme do
    buildIncrementOf(scheme: "projectScheme")     
  end
```
### Gym

You will also need to create a `Gymfile` which will contain some information needed to build and package the app. Create the `Gymfile` running `gym init` and add the following to the file:

```ruby
export_method "ad-hoc" # Use ad-hoc, enterprise or development depending on the provisioning profile you'll be using
clean true
```

### Environment Variables

You need a file called `.env` in your `fastlane` folder that will contain some variables that the actions in the `Fastfile` need. You can use this [file](fastlane/example.env) as an example.

**NOTE:** The `.env` file should not be uploaded to the repository as it contains sensitive information like the Slack webhook url.

### Automating Version and Build Numbers

In your `FastFile` you will notice that are two lanes to increment Version and Build Numbers, in order to this lanes to works check the project configuration (Build Settings ---> Versioning), use Apple Generic for your Versioning System . 

**NOTE:** you can check [here](https://developer.apple.com/library/mac/qa/qa1827/_index.html) an official resource from Apple documentation.

### Gitignore

Add the lines that `fastlane` recommends, you can find them [here](https://github.com/fastlane/fastlane/blob/master/fastlane/docs/Gitignore.md). Additionally, add the following lines:

```
fastlane/.env         # Don't use it with jenkins
fastlane/README.md    # This file is re-generated each time you run fastlane so we don't need to store it in the repository
```

## Usage

### Increment build

To increment the build number of the app and commit the build bump all you need to do is run in the `Terminal` the following command:

```
fastlane buildIncrement
```

Optionally you can pass any of the parameters that [`increment_build_number`](https://github.com/fastlane/fastlane/blob/master/fastlane/docs/Actions.md#increment_build_number) receives.

### Increment version

To increment the version number of the app and commit the version bump all you need to do is run in the `Terminal` the following command:

```
fastlane buildIncrementProjectscheme    #The default bump_type is `patch`
```

Optionally you can pass any of the parameters that [`increment_version_number`](https://github.com/fastlane/fastlane/blob/master/fastlane/docs/Actions.md#increment_version_number) receives.

### Create and upload new build

To send a new build of the app just run the following line in the `Terminal` changing `environment` for the environment you need

```
fastlane environment
```
### Multiples Xcode Versions 
In some cases, we all have multiples version of Xcode so for supporting that please add this to `FastFile`. Also, notice that Kogi iOS Team have a convention for naming the Xcode. Always keep the latest version as "Xcode" and the others XcodeX.Y

```ruby
xcversion(version: "8.2") # Selects Xcode 8.2
xcode_select "/Applications/Xcode.app"
#xcode_select "/Applications/Xcode 7.3.1.app"
```
