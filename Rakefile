abort('Please run this using `bundle exec rake`') unless ENV["BUNDLE_BIN_PATH"]
require 'html-proofer'

desc "Test the website"
task :test do
  options = {
    :check_sri => true,
    :check_external_hash => true,
    :check_html => true,
    :check_img_http => true,
    :check_opengraph => true,
    :enforce_https => true,
    :timeout => 10,
    :verbose => true,
    :cache => {
      :timeframe => '6w'
    }
  }
  begin
    HTMLProofer.check_directory("._site/", options).run
  rescue => msg
    puts "#{msg}"
  end
end

task :default => [:test]
