# TestAgent

This Gem contains some adapters to OpenNebula and SikuliX interfaces to make possible fully automated GUI testing.

## Dependencies

Gem works with sikulix > 1.1.0.3 and OpenNebula ~> 3.8 gems. Of course you need Chef and OpenNebula server available and SikuliX installed.

## Installation

I'm not going to push the gem to RubyGems so you can download it and install locally:

    $ gem install --local TestAgent-0.1.0.gem

## Usage

Common way of usage:
```ruby
require 'TestAgent'
include TestAgent
artifacts = SharedFolder.new "./build-result/"
=> Folder ./build-result/ shared via HTTP on some free port
pool = TestPool.new(
{name: 'server', template: 'opensuse13.2', run_list: 'role[web-server]', options: "{package_url: #{artifacts.get_file_url("serv*")}}"},
{name: 'test-client0', template: 'win8.1'}
)
=> two VM's created: server and test-client0. Chef client bootstrapped on server. Server is configured for role web-server and option package_url set to url of 1-st package starting with serv.
pool.init_vnc_screens('test-client0')
=> VNC connection opened for test-client0.
pool['test-client0'].click('start_button.png')
=> Click on start button performed on test-client0 (SikuliX method, see http://www.sikulix.com/).
```