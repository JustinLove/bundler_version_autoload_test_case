Test case to expose a troubling situation with Bundler.

    $ ruby -Iinternal/lib main.rb
    pass

    $ bundle exec ruby main.rb
    main.rb:3:in `<main>': undefined method `imp' for Internal:Module (NoMethodError)

The standard rubygem setup has a lib/gem/version.rb which defines

    module MyGem
      VERSION = "0.0.1"
    end

This file is loaded in the .gemspec  When the gem is referenced by a `path`, Bundler loads the gemspec, which causes `MyGem` to become defined.  Libraries or frameworks (Rails, Guard) which use `const_missing` or other forms of reflection to load or optimize loading will then not trigger loading of the main module because the constant is already defined.
