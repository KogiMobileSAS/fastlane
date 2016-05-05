# Fastlane

## Installation

To install `fastlane` use the following command:

```
sudo gem install fastlane
```

For more information check the official [instructions](https://github.com/fastlane/fastlane#installation)

## Setup

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

## Usage

To send a new build of the app just run the following line in the `Terminal` changing `environment` for the environment you need

```
fastlane environment
```
