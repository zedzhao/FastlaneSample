# Customise this file, documentation can be found here:
# https://github.com/fastlane/fastlane/tree/master/fastlane/docs
# All available actions: https://github.com/fastlane/fastlane/blob/master/fastlane/docs/Actions.md
# can also be listed using the `fastlane actions` command

# Change the syntax highlighting to Ruby
# All lines starting with a # are ignored when running `fastlane`

# If you want to automatically update fastlane if a new version is available:
# update_fastlane

# This is the minimum version number required.
# Update this, if you use features of a newer version

#usage fastlane lanename env:(adding additional param)  //use option[:env] to get the value
# eg. fastlane development_apns env:test_env

default_platform :ios

platform :ios do
    #get build number
  build_number = get_info_plist_value(path: "./Project/Project-Info.plist", key: "CFBundleVersion")
  # get current time
  year = Date.today.year
  day = Date.today.day
  month = Date.today.month
  time = DateTime.now.strftime("%H-%M") #最后输出的文件名会变成 H/M http://stackoverflow.com/questions/13298434/colon-appears-as-forward-slash-when-creating-file-name/13298479#13298479
  date = "#{year}.#{month}.#{day}-#{time}"
  folderDate = "#{year}.#{month}.#{day}"
  env = "prod_env"
  displayEnv  = "生产环境"
  # output_path = "./build/ipa-build/"
  output_path = "./build/ipa-build/#{folderDate}"

  #选择环境
    def getEnvrionment(environment)
      #如果有输入则直接读取输入变量作为环境变量
      if environment != nil
        return environment
      end

      environment = UI.select("Select your environment:", ["Test Envrionment", "Dev Environment", "UAT Environment","Production Environment"])
      case environment
      when "Test Envrionment"
        return ["test_env", "测试环境"]
      when "Dev Environment"
        return ["dev_env", "研发环境"]
      when "UAT Environment"
        return ["uat_env", "UAT环境"]
      when "Production Environment"
        return ["prod_env", "生产环境"]
      end
    end

    #gym 创建ipa的过程
    def build(configuration = "Release", outputName = "Project", outputPath =".build/ipa-build")
      gym(scheme: "Project",
          configuration: configuration,
          clean: true,
          output_directory: outputPath,
          output_name: outputName,
          use_legacy_build_api: true,
          include_symbols: false,
          silent: true)
    end



#执行lane之前 做一些环境设置
  before_all do |lane, options|
  env_param = options[:env]

  env,displayEnv = getEnvrionment(env_param)
  puts displayEnv
  sh "./setenv #{env}"

end
  desc "Build TZYJ.ipa with development cert, support APNS, 测试证书"
  lane :development_apns do

    configuration = "Development-APNS-Cert"
    outputName = "TZYJ_#{build_number}_#{date}_#{displayEnv}_测试证书"
    puts outputName
    build(configuration, outputName, output_path)
  end

  desc "Build TZYJ.ipa with development apns cert, support In App Purchase, 带支付功能的证书"
  lane :development_pay do
    build(configuration="Development-Pay-Cert",
    outputName="TZYJ_#{build_number}_#{date}_#{displayEnv}_支付证书",
    outputPath=output_path)
  end

  desc "Build TZYJ.ipa with enterprise apns cert, 企业证书"
  lane :enterprise do
    build(configuration="Enterprise-Cert",
    outputName = "TZYJ_#{build_number}_#{date}_#{displayEnv}_企业证书",
    outputPath = output_path)
  end

  desc "Build TZYJ.ipa with release cert, 发布证书"
  lane :release do
    build(configuration = "Release",
    outputName = "TZYJ_#{displayEnv}_企业证书_#{build_number}_#{date}",
    outputPath = output_path
    )
  end
  # You can define as many lanes as you want

  after_all do |lane|
    # This block is called, only if the executed lane was successful
    clear_derived_data
    sh "pwd"
    sh "open ../#{output_path}"
    # slack(
    #   message: "Successfully deployed new App Update."
    # )
  end

  error do |lane, exception|
    # slack(
    #   message: exception.message,
    #   success: false
    # )
  end
end


# More information about multiple platforms in fastlane: https://github.com/fastlane/fastlane/blob/master/fastlane/docs/Platforms.md
# All available actions: https://github.com/fastlane/fastlane/blob/master/fastlane/docs/Actions.md

# fastlane reports which actions are used
# No personal data is recorded. Learn more at https://github.com/fastlane/enhancer
