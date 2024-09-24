---
title: Run Jekyll locally on NixOS
author: Wout
date: 2024-09-23 22:00:00 +0200
categories: [NixOS, Jekyll]
tags: [NixOS, Jekyll, Blogging]
---

## Setup
* PC: Lenovo T590
* OS: NixOS 24.05
* Jekyll theme: [Chirpy](https://github.com/cotes2020/jekyll-theme-chirpy)

## Summary
Run Jekyll locally on NixOS.

## Method 1 [Deprecated]
Go to the root directory of your Jekyll site and start an interactive shell containing the packages `Jekyll` and `bundler`.
```console
nix-shell -p jekyll bundler
```
Install the gems listed in `Gemfile`.
```console
bundle install
```
Start the Jekyll site locally.
```console
bundle exec jekyll serve
```

When I switched from NixOS 23.11 to NixOS 24.05, his method no longer worked. Instead, I got the following error:
```console
Configuration file: /home/wout/Documents/WoutSwinkels.github.io/_config.yml
            Source: /home/wout/Documents/WoutSwinkels.github.io
       Destination: /home/wout/Documents/WoutSwinkels.github.io/_site
 Incremental build: disabled. Enable with --incremental
      Generating... 
  Conversion error: Jekyll::Converters::Scss encountered an error while converting 'assets/css/jekyll-theme-chirpy.scss':
                    Broken pipe
/home/wout/.local/share/gem/ruby/3.1.0/gems/sass-embedded-1.79.3/lib/sass/compiler/connection.rb:61: warning: Could not start dynamically linked executable: /home/wout/.local/share/gem/ruby/3.1.0/gems/sass-embedded-1.79.3/ext/sass/dart-sass/src/dart
/home/wout/.local/share/gem/ruby/3.1.0/gems/sass-embedded-1.79.3/lib/sass/compiler/connection.rb:61: warning: NixOS cannot run dynamically linked executables intended for generic
/home/wout/.local/share/gem/ruby/3.1.0/gems/sass-embedded-1.79.3/lib/sass/compiler/connection.rb:61: warning: linux environments out of the box. For more information, see:
/home/wout/.local/share/gem/ruby/3.1.0/gems/sass-embedded-1.79.3/lib/sass/compiler/connection.rb:61: warning: https://nix.dev/permalink/stub-ld
                    ------------------------------------------------
      Jekyll 4.3.4   Please append `--trace` to the `serve` command 
                     for any additional information or backtrace. 
                    ------------------------------------------------
/home/wout/.local/share/gem/ruby/3.1.0/gems/sass-embedded-1.79.3/lib/sass/compiler/connection.rb:90:in `write': Broken pipe (Errno::EPIPE)
        from /home/wout/.local/share/gem/ruby/3.1.0/gems/sass-embedded-1.79.3/lib/sass/compiler/connection.rb:90:in `block in write'
        from /home/wout/.local/share/gem/ruby/3.1.0/gems/sass-embedded-1.79.3/lib/sass/compiler/connection.rb:89:in `synchronize'
        from /home/wout/.local/share/gem/ruby/3.1.0/gems/sass-embedded-1.79.3/lib/sass/compiler/connection.rb:89:in `write'
        from /home/wout/.local/share/gem/ruby/3.1.0/gems/sass-embedded-1.79.3/lib/sass/compiler/dispatcher.rb:92:in `send_proto'
        from /home/wout/.local/share/gem/ruby/3.1.0/gems/sass-embedded-1.79.3/lib/sass/compiler/channel.rb:59:in `send_proto'
        from /home/wout/.local/share/gem/ruby/3.1.0/gems/sass-embedded-1.79.3/lib/sass/compiler/host.rb:220:in `send_message'
        from /home/wout/.local/share/gem/ruby/3.1.0/gems/sass-embedded-1.79.3/lib/sass/compiler/host.rb:79:in `block in compile_request'
...
```

As pointed out by [midchildan](https://discourse.nixos.org/t/setup-github-pages-jekyll-with-a-nix-shell/42323), this fails because a downloaded dependency, `sass-embedded-1.79.2`, runs a native binary, which almost never works on NixOS, as explained in the [Packaging/Binaries](https://nixos.wiki/wiki/Packaging/Binaries) section of the NixOS wiki. Initially, I considered reverting to the Jekyll package from NixOS 23.11, but that seemed like a lazy approach and not future-proof. It would feel like ductaping things together. The proper solution is to use [bundix](https://github.com/nix-community/bundix) to create Nix packages from Gemfiles. Before we do that, let me explain why the previous method no longer works.

If we look at the Jekyll package in NixOS 23.11, and more specifically at the [`Gemfile.lock`](https://github.com/NixOS/nixpkgs/blob/nixpkgs-23.11-darwin/pkgs/applications/misc/jekyll/basic/Gemfile.lock) then you can see that the `jekyll-sass-converter` is version `2.2.0`, and it uses [`sassc`](https://github.com/sass/sassc/), which is a wrapper around [`libsass`](https://sass-lang.com/blog/libsass-is-deprecated/), a now deprecated library. As a result, `sassc` is also deprecated. 

Why does this matter? Well, let's check the [`Gemfile.lock`](https://github.com/NixOS/nixpkgs/blob/nixpkgs-24.05-darwin/pkgs/applications/misc/jekyll/basic/Gemfile.lock) of the Jekyll package in NixOS 24.05. The `jekyll-sass-converter` is now version `3.0.0`, and as stated in its [documentation](https://jekyll-themes.com/jekyll/jekyll-sass-converter), it now uses `sass-embedded` since `sassc` is deprecated. And guess what? [`sass-embedded`](https://www.npmjs.com/package/sass-embedded) is a wrapper around a native Dart executable, and this native executable is the reason why the previous method no longer works.

## Method 2
As mentioned in the previous section, we will use `bundix`. To do this, I followed the tutorial of [Matthew Rhone](https://matthewrhone.dev/jekyll-in-nixos). However, I made some minor changes to his approach along the way.

1.  Go to the root directory of your Jekyll site and start an interactive shell containing the packages `bundler` and `bundix`.
    ```console
    nix-shell -p bundler bundix
    ```
2.  Set the path for the Gemfiles to `vendor`.
    ```console
    bundle config set path vendor
    ```
3.  Make sure that bundler doesn't use platform-specific gems.
    ```console
    bundle config set --local force_ruby_platform true
    ```
    If you skip this, you might encounter an error similar to the following in step 8. The solution above is suggested by [ymarkus and jonknapp](https://github.com/nix-community/bundix/issues/88).
    ```console
    error: hash mismatch in fixed-output derivation '/nix/store/q8r80dxq77ajl83jb0bvlrv4ah7viq7n-ffi-1.17.0.gem.drv':
             specified: sha256-YJyHTnZhRULG1IWwV25Cp6OP/N8IZhL5owDE7D/NDRI=
                got:    sha256-UWMOQ0JQeDEcBWynX5Ybs72hZBqzbkStTEVeCw5KIxw=
    error: 1 dependencies of derivation '/nix/store/2c93xzahy79b3xd6yzd7z2g0khlldi79-ruby3.1-ffi-1.17.0.drv' failed to build
    error: 1 dependencies of derivation '/nix/store/536yqbf0z7anv7byd85wh77zipgybag8-GitHubPagesBlog.drv' failed to build
    ```
4.  Package the gem files to `vendor/cache` without installing them to the local install location.
    ```console
    bundle cache --no-install
    ```
5.  Run `bundix` to create Nix packages from the Gemfiles. This command also generates the `gemset.nix` file.
    ```console
    bundix
    ```
6.  The `vendor` directory was only needed for `bundix` and can now be deleted. The same applies to the `bundix` cache directory.
    ```console
    rm -rf vendor
    rm -rf ~/.cache/bundix
    ```
7.  Create a `default.nix` file which sets up the `nix-shell` environment to locally run Jekyll. You can change the name parameters to your liking.
    ```console
    with (import <nixpkgs> {}); let
      env = bundlerEnv {
        name = "YourJekyllSite";
        inherit ruby;
        gemfile = ./Gemfile;
        lockfile = ./Gemfile.lock;
        gemset = ./gemset.nix;
      };
    in
      stdenv.mkDerivation {
        name = "YourJekyllSite";
        buildInputs = [env ruby];

        shellHook = ''
          exec ${env}/bin/jekyll serve --watch
        '';
      }
    ```
8.  Exit the `nix-shell` and enter a new one with the `default.nix` we just created.
    ```console
    exit
    nix-shell
    ```
    If no path is given to the `nix-shell` command then `default.nix` is used. If this doesn't work then explicitly specify `default.nix`.
    ```console
    nix-shell default.nix
    ```
    The Jekyll site is now running locally.

## Resources

1.  Chirpy Jekyll theme GitHub repository [[link](https://github.com/cotes2020/jekyll-theme-chirpy)]
2.  Setup github pages + jekyll with a nix shell [[link](https://discourse.nixos.org/t/setup-github-pages-jekyll-with-a-nix-shell/42323)]
3.  NixOS wiki: Packaging/Binaries [[link](https://nixos.wiki/wiki/Packaging/Binaries)]
4.  Bundix GitHub repository [[link](https://github.com/nix-community/bundix)]
5.  Jekyll's NixOS 23.11 Gemfile.lock [[link](https://github.com/NixOS/nixpkgs/blob/nixpkgs-23.11-darwin/pkgs/applications/misc/jekyll/basic/Gemfile.lock)]
6.  SassC GitHub repository [[link](https://github.com/sass/sassc/)]
7.  LibSass is Deprecated [[link](https://sass-lang.com/blog/libsass-is-deprecated/)]
8.  Jekyll's NixOS 24.05 package [[link](https://github.com/NixOS/nixpkgs/blob/nixpkgs-24.05-darwin/pkgs/applications/misc/jekyll/basic/Gemfile.lock)]
9.  Jekyll Sass Converter documentation [[link](https://jekyll-themes.com/jekyll/jekyll-sass-converter)]
10. npm sass-embedded documentation [[link](https://www.npmjs.com/package/sass-embedded)]
11. Jekyll in NixOS [[link](https://matthewrhone.dev/jekyll-in-nixos)]
12. Bundix GitHub issue #88 [[link](https://github.com/nix-community/bundix/issues/88)]