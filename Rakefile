require 'rake/clean'
require 'json'

BUILDDIR = "build"
SRCDIR = "src"
LIBDIR = "lib"
FUEHRUNGEN_DIR = "#{SRCDIR}/fuehrungen"

CLEAN.include File.join BUILDDIR, '*'
CLEAN.include File.join FUEHRUNGEN_DIR, '*'

TEXFILES = []

CONF = JSON.parse IO.read "conf.json"

DEPS = Hash.new []
DEPS["fuehrungen.pdf"] = (1..CONF["fuehrungen"]["num"].to_i).map { |i| "#{FUEHRUNGEN_DIR}/fuehrung#{i}.tex" }

def make_and_add_to_default_task t, deps=[]
  job = t.gsub '.pdf', ''
  TEXFILES << job
  desc "build #{t}"
  task t => deps do
    compile job, true
  end
end

def compile job, preview = false
  Dir.mkdir BUILDDIR unless Dir.exists? BUILDDIR
  sh %Q[TEXINPUTS=./#{LIBDIR}:$TEXINPUTS latexmk -pdf #{"-pvc" if preview} --jobname="#{File.join BUILDDIR, job}" #{SRCDIR}/#{job}.tex]
end

def make_fuehrung num
  str_fuehrungen = ""
  station_number = 1
  CONF["fuehrungen"]["sections"].each do |section|
    str_fuehrungen += %Q[\\hline
  \\multicolumn{3}{l}{\\large{\\textbf{#{section["name"]}}}} \\\\\\\\
]
    section["stations"].each do |station|
      str_fuehrungen += "\\rowcolor{cyan}" if station_number == num
      str_fuehrungen += %Q[  \\rownumber & #{station["name"]} & #{station["hinweise"]} \\\\\\\\
]
      station_number += 1
    end
  end

  result = IO.read("#{SRCDIR}/fuehrung.textmpl").gsub "{{stations}}", str_fuehrungen

  Dir.mkdir FUEHRUNGEN_DIR unless Dir.exists? FUEHRUNGEN_DIR
  File.open("#{FUEHRUNGEN_DIR}/fuehrung#{num}.tex", "w") do |f|
    f.write result
  end
end

Dir.glob(File.join SRCDIR, "*.tex") do |file|
  t = file.gsub(/^#{SRCDIR}\/(.+)tex$/, '\1pdf')
  make_and_add_to_default_task t, DEPS[t]
end

fuehrung = 1
DEPS["fuehrungen.pdf"].each do |dep|
  task dep do
    make_fuehrung fuehrung
    fuehrung += 1
  end
end

desc "build all tex files in #{SRCDIR}/"
task :default do
  TEXFILES.each do |f|
    compile f
  end
end
