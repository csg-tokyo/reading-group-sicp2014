CURRENT_DIR = File.dirname(__FILE__)
INPUT_DIR   = File.join(CURRENT_DIR, "common")

DEFAULT_INPUT        = File.join(CURRENT_DIR, "main.tex")
DEFAULT_COLOR_OUTPUT = "summary"
DEFAULT_PRINT_OUTPUT = "summary_print"

ENV['TEXINPUTS'] = "#{CURRENT_DIR}:#{INPUT_DIR}:"

task :default => ['build:color', 'build:print']

namespace :build do
  task :color, [:input, :output] do |t, args|
    args.with_defaults(:input => DEFAULT_INPUT, :output => DEFAULT_COLOR_OUTPUT)
    sh "latexmk -pdf -xelatex -cd -jobname=#{args.output} -use-make #{args.input}"
  end

  task :print, [:input, :output] do |t, args|
    args.with_defaults(:input => DEFAULT_INPUT, :output => DEFAULT_PRINT_OUTPUT)
    pdf_command = "xelatex %O '\\def\\print{}\\input{%S}'"
    sh "latexmk -pdf -dvi- -cd -jobname=#{args.output} -pdflatex=\"#{pdf_command}\" #{args.input}"
  end
end

namespace :view do
  task :color, [:input] do |t, args|
    args.with_defaults(:input => "#{DEFAULT_COLOR_OUTPUT}.pdf")
    sh "evince #{args.input}"
  end

  task :print, [:input] do |t, args|
    args.with_defaults(:input => "#{DEFAULT_PRINT_OUTPUT}.pdf")
    sh "evince #{args.input}"
  end
end

task :clean, [:color_job, :print_job] do |t, args|
  args.with_defaults(:color_job => DEFAULT_COLOR_OUTPUT, :print_job => DEFAULT_PRINT_OUTPUT)
  sh "latexmk -CA -jobname=#{args.color_job}"
  sh "latexmk -CA -jobname=#{args.print_job}"
  sh "find . -name *.aux -or -name *.log -delete"
  sh "rm *.bbl" unless Dir["*.bbl"].empty?
end
