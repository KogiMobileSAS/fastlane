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
brew install graphicsmagick
```

**NOTE:** In case you don't have `Homebrew` installed you can find the instructions in their [page](http://brew.sh/)

## Setup

### Fastlane

To start using `fastlane` run `fastlane init` in your project folder which will start an assistant and set up `fastlane`. For more information check the official [instructions](https://github.com/fastlane/fastlane#quick-start)

After you have `fastlane` configured in your project add to the beggining of the `Fastfile` in the `fastlane` folder the following line:

```ruby
import_from_git(url: 'https://github.com/KogiMobileSAS/fastlane.git')
```

And the following under your platform:

```ruby
desc "Create a new build in test environment, upload it to Fabric and send it to Kogi group"
lane :development do
    fabric(configuration: "AdHoc", environment: "Test", groups: ['Kogi'])
end

desc "Create a new build in test environment, upload it to Fabric and send it to the client and Kogi groups"
lane :testing do
    fabric(configuration: "AdHoc", environment: "Test", groups: ['ClientGroup', 'Kogi'])
end

desc "Create a new build in production environment, upload it to TestFlight (Only upload the build, THIS LANE DON'T DO THE SUBMISSION)"
lane :uploadtestflight do
    itunesconnect(configuration: "AdHoc", environment: "Test")
end

desc "Create a new build in production environment, upload it to iTunes Connect (Only upload the build, THIS LANE DON'T DO THE SUBMISSION)"
lane :uploadtostore do
    itunesconnect(configuration: "Release", environment: "Production")
end

desc "Create a new build in production environment, upload it to Fabric and send it to the client and Kogi groups"
lane :production do
    fabric(configuration: "Release", environment: "Production", groups: ['ClientGroup', 'Kogi'])
end
```
### Gym

You will also need to create a `Gymfile` which will contain some information needed to build and package the app. Create the `Gymfile` running `gym init` and add the following to the file:

```ruby
scheme "AppScheme"
export_method "ad-hoc" # Use ad-hoc, enterprise or development depending on the provisioning profile you'll be using
clean true
use_legacy_build_api true
```

### Environment Variables

You need a file called `.env` in your `fastlane` folder that will contain some variables that the actions in the `Fastfile` need. You can use this [file](fastlane/example.env) as an example.

**NOTE:** The `.env` file should not be uploaded to the repository as it contains sensitive information like the Slack webhook url.

### Automating Version and Build Numbers

In your `FastFile` you will notice that are two lanes to increment Version and Build Numbers, in order to this lanes to works check the project configuration (Build Settings ---> Versioning), use Apple Generic for your Versioning System . 

**NOTE:** you can check [here](https://developer.apple.com/library/mac/qa/qa1827/_index.html) an official resource from Apple documentation .

### Gitignore

Add the lines that `fastlane` recomments, you can find them [here](https://github.com/fastlane/fastlane/blob/master/fastlane/docs/Gitignore.md). Additionally add the following lines:

```
fastlane/.env 		# The file with the environment variables
fastlane/README.md 	# This file is re-generated each time you run fastlane so we don't need to store it on the repository
```

### TestFlight

If the project use TestFlight for beta distributions, you need to take extra care with the provisioning and code signing options in the xcode project, and additionally add the following lines to the AppFile in your project (you can find this file on your fastlane folder):

```
itc_team_name "XXXXXX" #Paste Here your Development Team Name 
```

## Usage

### Increment build

To increment the build number of the app and commit the build bump all you need to do is run in the `Terminal` the following command:

```
fastlane increment_build
```

Optionally you can pass any of the parameters that [`increment_build_number`](https://github.com/fastlane/fastlane/blob/master/fastlane/docs/Actions.md#increment_build_number) receives.

### Increment version

To increment the version number of the app and commit the version bump all you need to do is run in the `Terminal` the following command:

```
fastlane increment_version_number	#The default bump_type is `patch`
```

Optionally you can pass any of the parameters that [`increment_version_number`](https://github.com/fastlane/fastlane/blob/master/fastlane/docs/Actions.md#increment_version_number) receives.

### Create and upload new build

To send a new build of the app just run the following line in the `Terminal` changing `environment` for the environment you need

```
fastlane environment
```
