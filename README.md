# DocSpring gems

A static gem source on GitHub Pages: https://docspring.github.io/gems/. It serves every `.gem` attached to the
releases listed in `sources.txt`:

- DocSpring's Sorbet build (DocSpring/sorbet, tags `docspring-*`), which adds the rewriter plugins from
  [DocSpring/sorbet-plugins](https://github.com/DocSpring/sorbet-plugins).
- DocSpring's ironpress build (DocSpring/ironpress, tags `docspring-*`): upstream fixes that aren't released yet.
  What it changes is in its `docspring/README.md`.

```ruby
source 'https://docspring.github.io/gems/' do
  gem 'sorbet'
  gem 'sorbet-static'
  gem 'sorbet-static-and-runtime'
end
```

List all three: Tapioca and Spoom depend on `sorbet-static-and-runtime`, which pins `sorbet` exactly, and a gem
outside the block would come from rubygems.org. Our versions add a fourth part to the upstream version they're based
on (`0.6.13583.1`), so they never collide with an upstream release.

## Publishing

The repository holds no gems. The Publish workflow downloads them from the releases, runs `gem generate_index`
(the compact index Bundler reads, and the Marshal index `gem install` reads) and deploys the result to Pages. It runs
on every push to `main`; after a new release elsewhere, run it by hand:

```sh
gh workflow run publish.yml --repo DocSpring/gems
```

Never delete a release whose gems a lockfile uses: the next publish drops them from the index. To build the index
locally, `gem install rubygems-generate_index` and run `script/build site`.
