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

desc "Create a new build in production environment, upload it to Fabric and send it to the client and Kogi groups"
lane :production do
	fabric(configuration: "Release", environment: "Production", groups: ['ClientGroup', 'Kogi'])
end
```
### Gym

You will also need to create a `Gymfile` which will contain some information needed to build and package the app. Create the `Gymfile` running `gym init` and add the following to the file:

```ruby
scheme "AppScheme"
export_method "ad-hoc" # Use ad-hoc or development depending on the provisioning profile you'll be using
clean true
```

### Environment Variables

You need a file called `.env` in your `fastlane` folder that will contain some variables that the actions in the `Fastfile` need. You can use this [file](fastlane/example.env) as an example.

**NOTE:** The `.env` file should not be uploaded to the repository as it contains sensitive information like the Slack webhook url.

### Gitignore

Add the lines that `fastlane` recomments, you can find them [here](https://github.com/fastlane/fastlane/blob/master/fastlane/docs/Gitignore.md). Additionally add the following lines:

```
fastlane/.env 		# The file with the environment variables
fastlane/README.md 	# This file is re-generated each time you run fastlane so we don't need to store it on the repository
```

## Usage

To send a new build of the app just run the following line in the `Terminal` changing `environment` for the environment you need

```
fastlane environment
```
