require 'rake'
require 'rake/clean'

# You need this: http://developer.yahoo.com/yui/compressor/
COMPRESSOR = 'yuicompressor --type css'

def minify (file, out=nil)
    if !File.exists?(file)
        return false
    end

    if !out
        out = file.clone;
        out[out.length - 4, 4] = '.min.css'
    end

    if !File.exists?(out) || File.mtime(file) > File.mtime(out)
        result = `#{COMPRESSOR} -o '#{out}' '#{file}'`

        if $? != 0
            File.delete(out) rescue nil

            return false
        end
    end

    return true
end

FILES = FileList['**/**.css']
FILES.exclude('**/**.min.css')

CLEAN.include('**/**.min.css')

task :default do
    root = Dir.pwd

    FILES.each {|file|
        Dir.chdir(File.dirname(file))

        puts "Compressing `#{file}`"

        if !minify(File.basename(file))
            puts "Failed to compress `#{file}`"
            exit
        end

        Dir.chdir(root)
    }
end
