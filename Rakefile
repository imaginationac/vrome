require 'rake'
require 'rubygems'
require 'builder'
require 'json'

desc 'preinstall'
task :preinstall do
  file = File.join(File.dirname(__FILE__),'preinstall.sh')
  `sh #{file}` if File.exist?(file)
end

desc 'build extension'
task :build => [:build_xml] do
  pem_file = File.join(File.dirname(__FILE__),"vimlike-smooziee.pem")
  `ruby #{File.join(File.dirname(__FILE__),'buildex.rb')} --pack-extension=#{File.dirname(__FILE__)} #{File.exist?(pem_file) ? "--pack-extension-key=#{pem_file}" : ""}`
end

desc "build manifest"
task :build_manifest do
  file = File.join(File.dirname(__FILE__),'manifest.json')

  json = JSON.parse(File.read(file))
  json["version"] = File.read('Version').strip
  json["content_scripts"][0]["js"]  = Dir['modules/*.js'].concat(["main.js" ])
  json["content_scripts"][0]["css"] = Dir['styles/*.css']

  File.open(file,'w+') do |f|
    f << json.to_json
  end
end

desc "build xml"
task :build_xml => [:build_manifest] do
  version = JSON.parse(File.read('manifest.json'))['version']
  xml = Builder::XmlMarkup.new( :target => File.open('vimlike-smooziee-updates.xml','w+') , :indent => 2 )
  xml.instruct!
  xml.gupdate(:xmlns => 'http://www.google.com/update2/response',:protocol => '2.0') do |x|
    x.app(:appid => 'iiffmolbankaonfoniihhpbpclcenokk') do |y|
      y.updatecheck(:codebase => 'http://github.com/jinzhu/vrome/raw/master/vrome.crx',:version => version)
    end
  end
end

desc 'install extension'
task :install => [:preinstall,:build] do
  `chromium-browser  #{File.join(File.dirname(__FILE__),'vimlike-smooziee.crx')}`
end
