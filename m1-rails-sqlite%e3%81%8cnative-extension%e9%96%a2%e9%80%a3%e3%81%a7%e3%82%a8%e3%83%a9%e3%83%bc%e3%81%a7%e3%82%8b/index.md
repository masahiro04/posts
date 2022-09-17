```metadata
{
    "title": "M1 + Rails + SQLiteがnative extension関連でエラーでる",
    "date": "2022-01-24T00:18:34",
    "categories": "Ruby on Rails, SQLite",
}```

こちらがエラーの詳細

```ruby
 Gem::Ext::BuildError: ERROR: Failed to build gem native extension.

    current directory: /Users/masahirookubo/.rbenv/versions/2.7.2/lib/ruby/gems/2.7.0/gems/sqlite3-1.3.13/ext/sqlite3
/Users/masahirookubo/.rbenv/versions/2.7.2/bin/ruby -I /Users/masahirookubo/.rbenv/versions/2.7.2/lib/ruby/site_ruby/2.7.0 -r ./siteconf20220124-84840-1c5966v.rb extconf.rb
checking for sqlite3.h... yes
checking for pthread_create() in -lpthread... yes
checking for sqlite3_libversion_number() in -lsqlite3... no
sqlite3 is missing. Try 'brew install sqlite3',
'yum install sqlite-devel' or 'apt-get install libsqlite3-dev'
and check your shared library search path (the
location where your sqlite3 shared library is located).
*** extconf.rb failed ***
Could not create Makefile due to some reason, probably lack of necessary
libraries and/or headers.  Check the mkmf.log file for more details.  You may
need configuration options.

Provided configuration options:
	--with-opt-dir
	--without-opt-dir
	--with-opt-include
	--without-opt-include=${opt-dir}/include
	--with-opt-lib
	--without-opt-lib=${opt-dir}/lib
	--with-make-prog
	--without-make-prog
	--srcdir=.
	--curdir
	--ruby=/Users/masahirookubo/.rbenv/versions/2.7.2/bin/$(RUBY_BASE_NAME)
	--with-sqlite3-config
	--without-sqlite3-config
	--with-pkg-config
	--without-pkg-config
	--with-sqlite3-dir
	--without-sqlite3-dir
	--with-sqlite3-include
	--without-sqlite3-include=${sqlite3-dir}/include
	--with-sqlite3-lib
	--without-sqlite3-lib=${sqlite3-dir}/lib
	--with-pthread-dir
	--without-pthread-dir
	--with-pthread-include
	--without-pthread-include=${pthread-dir}/include
	--with-pthread-lib
	--without-pthread-lib=${pthread-dir}/lib
	--with-pthreadlib
	--without-pthreadlib
	--with-sqlite3lib
	--without-sqlite3lib

To see why this extension failed to compile, please check the mkmf.log which can be found here:

  /Users/masahirookubo/.rbenv/versions/2.7.2/lib/ruby/gems/2.7.0/extensions/x86_64-darwin-20/2.7.0/sqlite3-1.3.13/mkmf.log

extconf failed, exit code 1

Gem files will remain installed in /Users/masahirookubo/.rbenv/versions/2.7.2/lib/ruby/gems/2.7.0/gems/sqlite3-1.3.13 for inspection.
Results logged to /Users/masahirookubo/.rbenv/versions/2.7.2/lib/ruby/gems/2.7.0/extensions/x86_64-darwin-20/2.7.0/sqlite3-1.3.13/gem_make.out

  /Users/masahirookubo/.rbenv/versions/2.7.2/lib/ruby/site_ruby/2.7.0/rubygems/ext/builder.rb:92:in `run'
  /Users/masahirookubo/.rbenv/versions/2.7.2/lib/ruby/site_ruby/2.7.0/rubygems/ext/ext_conf_builder.rb:47:in `block in build'
  /Users/masahirookubo/.rbenv/versions/2.7.2/lib/ruby/2.7.0/tempfile.rb:291:in `open'
  /Users/masahirookubo/.rbenv/versions/2.7.2/lib/ruby/site_ruby/2.7.0/rubygems/ext/ext_conf_builder.rb:26:in `build'
  /Users/masahirookubo/.rbenv/versions/2.7.2/lib/ruby/site_ruby/2.7.0/rubygems/ext/builder.rb:158:in `build_extension'
  /Users/masahirookubo/.rbenv/versions/2.7.2/lib/ruby/site_ruby/2.7.0/rubygems/ext/builder.rb:192:in `block in build_extensions'
  /Users/masahirookubo/.rbenv/versions/2.7.2/lib/ruby/site_ruby/2.7.0/rubygems/ext/builder.rb:189:in `each'
  /Users/masahirookubo/.rbenv/versions/2.7.2/lib/ruby/site_ruby/2.7.0/rubygems/ext/builder.rb:189:in `build_extensions'
  /Users/masahirookubo/.rbenv/versions/2.7.2/lib/ruby/site_ruby/2.7.0/rubygems/installer.rb:837:in `build_extensions'
  /Users/masahirookubo/.rbenv/versions/2.7.2/lib/ruby/gems/2.7.0/gems/bundler-2.2.28/lib/bundler/rubygems_gem_installer.rb:66:in `build_extensions'
  /Users/masahirookubo/.rbenv/versions/2.7.2/lib/ruby/gems/2.7.0/gems/bundler-2.2.28/lib/bundler/rubygems_gem_installer.rb:26:in `install'
  /Users/masahirookubo/.rbenv/versions/2.7.2/lib/ruby/gems/2.7.0/gems/bundler-2.2.28/lib/bundler/source/rubygems.rb:199:in `install'
  /Users/masahirookubo/.rbenv/versions/2.7.2/lib/ruby/gems/2.7.0/gems/bundler-2.2.28/lib/bundler/installer/gem_installer.rb:54:in `install'
  /Users/masahirookubo/.rbenv/versions/2.7.2/lib/ruby/gems/2.7.0/gems/bundler-2.2.28/lib/bundler/installer/gem_installer.rb:16:in `install_from_spec'
  /Users/masahirookubo/.rbenv/versions/2.7.2/lib/ruby/gems/2.7.0/gems/bundler-2.2.28/lib/bundler/installer/parallel_installer.rb:186:in `do_install'
  /Users/masahirookubo/.rbenv/versions/2.7.2/lib/ruby/gems/2.7.0/gems/bundler-2.2.28/lib/bundler/installer/parallel_installer.rb:177:in `block in worker_pool'
  /Users/masahirookubo/.rbenv/versions/2.7.2/lib/ruby/gems/2.7.0/gems/bundler-2.2.28/lib/bundler/worker.rb:62:in `apply_func'
  /Users/masahirookubo/.rbenv/versions/2.7.2/lib/ruby/gems/2.7.0/gems/bundler-2.2.28/lib/bundler/worker.rb:57:in `block in process_queue'
  /Users/masahirookubo/.rbenv/versions/2.7.2/lib/ruby/gems/2.7.0/gems/bundler-2.2.28/lib/bundler/worker.rb:54:in `loop'
  /Users/masahirookubo/.rbenv/versions/2.7.2/lib/ruby/gems/2.7.0/gems/bundler-2.2.28/lib/bundler/worker.rb:54:in `process_queue'
  /Users/masahirookubo/.rbenv/versions/2.7.2/lib/ruby/gems/2.7.0/gems/bundler-2.2.28/lib/bundler/worker.rb:91:in `block (2 levels) in create_threads'

An error occurred while installing sqlite3 (1.3.13), and Bundler cannot continue.
```

こちら実行することで解消できました🙌

```ruby
 bundle config build.sqlite3 --with-sqlite3-lib="/usr/lib"
bundle install
```

## 参考記事

[https://github.com/railsinstaller/railsinstaller-windows/issues/111#issuecomment-986998444](https://github.com/railsinstaller/railsinstaller-windows/issues/111#issuecomment-986998444)
