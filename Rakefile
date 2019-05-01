# Add your own tasks in files placed in lib/tasks ending in .rake,
# for example lib/tasks/capistrano.rake, and they will automatically be available to Rake.

require_relative 'config/application'
require 'asciidoctor'
require 'pathname'

Rails.application.load_tasks

task docs: %w[doc/func_spec.html]

rule '.adoc' => '.Radoc' do |t|
  Dir.chdir(Pathname.new(t.name).dirname) do
    sh "R -e 'library(knitr);knit(\"#{Pathname.new(t.source).basename}\", " +
      "\"#{Pathname.new(t.name).basename}\")'"
  end
end

rule '.html' => '.adoc' do |t|
  Dir.chdir(Pathname.new(t.name).dirname) do
    Asciidoctor.convert_file Pathname.new(t.source).basename,
                             backend: :html5,
                             to_file: true,
                             safe: :safe
  end
end
