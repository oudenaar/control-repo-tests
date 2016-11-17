require 'puppet-lint/tasks/puppet-lint'
require 'metadata-json-lint/rake_task'
require 'puppet_blacksmith/rake_tasks'
PuppetLint.configuration.send('disable_80chars')
PuppetLint.configuration.ignore_paths = ["spec/**/*.pp", "pkg/**/*.pp"]

desc "Validate manifests, templates, and ruby files"
task :validate do
  Dir['manifests/**/*.pp','site/**/manifests/**/**.pp'].each do |manifest|
    sh "puppet parser validate --noop #{manifest}"
  end
  Dir['site/**/spec/**/*.rb','site/**/lib/**/*.rb','scripts/**/**.rb'].each do |ruby_file|
    sh "ruby -c #{ruby_file}" unless ruby_file =~ /spec\/fixtures/
  end
  Dir['site/**/templates/**/*.erb'].each do |template|
    sh "erb -P -x -T '-' #{template} | ruby -c"
  end
  Dir['hieradata/**/*.yaml'].each do |hieradata|
    begin
      YAML.load_file(hieradata)
    rescue => e
      puts "\e[31m" + hieradata + "\e[0m: " + "#{(e.message.match(/on line (\d+)/)[1] + ':') rescue nil} #{e.message}"
    end
  end    
end

task :default => [:validate, :lint]
