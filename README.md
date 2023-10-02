
# Jaime de Arcos Blog 

You can reach this website at [blog.jaimedearcos.com](blog.jaimedearcos.com)

![](assets/img/blog-image.png)

## Template

Jekyl development blog based on [jekflix-template](https://github.com/thiagorossener/jekflix-template)
 thanks to [@thiagorossener](https://github.com/thiagorossener)

## Run locally

### Install Ruby + Jekyll

```bash
sudo apt-get install ruby-full build-essential zlib1g-dev

echo '# Install Ruby Gems to ~/gems' >> ~/.bashrc
echo 'export GEM_HOME="$HOME/gems"' >> ~/.bashrc
echo 'export PATH="$HOME/gems/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc

gem install jekyll bundler
```

### Building site locally

```bash
bundle install
bundle exec jekyll serve
```